---
title: "Worklog Tuần 10"
date: 2026-08-03
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

Mục tiêu tuần 10:
* Cấu hình API Gateway REST API tích hợp Cognito JWT Authorizer.
* Triển khai giao diện Frontend Web App lên S3 và phân phối qua AWS CloudFront (HTTPS).

Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tạo REST API trên API Gateway và gắn Cognito JWT Authorizer cho tất cả Endpoints | 03/08/2026 | 03/08/2026 | Project Architecture |
| 3 | - Tích hợp API Gateway Resources/Methods với các AWS Lambda Backend tương ứng | 04/08/2026 | 04/08/2026 | Project Architecture |
| 4 | - Kết nối giao diện Frontend Single Page App (React/Vue/HTMLJS) với Cognito SDK | 05/08/2026 | 05/08/2026 | User Stories |
| 5 | - Upload giao diện Frontend hoàn chỉnh lên S3 Frontend Static Bucket | 06/08/2026 | 06/08/2026 | S3 Hosting |
| 6 | - Tạo CloudFront Distribution tích hợp OAC, kích hoạt HTTPS và test CDN | 07/08/2026 | 07/08/2026 | CloudFront Guide |

Kết quả đạt được tuần 10:
* Triển khai hoàn chỉnh tầng API Layer bảo vệ bằng Cognito JWT Authorizer.
* Đăng tải thành công ứng dụng Frontend Web App chạy mượt mà trên CloudFront CDN HTTPS.
* Người dùng có thể Đăng ký, Đăng nhập và gọi API bảo mật trực tiếp từ giao diện Web Browser.