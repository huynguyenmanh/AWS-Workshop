---
title: "5.4.2 Create the Lambda function"
date: 2026-07-30
weight: 2
chapter: false
pre: "<b>5.4.2 </b>"
---

# Create the Lambda function

This project uses one Lambda router for all backend routes.

## 1. Create the execution role

1. Open **IAM → Roles → Create role**.
2. For trusted entity, choose **AWS service**.
3. For use case, choose **Lambda**.
4. Add the AWS-managed `AWSLambdaBasicExecutionRole` policy.
5. Give the role a recognizable name and create it.
6. Open the new role and choose **Add permissions → Create inline policy**.
7. Select the JSON editor and paste the completed project policy based on
   `iam/lambda-execution-policy.json`.
8. Review and save the inline policy.

Check that the DynamoDB ARN uses `YOUR_DYNAMODB_REGION`, the Cognito ARN uses
`YOUR_COGNITO_REGION`, and every resource name matches the worksheet from
section 5.4.1. IAM itself is global, so one execution role can authorize
service calls in multiple Regions.

## 2. Create the function

Open **AWS Lambda → Functions → Create function**, then select **Author from
scratch**.

Recommended starting configuration:

| Setting | Value |
|---|---|
| Function name | `YOUR_FUNCTION_NAME` |
| Runtime | Python 3.14 |
| Architecture | `x86_64` |
| Execution role | Use the role prepared above |
| Package type | ZIP |

Create the function in the same Region as the API Gateway HTTP API. The
DynamoDB, S3, and Cognito SDK clients will use their own configured Regions.

This backend does not need VPC attachment when it accesses the normal public
AWS service endpoints. Adding it to a VPC without the required egress or VPC
endpoints can prevent access to Cognito, S3, or DynamoDB.

## 3. Upload the backend package

Open the function, then choose **Code → Upload from → .zip file** and upload:

```text
todo-backend.zip
```

After the upload completes, choose **Configuration → Runtime settings → Edit**
and set:

```text
lambda_function.lambda_handler
```

Choose **Save**.

## 4. Configure environment variables

Open **Configuration → Environment variables → Edit** and add:

| Key | Value |
|---|---|
| `TABLE_NAME` | `YOUR_TABLE_NAME` |
| `ATTACHMENTS_BUCKET` | `YOUR_ATTACHMENTS_BUCKET` |
| `USER_POOL_ID` | `YOUR_USER_POOL_ID` |
| `DYNAMODB_REGION` | `YOUR_DYNAMODB_REGION` |
| `S3_REGION` | `YOUR_S3_REGION` |
| `COGNITO_REGION` | `YOUR_COGNITO_REGION` |

These identifiers are not application secrets. The function must not receive
AWS access keys; Lambda automatically supplies temporary credentials for its
execution role.

When the reader selected one Region for all resources, enter that same Region
code in all three Region variables. When the resources are split, enter each
resource's actual Region.

If the source has not yet been updated to read the three Region variables, stop
and complete section 5.4.1 before continuing.

## 5. Configure runtime resources

Under **Configuration → General configuration**, start with:

| Setting | Suggested value |
|---|---|
| Memory | 256 MB |
| Timeout | 30 seconds |
| Ephemeral storage | Default |

These are starting values, not fixed requirements. Account deletion and large
imports perform more operations than a normal profile request, so the Lambda
default timeout of three seconds may be too short. Check CloudWatch duration
metrics before reducing the timeout.
## References

- [Building Lambda functions with Python](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html)
- [Working with Lambda environment variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html)
