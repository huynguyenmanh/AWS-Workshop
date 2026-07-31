---
title: "Tạo Lambda function"
date: 2026-07-30
weight: 2
chapter: false
pre: "<b> 5.4.2 </b>"
---

# Tạo Lambda function

Dự án sử dụng một Lambda router để xử lý toàn bộ route backend.

## 1. Tạo execution role

1. Mở **IAM → Roles → Create role**.
2. Ở trusted entity, chọn **AWS service**.
3. Ở use case, chọn **Lambda**.
4. Thêm AWS-managed policy `AWSLambdaBasicExecutionRole`.
5. Đặt tên dễ nhận biết cho role và tạo role.
6. Mở role mới, chọn **Add permissions → Create inline policy**.
7. Chọn JSON editor và dán project policy đã điền placeholder từ
   `iam/lambda-execution-policy.json`.
8. Review và lưu inline policy.


## 2. Tạo function

Mở **AWS Lambda → Functions → Create function**, chọn **Author from scratch**.

Cấu hình khởi đầu đề xuất:

| Cấu hình | Giá trị |
|---|---|
| Function name | `YOUR_FUNCTION_NAME` |
| Runtime | Python 3.14 |
| Architecture | `x86_64` |
| Execution role | Role đã chuẩn bị ở trên |
| Package type | ZIP |

Tạo Lambda ở cùng Region với API Gateway HTTP API. Các SDK client của DynamoDB,
S3 và Cognito sẽ tự dùng Region riêng đã cấu hình.

Backend này không cần kết nối VPC khi gọi các public AWS service endpoint thông
thường. Nếu đưa Lambda vào VPC nhưng không có đường ra hoặc VPC endpoint phù
hợp, Lambda có thể không gọi được Cognito, S3 hoặc DynamoDB.

## 3. Tải backend package lên

Mở function, chọn **Code → Upload from → .zip file** và tải lên:

```text
todo-backend.zip
```

Sau khi tải xong, chọn **Configuration → Runtime settings → Edit** và đặt:

```text
lambda_function.lambda_handler
```

Chọn **Save**.

## 4. Cấu hình biến môi trường

Mở **Configuration → Environment variables → Edit** và thêm:

| Key | Value |
|---|---|
| `TABLE_NAME` | `YOUR_TABLE_NAME` |
| `ATTACHMENTS_BUCKET` | `YOUR_ATTACHMENTS_BUCKET` |
| `USER_POOL_ID` | `YOUR_USER_POOL_ID` |
| `DYNAMODB_REGION` | `YOUR_DYNAMODB_REGION` |
| `S3_REGION` | `YOUR_S3_REGION` |
| `COGNITO_REGION` | `YOUR_COGNITO_REGION` |



Nếu người đọc chọn một Region cho tất cả resource, nhập cùng một Region code
cho cả ba biến. Nếu resource nằm ở nhiều Region, nhập đúng Region của từng
resource.

Nếu source chưa đọc ba biến Region, hãy dừng lại và hoàn thành phần 5.4.1.

## 5. Cấu hình tài nguyên runtime

Trong **Configuration → General configuration**, có thể bắt đầu với:

| Cấu hình | Giá trị đề xuất |
|---|---|
| Memory | 256 MB |
| Timeout | 30 giây |
| Ephemeral storage | Mặc định |

Đây là giá trị khởi đầu, không phải yêu cầu cố định. Xóa tài khoản và import số
lượng lớn cần nhiều thao tác hơn một request profile thông thường, nên timeout
mặc định ba giây của Lambda có thể quá ngắn. Kiểm tra duration trên CloudWatch
trước khi giảm timeout.

## Tài liệu tham khảo

- [Xây dựng Lambda bằng Python](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html)
- [Biến môi trường của Lambda](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html)
