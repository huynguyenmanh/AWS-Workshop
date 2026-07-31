---
title: "5.4.1 Chuẩn bị backend"
date: 2026-07-30
weight: 1
chapter: false
pre: "<b>5.4.1 </b>"
---

# Chuẩn bị backend từ một AWS account mới

Phần này giả sử các resource của dự án chưa tồn tại. Người đọc sẽ tạo DynamoDB
table, S3 bucket riêng tư, Cognito user pool và IAM policy trước khi triển khai
backend.

Các bước sử dụng AWS Management Console. AWS CLI không bắt buộc.

## Trước khi bắt đầu

Đăng nhập bằng một AWS identity có quyền tạo hoặc cấu hình DynamoDB, S3,
Cognito, IAM role và policy, Lambda, API Gateway và CloudWatch Logs. Nếu tài
khoản sinh viên hoặc tài khoản tổ chức hạn chế IAM, quản trị viên có thể tạo
resource thay cho người đọc.

Nên dùng AWS account hoặc môi trường development thay vì một production account
không liên quan. Kiểm tra Region đang chọn trên console trước khi tạo từng
resource.

## 1. Chọn Region và tên resource

AWS Region là vị trí AWS tạo resource. Region selector nằm gần góc trên bên
phải của AWS console.

Để triển khai lần đầu dễ nhất, chọn một Region và tạo DynamoDB, S3, Cognito,
Lambda và API Gateway trong Region đó. Backend cũng hỗ trợ nhiều Region, nhưng
phải ghi lại đúng Region của từng dịch vụ.

Tạo một bảng cấu hình riêng:

| Cấu hình | Giá trị do người đọc chọn |
|---|---|
| AWS account ID | `YOUR_AWS_ACCOUNT_ID` |
| DynamoDB Region | `YOUR_DYNAMODB_REGION` |
| Tên DynamoDB table | `YOUR_TABLE_NAME` |
| S3 Region | `YOUR_S3_REGION` |
| Tên S3 bucket | `YOUR_ATTACHMENTS_BUCKET` |
| Cognito Region | `YOUR_COGNITO_REGION` |
| Cognito user-pool ID | `YOUR_USER_POOL_ID` |
| Cognito app-client ID | `YOUR_APP_CLIENT_ID` |
| Lambda/API Gateway Region | `YOUR_LAMBDA_REGION` |
| Tên Lambda function | `YOUR_FUNCTION_NAME` |
| Frontend origin | `https://YOUR_FRONTEND_DOMAIN` |

Các giá trị viết hoa là placeholder. Thay chúng trong cấu hình AWS, nhưng không
đưa ID thật vào tài liệu được commit lên repository dùng chung.

## 2. Tạo DynamoDB table

1. Chọn `YOUR_DYNAMODB_REGION` trên AWS console.
2. Mở **DynamoDB → Tables → Create table**.
3. Chọn một tên table hợp lệ và ghi lại dưới tên `YOUR_TABLE_NAME`.
4. Đặt **Partition key** là `PK`, kiểu **String**.
5. Đặt **Sort key** là `SK`, kiểu **String**.
6. Chọn capacity mode **On-demand**.
7. Không tạo secondary index. Backend hiện tại không yêu cầu index.
8. Chọn **Create table** và chờ trạng thái **Active**.

Không đổi tên `PK` hoặc `SK`. Source code dùng chính xác hai tên này và có phân
biệt chữ hoa, chữ thường.
![Create Table](<../../../../static/images/5-Workshop/5.4-Lambda-S3/Screenshot 2026-07-30 172625.png>)
## 3. Tạo S3 attachment bucket riêng tư

1. Chọn `YOUR_S3_REGION`.
2. Mở **Amazon S3 → General purpose buckets → Create bucket**.
3. Chọn một tên bucket duy nhất toàn cầu và ghi lại dưới tên
   `YOUR_ATTACHMENTS_BUCKET`.
4. Giữ **Block all public access** ở trạng thái bật.
5. Tắt ACL bằng **Bucket owner enforced**.
6. Bật default encryption hoặc giữ cấu hình mã hóa S3-managed mặc định.
7. Tạo bucket.

Bucket này không phải public website. Lambda tạo temporary signed URL để cho
phép tải lên hoặc tải xuống từng tệp cụ thể.
![Create Bucket](<../../../../static/images/5-Workshop/5.4-Lambda-S3/Screenshot 2026-07-30 172744.png>)
### Cấu hình S3 CORS

Trình duyệt tải tệp trực tiếp lên S3, do đó bucket phải cho phép frontend
origin.

