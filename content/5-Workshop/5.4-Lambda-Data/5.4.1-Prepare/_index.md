---
title: "Prepare the backend"
date: 2026-07-30
weight: 1
chapter: false
pre: "<b> 5.4.1 </b>"
---

This section assumes the project resources do not exist yet. It creates the
DynamoDB table, private S3 bucket, Cognito user pool, and IAM policy required by
the backend.

The instructions use the AWS Management Console. AWS CLI is optional.

## Before starting

Sign in with an AWS identity that is allowed to create or configure DynamoDB,
S3, Cognito, IAM roles and policies, Lambda, API Gateway, and CloudWatch Logs.
An administrator can create the resources on the reader's behalf when a
student or organization account restricts IAM access.

Use a development AWS account or environment rather than an unrelated
production account. Review the selected Region in the console before creating
each resource.

## 1. Choose the Regions and names

An AWS Region is the location in which AWS creates a resource. The Region
selector is displayed near the top-right of the AWS console.

For the simplest first deployment, select one Region and create DynamoDB,
S3, Cognito, Lambda, and API Gateway there. The backend also supports separate
Regions, but each service Region must then be recorded correctly.

Create a private configuration worksheet:

| Setting | Reader's value |
|---|---|
| AWS account ID | `YOUR_AWS_ACCOUNT_ID` |
| DynamoDB Region | `YOUR_DYNAMODB_REGION` |
| DynamoDB table name | `YOUR_TABLE_NAME` |
| S3 Region | `YOUR_S3_REGION` |
| S3 bucket name | `YOUR_ATTACHMENTS_BUCKET` |
| Cognito Region | `YOUR_COGNITO_REGION` |
| Cognito user-pool ID | `YOUR_USER_POOL_ID` |
| Cognito app-client ID | `YOUR_APP_CLIENT_ID` |
| Lambda/API Gateway Region | `YOUR_LAMBDA_REGION` |
| Lambda function name | `YOUR_FUNCTION_NAME` |
| Frontend origin | `https://YOUR_FRONTEND_DOMAIN` |

The uppercase values are placeholders. Replace them in AWS configuration, but
do not replace them with real IDs in documentation committed to a shared
repository.

## 2. Create the DynamoDB table

1. Select `YOUR_DYNAMODB_REGION` in the AWS console.
2. Open **DynamoDB → Tables → Create table**.
3. Enter any valid table name and record it as `YOUR_TABLE_NAME`.
4. Set **Partition key** to `PK` with type **String**.
5. Set **Sort key** to `SK` with type **String**.
6. Choose **On-demand** capacity mode.
7. Leave secondary indexes empty. The current backend does not require one.
8. Choose **Create table** and wait until its status is **Active**.

Do not rename `PK` or `SK`. The source code uses those exact, case-sensitive
attribute names.
![Create Table](<../../../images/5-Workshop/5.4-Lambda-S3/Screenshot 2026-07-30 172625.png>)
## 3. Create the private S3 attachment bucket

1. Select `YOUR_S3_REGION`.
2. Open **Amazon S3 → General purpose buckets → Create bucket**.
3. Enter a globally unique bucket name and record it as
   `YOUR_ATTACHMENTS_BUCKET`.
4. Keep **Block all public access** enabled.
5. Keep ACLs disabled with **Bucket owner enforced** object ownership.
6. Enable default encryption or keep the current S3-managed encryption
   default.
7. Create the bucket.

The bucket is not a public website. Lambda creates temporary signed URLs that
authorize individual uploads and downloads.
![Create Bucket](<../../../images/5-Workshop/5.4-Lambda-S3/Screenshot 2026-07-30 172744.png>)

### Configure S3 CORS

The browser uploads directly to S3, so the bucket must allow the frontend
origin.

Open the bucket, then choose **Permissions → Cross-origin resource sharing
(CORS) → Edit** and enter:

```json
[
  {
    "AllowedHeaders": [
      "Content-Type"
    ],
    "AllowedMethods": [
      "GET",
      "PUT"
    ],
    "AllowedOrigins": [
      "https://YOUR_FRONTEND_DOMAIN",
      "http://localhost:5173"
    ],
    "ExposeHeaders": [
      "ETag"
    ],
    "MaxAgeSeconds": 3600
  }
]
```

Remove the local origin for production if it is not required. CORS does not
make the bucket public; IAM and the presigned URL still control access.

## 4. Create the Cognito user pool

