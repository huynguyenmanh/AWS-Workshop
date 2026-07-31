---
title: "Bài viết Blog đã đăng"
date: 2026-07-24
weight: 3
chapter: false
pre: " <b> 3. </b> "
---



Mục này tổng hợp và giới thiệu các bài viết chia sẻ kiến thức kỹ thuật (Tech Blogs) đã được biên soạn và đăng tải lên cộng đồng [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj).

---

### 📌 [Blog 1 - LƯU TRỮ DỮ LIỆU NGAY TẠI ĐỊA PHƯƠNG VỚI AMAZON S3 TRONG AWS LOCAL ZONES](3.1-blog-1/)
Bài viết giới thiệu tính năng lưu trữ Amazon S3 trên AWS Local Zones (đặc biệt là Hanoi Local Zone), giải thích cách giúp các doanh nghiệp Tài chính, Y tế và Cơ quan nhà nước đáp ứng các quy định nghiêm ngặt về vị trí lưu trữ dữ liệu (Data Residency) tại Việt Nam mà vẫn duy trì độ trễ cực thấp và trải nghiệm quản lý S3 API quen thuộc.

---

### 📌 [Blog 2 - CHỦ ĐỘNG QUẢN LÝ MÃ NGUỒN AWS LAMBDA VỚI AMAZON S3 SELF-MANAGED BUCKETS](3.2-blog-2/)
Bài viết phân tích chuyên sâu tính năng Self-Managed Amazon S3 Buckets (chế độ `REFERENCE`) dành cho AWS Lambda. Giải pháp này giúp các hệ thống Serverless quy mô lớn đọc trực tiếp file triển khai `.zip` từ S3 Bucket do người dùng quản lý mà không bị tính vào hạn mức Lambda-managed storage quota, nâng cao tính bảo mật, tối ưu CI/CD pipeline và hỗ trợ Rollback siêu tốc nhờ S3 Versioning.

---

### 📌 [Blog 3 - XỬ LÝ HÀNG TRIỆU BẢN GHI DYNAMODB DỄ DÀNG HƠN VỚI BULK EXECUTOR](3.3-blog-3/)
Bài viết giới thiệu công cụ mã nguồn mở Bulk Executor for Amazon DynamoDB do AWS Labs phát triển. Công cụ này kết hợp giao diện dòng lệnh (CLI) đơn giản với khả năng xử lý tính toán phân tán song song của AWS Glue (Apache Spark) ở phía sau, giúp quản trị viên thực hiện các thao tác hàng loạt như đếm, tìm kiếm, cập nhật, xóa, sao chép hay phân tích trên hàng triệu bản ghi DynamoDB một cách an toàn và hiệu quả.

