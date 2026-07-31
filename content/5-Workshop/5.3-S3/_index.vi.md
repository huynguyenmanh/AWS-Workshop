---
title : "Thiết lập S3 Frontend & CloudFront"
date : 2024-01-01 
weight : 3
chapter : true
pre : " <b> 5.3. </b> "
---

### Mục tiêu của phần này

Trong phần này, chúng ta sẽ xây dựng lớp giao diện người dùng (Frontend) cho ứng dụng Web Serverless. Mục tiêu quan trọng nhất là phải đảm bảo mã nguồn tĩnh được phân phối với tốc độ cao nhất toàn cầu, nhưng S3 Bucket lưu trữ phải được **khóa hoàn toàn khỏi Public Internet** để đảm bảo bảo mật.

Chúng ta sẽ giải quyết bài toán này bằng cách kết hợp **Amazon S3** để lưu trữ tĩnh và **Amazon CloudFront** làm mạng phân phối nội dung (CDN), sử dụng cơ chế bảo mật **OAC (Origin Access Control)**.

### Lộ trình thực hiện

Phần này được chia thành các bước chi tiết sau:

1. **[5.3.1. Khởi tạo Amazon S3 Bucket](5.3.1-create-s3/)** 
   Tạo nơi lưu trữ an toàn (Private) cho mã nguồn Frontend (React/Vite).
2. **[5.3.2. Cấu hình CloudFront Distribution & OAC](5.3.2-config-cloudfront/)** 
   Thiết lập mạng CDN và tạo chứng chỉ OAC để cấp quyền cho CloudFront đọc dữ liệu từ S3.
3. **[5.3.3. Cập nhật S3 Bucket Policy](5.3.3-update-s3-policy/)** 
   Cấu hình chính sách cho S3 Bucket để chính thức cho phép CloudFront truy cập thông qua OAC.
4. **[5.3.4. Build và Upload Source Code Frontend](5.3.4-build-upload-frontend/)** 
   Biên dịch mã nguồn Frontend ở máy local và tải lên S3 Bucket để website chính thức hoạt động.