---
title: "Worklog Tuần 9"
date: 2026-07-27
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

Mục tiêu tuần 9:
* Khởi tạo cơ sở hạ tầng Backend và Database cho dự án Serverless Todo/Note App.
* Lập trình các hàm AWS Lambda xử lý nghiệp vụ Task/Note CRUD và Presigned URL.

Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tạo DynamoDB Table (`TodoNotesTable`) với PK `userId` và SK `taskId` | 27/07/2026 | 27/07/2026 | Project Architecture |
| 3 | - Khởi tạo S3 Attachments Bucket lưu trữ ảnh/file đính kèm công việc | 28/07/2026 | 28/07/2026 | Project Architecture |
| 4 | - Viết Lambda Functions xử lý các API: `CreateTask`, `GetTasks`, `UpdateTask`, `DeleteTask` | 29/07/2026 | 30/07/2026 | AWS SDK Node.js/Python |
| 5 | - Viết Lambda Function tạo S3 Presigned URL cho phép Frontend upload ảnh đính kèm | 30/07/2026 | 31/07/2026 | AWS SDK |
| 6 | - Tạo IAM Execution Roles tuân thủ nguyên tắc Least Privilege cho các hàm Lambda | 31/07/2026 | 31/07/2026 | AWS IAM Best Practices |

Kết quả đạt được tuần 9:
* Khởi tạo thành công DynamoDB Table và S3 Bucket lưu trữ file đính kèm.
* Lập trình xong toàn bộ bộ xử lý Backend Lambda CRUD và API cấp Presigned URL.
* Đảm bảo bảo mật IAM Role chỉ cho phép Lambda truy cập đúng tài nguyên DynamoDB/S3 cần thiết.