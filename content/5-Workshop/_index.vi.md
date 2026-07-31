---
title: "Workshop"
date: 2026-07-31
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# Xây dựng và triển khai ứng dụng Todo serverless trên AWS

#### Tổng quan

Workshop này hướng dẫn bạn xây dựng và triển khai một ứng dụng web quản lý công việc và ghi chú theo kiến trúc serverless trên AWS. Người dùng có thể đăng ký tài khoản, quản lý hồ sơ, tổ chức công việc bằng trạng thái và danh mục tùy chỉnh, tự động lưu bản nháp, sử dụng bộ lọc, xem thống kê, import hoặc export công việc và tải lên các tệp đính kèm riêng tư.

Kiến trúc của dự án được chia thành các nhóm dịch vụ sau:
+ **Frontend & Authentication** - Lưu frontend tĩnh trong Amazon S3, phân phối nội dung thông qua Amazon CloudFront và sử dụng Amazon Cognito để đăng ký và xác thực người dùng.
+ **API & Compute** - Cung cấp các HTTP endpoint được bảo vệ thông qua Amazon API Gateway và xử lý nghiệp vụ bằng AWS Lambda.
+ **Data & Storage** - Lưu dữ liệu thuộc quyền sở hữu của từng người dùng trong Amazon DynamoDB, đồng thời lưu ảnh đại diện và tệp đính kèm của công việc trong một Amazon S3 bucket riêng tư.
+ **Monitoring & Permissions** - Thu thập log của API và Lambda bằng Amazon CloudWatch, đồng thời kiểm soát quyền truy cập giữa các dịch vụ bằng AWS IAM.

Trong quá trình triển khai, bạn sẽ tự chọn tên tài nguyên và AWS Region phù hợp với môi trường của mình. Frontend nhận JWT từ Cognito và gửi access token đến API Gateway. API Gateway kiểm tra token trước khi gọi Lambda, còn Lambda giao tiếp với DynamoDB, S3 bucket chứa tệp đính kèm và các Cognito administrative API khi cần thiết.

#### Nội dung

1. [Tổng quan Workshop](5.1-Overview)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Deploy frontend lên S3](5.3-S3/)
4. [Deploy backend và storage](5.4-Lambda-Data/)
5. [Kiểm thử](5.5-Testing/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)