1. Select `YOUR_COGNITO_REGION`.
2. Open **Amazon Cognito → User pools → Create user pool**.
3. Choose email as the sign-in identifier.
4. Require email verification.
5. Select a password policy suitable for the project.
6. Enable account recovery by verified email.
7. Create or name the user pool and record its ID as `YOUR_USER_POOL_ID`.

Create an app client for the React browser application:

1. Open the user pool's application or app-client settings.
2. Choose a single-page or public client application type.
3. Enter a name for the frontend.
4. Do **not** create or add a client secret. Browser source code cannot keep a
   secret confidential.
5. Enable the authentication flows required by the frontend, including user
   password/SRP authentication and refresh-token authentication where exposed
   by the console.
6. Record the app client ID as `YOUR_APP_CLIENT_ID`.

The frontend needs the user-pool ID, app-client ID, and Cognito Region. These
identifiers are configuration values. Never put an app-client secret in
frontend code.

## 5. Make the backend Region-aware

The backend must not assume that every service uses the Lambda Region. In
`src/common.py`, add:

```python
from botocore.config import Config
```

Configure resource names and Regions from environment variables:

```python
TABLE_NAME = required_environment_variable("TABLE_NAME")
ATTACHMENTS_BUCKET = required_environment_variable("ATTACHMENTS_BUCKET")
USER_POOL_ID = required_environment_variable("USER_POOL_ID")

DYNAMODB_REGION = required_environment_variable("DYNAMODB_REGION")
S3_REGION = required_environment_variable("S3_REGION")
COGNITO_REGION = required_environment_variable("COGNITO_REGION")

dynamodb = boto3.resource(
    "dynamodb",
    region_name=DYNAMODB_REGION,
)
table = dynamodb.Table(TABLE_NAME)

s3 = boto3.client(
    "s3",
    region_name=S3_REGION,
    config=Config(signature_version="s3v4"),
)

cognito = boto3.client(
    "cognito-idp",
    region_name=COGNITO_REGION,
)
```

This works whether all resources share one Region or use separate Regions.
Do not hardcode the reader's resource names or Regions in `common.py`.

## 6. Prepare the Lambda execution policy

Open `iam/lambda-execution-policy.json`. Replace:

- `YOUR_AWS_ACCOUNT_ID` with the reader's account ID;
- `YOUR_DYNAMODB_REGION` with the table's Region;
- `YOUR_TABLE_NAME` with the table name;
- `YOUR_ATTACHMENTS_BUCKET` with the S3 bucket name;
- `YOUR_COGNITO_REGION` with the user pool's Region;
- `YOUR_USER_POOL_ID` with the user-pool ID.

If the policy template still uses one `YOUR_AWS_REGION` placeholder, replace
the DynamoDB ARN with `YOUR_DYNAMODB_REGION` and the Cognito ARN with
`YOUR_COGNITO_REGION`. S3 ARNs do not contain a Region.

The custom policy grants:

- the required DynamoDB item, query, and batch-write actions;
- S3 object management only under the bucket's `users/*` prefix;
- Cognito administrative operations used for active-account checks and account
  deletion.

The Lambda execution role will also need the AWS-managed
`AWSLambdaBasicExecutionRole` policy for CloudWatch Logs.

## 7. Verify and package the source

The deployment must contain the latest `filters.py` with recursive
`expression` support.

Run:

```bash
python -m py_compile src/*.py
```

Build the ZIP with Python files at the archive root.

PowerShell:

```powershell
Compress-Archive `
  -Path .\src\*.py `
  -DestinationPath .\todo-backend.zip `
  -Force

tar -tf .\todo-backend.zip
```

macOS or Linux:

```bash
cd src
zip -r ../todo-backend.zip *.py
cd ..
unzip -l todo-backend.zip
```

The archive must directly contain `lambda_function.py`, `common.py`,
`tasks.py`, `attachments.py`, `entities.py`, `filters.py`, `profile_api.py`,
`statistics.py`, and `transfer.py`. The Lambda handler will be:

```text
lambda_function.lambda_handler
```

## References

- [Create a DynamoDB table](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/getting-started-step-1.html)
- [Create an S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)
- [Configure S3 CORS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/enabling-cors-examples.html)
- [Cognito app-client settings](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-client-apps.html)
- [Cognito security practices](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-security-best-practices.html)
- [Boto3 Region configuration](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/configuration.html)
