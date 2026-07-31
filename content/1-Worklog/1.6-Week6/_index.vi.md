---
title: "Worklog Tuần 6"
date: 2026-07-06
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

Mục tiêu tuần 6:
* Nghiên cứu dịch vụ quản lý định danh Amazon Cognito để xử lý Authentication.
* Thực hành bảo mật Amazon S3 Bucket theo tiêu chuẩn AWS Security Best Practices.

Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu Amazon Cognito: User Pools, Identity Pools, JWT Tokens | 06/07/2026 | 06/07/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Cấu hình Cognito User Pool cho tính năng Đăng ký/Đăng nhập người dùng | 07/07/2026 | 07/07/2026 | FCAJ - Cross-Domain Authentication |
| 4 | - Tích hợp JWT Authorizer vào API Gateway để bảo vệ các REST Endpoint | 08/07/2026 | 08/07/2026 | FCAJ - Single Page Application Auth |
| 5 | - Nghiên cứu S3 Security Best Practices: Block Public Access, KMS Encryption | 09/07/2026 | 09/07/2026 | FCAJ - S3 Security Best Practices |
| 6 | - Thử nghiệm cơ chế Presigned URL trên S3 cho thao tác upload file đính kèm | 10/07/2026 | 10/07/2026 | AWS Documentation |

Kết quả đạt được tuần 6:
* Cấu hình thành công Cognito User Pool cho phép người dùng Đăng ký/Đăng nhập bảo mật.
* Tích hợp thành công JWT Authorizer chặn các request không hợp lệ vào API Gateway.
* Nắm vững phương pháp bảo vệ dữ liệu S3 bằng KMS Encryption và S3 Presigned URL.