Mở bucket, chọn **Permissions → Cross-origin resource sharing (CORS) → Edit**
và nhập:

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

Xóa local origin khi deploy production nếu không cần. CORS không làm bucket
thành public; IAM và presigned URL vẫn quyết định quyền truy cập.

## 4. Tạo Cognito user pool

1. Chọn `YOUR_COGNITO_REGION`.
2. Mở **Amazon Cognito → User pools → Create user pool**.
3. Chọn email làm sign-in identifier.
4. Yêu cầu xác minh email.
5. Chọn password policy phù hợp với dự án.
6. Bật account recovery bằng verified email.
7. Tạo hoặc đặt tên user pool và ghi lại ID dưới tên `YOUR_USER_POOL_ID`.

Tạo app client cho ứng dụng React chạy trên trình duyệt:

1. Mở phần application hoặc app-client settings của user pool.
2. Chọn loại ứng dụng single-page hoặc public client.
3. Đặt tên cho frontend.
4. Không tạo hoặc thêm client secret. Source code chạy trên browser không thể
   giữ secret bí mật.
5. Bật các authentication flow frontend cần, gồm user password/SRP và
   refresh-token authentication nếu console hiển thị các lựa chọn đó.
6. Ghi app client ID dưới tên `YOUR_APP_CLIENT_ID`.

Frontend cần user-pool ID, app-client ID và Cognito Region. Đây là các giá trị
cấu hình. Không bao giờ đưa app-client secret vào frontend.

## 5. Cho backend hỗ trợ Region tùy chọn

Backend không được giả định tất cả dịch vụ nằm cùng Region với Lambda. Trong
`src/common.py`, thêm:

```python
from botocore.config import Config
```

Đọc tên resource và Region từ biến môi trường:

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

Cấu hình này hoạt động khi resource dùng cùng một Region hoặc nhiều Region.
Không hardcode tên resource hoặc Region do người đọc chọn trong `common.py`.

## 6. Chuẩn bị Lambda execution policy

Mở `iam/lambda-execution-policy.json` và thay:

- `YOUR_AWS_ACCOUNT_ID` bằng account ID của người đọc;
- `YOUR_DYNAMODB_REGION` bằng Region của table;
- `YOUR_TABLE_NAME` bằng tên table;
- `YOUR_ATTACHMENTS_BUCKET` bằng tên S3 bucket;
- `YOUR_COGNITO_REGION` bằng Region của user pool;
- `YOUR_USER_POOL_ID` bằng user-pool ID.

Nếu policy template vẫn dùng một placeholder `YOUR_AWS_REGION`, hãy đổi
DynamoDB ARN sang `YOUR_DYNAMODB_REGION` và Cognito ARN sang
`YOUR_COGNITO_REGION`. S3 ARN không chứa Region.

Custom policy cho phép:

- các action DynamoDB cần thiết cho item, query và batch write;
- quản lý S3 object chỉ trong prefix `users/*`;
- các Cognito administrative operation dùng để kiểm tra tài khoản hoạt động và
  xóa tài khoản.

Execution role của Lambda cũng cần AWS-managed policy
`AWSLambdaBasicExecutionRole` để ghi CloudWatch Logs.

## 7. Kiểm tra và đóng gói source

Gói triển khai phải có `filters.py` mới nhất hỗ trợ `expression` lồng nhau.

Chạy:

```bash
python -m py_compile src/*.py
```

Tạo ZIP sao cho các tệp Python nằm ngay tại thư mục gốc.

PowerShell:

```powershell
Compress-Archive `
  -Path .\src\*.py `
  -DestinationPath .\todo-backend.zip `
  -Force

tar -tf .\todo-backend.zip
```

macOS hoặc Linux:

```bash
cd src
zip -r ../todo-backend.zip *.py
cd ..
unzip -l todo-backend.zip
```

ZIP phải trực tiếp chứa `lambda_function.py`, `common.py`, `tasks.py`,
`attachments.py`, `entities.py`, `filters.py`, `profile_api.py`,
`statistics.py` và `transfer.py`. Lambda handler:

```text
lambda_function.lambda_handler
```

## Tài liệu tham khảo

- [Tạo DynamoDB table](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/getting-started-step-1.html)
- [Tạo S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)
- [Cấu hình S3 CORS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/enabling-cors-examples.html)
- [Cognito app-client settings](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-client-apps.html)
- [Cognito security practices](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-security-best-practices.html)
- [Cấu hình Boto3 Region](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/configuration.html)
