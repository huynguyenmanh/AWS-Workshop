---
title : "Các bước chuẩn bị"
date : 2026-07-31 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### 1. Thiết lập quyền IAM
Để triển khai suôn sẻ và dọn dẹp các tài nguyên trong workshop này, bạn cần gắn IAM Policy dưới đây vào tài khoản AWS User của mình. Policy này đã được tối ưu hóa để chỉ bao gồm các dịch vụ Serverless cần thiết cho dự án.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ServerlessAppWorkshopPermissions",
            "Effect": "Allow",
            "Action": [
                "apigateway:*",
                "lambda:*",
                "dynamodb:*",
                "s3:*",
                "cloudfront:*",
                "cognito-idp:*",
                "cognito-identity:*",
                "logs:*",
                "cloudwatch:*",
                "sns:*",
                "iam:CreateRole",
                "iam:GetRole",
                "iam:PassRole",
                "iam:AttachRolePolicy",
                "iam:DetachRolePolicy",
                "iam:PutRolePolicy",
                "iam:DeleteRolePolicy",
                "iam:CreatePolicy",
                "iam:GetPolicy",
                "iam:DeletePolicy",
                "iam:ListAttachedRolePolicies"
            ],
            "Resource": "*"
        }
    ]
}
```

#### 2. Yêu cầu môi trường & Công cụ
Trước khi đi sâu vào cấu hình AWS, hãy đảm bảo máy tính cá nhân của bạn được trang bị các công cụ nền tảng sau:

* **Node.js (v18+) & npm/pnpm:** Môi trường runtime cần thiết để khởi tạo và build mã nguồn Frontend (React/Vite).
* **Python (3.9+) & pip:** Môi trường để phát triển, cài đặt các thư viện (dependencies) và đóng gói mã nguồn Backend.
* **AWS CLI (v2):** Giao diện dòng lệnh để tương tác với AWS. Chạy lệnh `aws configure` để thiết lập thông tin xác thực của bạn. (Region mặc định được đề xuất: `ap-southeast-1` - Singapore).
* **Git:** Được sử dụng để clone (sao chép) kho lưu trữ mã nguồn của workshop về máy cá nhân của bạn.

#### 3. Tổng quan về Kiến trúc
Trong workshop này, chúng ta sẽ xây dựng một Ứng dụng Web Serverless hoàn chỉnh tại region **Singapore (`ap-southeast-1`)**, bao gồm các thành phần cốt lõi sau:

* **S3 Bucket:** Đóng vai trò là nơi lưu trữ web tĩnh cho Frontend. Bucket này sẽ được bảo mật 100% (Private).
* **CloudFront:** Mạng phân phối nội dung (CDN) kết nối với S3 thông qua OAC (Origin Access Control) để tăng cường tốc độ và bảo mật.
* **API Gateway & AWS Lambda:** Xử lý toàn bộ logic nghiệp vụ (Backend) thông qua các RESTful API, được đóng gói bằng Python.
* **DynamoDB:** Cơ sở dữ liệu NoSQL với độ trễ tính bằng mili-giây, được sử dụng để lưu trữ dữ liệu người dùng và các tác vụ (tasks).
* **CloudWatch:** Trái tim của hệ thống giám sát, thu thập tất cả Log và Metric để khắc phục sự cố theo thời gian thực.
