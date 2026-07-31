---
title: "Worklog Tuần 3"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

Mục tiêu tuần 3:
* Tìm hiểu giải pháp lưu trữ đối tượng Amazon S3 và mạng phân phối nội dung AWS CloudFront.
* Thực hành lưu trữ và phân phối Static Web Application (Frontend Layer).

Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu Amazon S3: Buckets, Objects, Storage Classes & Lifecycle rules | 15/06/2026 | 15/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Cấu hình Amazon S3 Static Website Hosting & phân quyền S3 Bucket Policy | 16/06/2026 | 16/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Tìm hiểu AWS CloudFront (CDN), Edge Locations và cơ chế Cache | 17/06/2026 | 17/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Tích hợp CloudFront với S3 Bucket sử dụng Origin Access Control (OAC) để bảo mật | 18/06/2026 | 18/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - Thử nghiệm upload giao diện Web tĩnh Todo/Note và truy cập qua HTTPS CDN | 19/06/2026 | 19/06/2026 | AWS Documentation |

Kết quả đạt được tuần 3:
* Làm chủ Amazon S3 và hiểu các phương thức bảo mật dữ liệu lưu trữ.
* Đã dựng thành công tầng Frontend Hosting Layer (S3 + CloudFront HTTPS CDN) cho ứng dụng Todo/Note.
* Cấu hình thành công Origin Access Control (OAC) chặn truy cập trực tiếp vào S3 qua Public Internet.