---
title: "Blog 2: Chủ Động Quản Lý Mã Nguồn AWS Lambda Với Amazon S3 Self-Managed Buckets"
date: 2026-07-22
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả cảnh báo này.
{{% /notice %}}

# CHỦ ĐỘNG QUẢN LÝ MÃ NGUỒN AWS LAMBDA VỚI AMAZON S3 SELF-MANAGED BUCKETS

AWS Lambda cho phép lập trình viên chạy mã nguồn mà không phải trực tiếp quản lý máy chủ. Khi triển khai một Lambda function bằng tệp `.zip`, mã nguồn thường được tải lên Amazon S3 trước khi được chuyển đến Lambda.

Trước đây, dù tệp triển khai đã nằm trong S3 bucket của người dùng, Lambda vẫn sao chép tệp đó vào một vùng lưu trữ do dịch vụ quản lý (Lambda-managed storage). Bản sao này được sử dụng để chuẩn bị function khi có yêu cầu thực thi. Cách làm này đơn giản nhưng có thể gây khó khăn khi doanh nghiệp quản lý hàng trăm function và nhiều phiên bản mã nguồn khác nhau.

Với tính năng **Self-Managed Amazon S3 Buckets**, Lambda có thể đọc trực tiếp deployment package từ S3 bucket do chính người dùng quản lý. Thay vì tạo thêm bản sao, Lambda lưu thông tin tham chiếu (reference) đến đúng phiên bản của object trong S3, giúp tối ưu chi phí, nâng cao bảo mật và đơn giản hóa quy trình CI/CD.

---

## 🌟 NHỮNG ĐIỂM NỔI BẬT

1. **Lambda đọc mã nguồn trực tiếp từ S3:** Deployment package trong S3 trở thành nguồn mã triển khai chính thức. Lambda không cần sao chép tệp sang vùng lưu trữ nội bộ của dịch vụ trước khi sử dụng.
2. **Hai chế độ lưu trữ linh hoạt:** * **`COPY` (Mặc định):** Lambda tạo một bản sao độc lập của mã nguồn vào vùng lưu trữ nội bộ.
   * **`REFERENCE` (Mới):** Lambda tham chiếu trực tiếp đến object trong S3 bucket do người dùng quản lý.
3. **Không tính vào hạn mức Lambda storage:** Khi dùng chế độ `REFERENCE`, deployment package không bị tính vào hạn mức dung lượng mã do Lambda quản lý ($75\text{ GB}$ mặc định theo Region).
4. **Quản lý bảo mật chủ động:** Áp dụng trực tiếp S3 Bucket Policy, KMS Encryption, S3 Versioning, Object Lock, Access Logging và Compliance tags lên chính artifact của doanh nghiệp.
5. **Yêu cầu bắt buộc S3 Versioning:** S3 Bucket phải bật **Versioning** để mỗi lần tải package mới, S3 sẽ tạo một Object Version ID riêng biệt. Lambda sẽ tham chiếu đến chính xác Version ID được chọn.
6. **Hỗ trợ Rollback siêu tốc:** Nếu phiên bản mới gặp lỗi, CI/CD pipeline chỉ cần cập nhật Lambda tham chiếu về Object Version ID cũ mà không cần Re-build lại mã nguồn.
7. **Tăng tốc độ triển khai:** Loại bỏ bước sao chép tệp `.zip` nội bộ, giúp việc tạo hoặc cập nhật function diễn ra nhanh hơn, đặc biệt với các gói code kích thước lớn.
8. **Khôi phục sau sự cố (Disaster Recovery):** Kết hợp với **S3 Cross-Region Replication**, nếu Region chính gặp sự cố, Lambda ở Region dự phòng có thể được cập nhật để tham chiếu đến bản sao S3 tương ứng ngay lập tức.

---

## 🔄 SO SÁNH QUY TRÌNH TRIỂN KHAI

### 1. Chế độ truyền thống (`COPY` Mode)
```text
Lập trình viên ──> CI/CD Pipeline ──> S3 Bucket cá nhân ──> Lambda-Managed Storage ──> AWS Lambda Function
```
* **Nhược điểm:** Tệp triển khai bị lưu trùng lặp ở 2 nơi. Không thể áp dụng trực tiếp Lifecycle Policy hay KMS Key riêng cho bản sao nội bộ của Lambda.

### 2. Chế độ mới (`REFERENCE` Mode - Self-Managed S3)
```text
Lập trình viên ──> CI/CD Pipeline ──> S3 Artifact Bucket (Self-Managed) ──> AWS Lambda Function (Read Reference)
```
* **Ưu điểm:** Pipeline chỉ tải tệp lên S3 một lần. Khi cập nhật Lambda, pipeline cung cấp các tham số:
  * `S3Bucket`: Tên bucket lưu trữ.
  * `S3Key`: Đường dẫn object.
  * `S3ObjectVersion`: ID phiên bản cụ thể.
  * `S3ObjectStorageMode=REFERENCE`.

---

## 🏬 TÌNH HUỐNG THỰC TẾ & KIẾN TRÚC THIẾT LẬP

Một hệ thống Thương mại Điện tử Serverless quản lý hàng trăm Lambda Functions (Auth, Product, Orders, Payment, Analytics). Hệ thống áp dụng mô hình quản lý tập trung mã nguồn như sau:

```text
[Git Repository] 
       │
       ▼
[CI/CD Pipeline (Build & Test)]
       │
       ▼
[Amazon S3 Artifact Bucket (Versioning Enabled)] ──(IAM Bucket Policy)──► [AWS Lambda Functions (REFERENCE Mode)]
       │
       ▼
[Amazon CloudTrail / S3 Access Logs]
```

### Chi tiết các thành phần:
* **S3 Artifact Bucket:** Lưu trữ tập trung toàn bộ mã nguồn `.zip`. Bật S3 Versioning và S3 Lifecycle Policy (chuyển phiên bản cũ sau 30 ngày sang Glacier hoặc xóa bỏ).
* **IAM & Bucket Policy:** Cấp quyền `s3:GetObject` và `s3:GetObjectVersion` cho Lambda Execution Service Principal, kết hợp điều kiện `aws:SourceArn` để giới hạn phạm vi truy cập.
* **Theo dõi & Kiểm toán:** Sử dụng CloudTrail / S3 Access Logs để ghi lại chính xác thời điểm Lambda đọc mã nguồn triển khai.

---

## ⚠️ LƯU Ý QUAN TRỌNG KHI SỬ DỤNG

* **S3 Object là Nguồn Mã Duy Nhất:** Không được xóa object hoặc gỡ quyền truy cập S3 khi Lambda Function vẫn đang chạy. Nếu Lambda không thể đọc được object (do bị xóa, khóa KMS bị disable,...) function sẽ chuyển sang trạng thái **`Inactive`**.
* **Phạm vi hỗ trợ:** Chỉ áp dụng cho các Lambda Function triển khai dưới dạng tệp `.zip` (Không áp dụng cho Container Images triển khai qua Amazon ECR).
* **Giới hạn kích thước:** Giới hạn tệp `.zip` sau khi giải nén vẫn tuân theo quy định tối đa $250\text{ MB}$.
* **Chi phí:** Không tốn thêm phí dịch vụ Lambda, nhưng người dùng trả chi phí lưu trữ S3 và chi phí Data Transfer nếu S3 Bucket nằm khác Region với Lambda.

---

## 📝 KẾT LUẬN

Tính năng **Self-Managed Amazon S3 Buckets (REFERENCE Mode)** giúp doanh nghiệp hoàn toàn chủ động trong việc quản lý mã nguồn AWS Lambda. Đây là giải pháp tối ưu cho các hệ thống Serverless quy mô lớn, giúp giảm tải hạn mức lưu trữ, nâng cao tính bảo mật và tối ưu hóa quy trình CI/CD deployment.

🔗 **Link tài liệu tham khảo gốc:** [AWS Compute Blog: Introducing Self-Managed Amazon S3 Buckets for AWS Lambda Function Code](https://aws.amazon.com/blogs/compute/introducing-self-managed-amazon-s3-buckets-for-aws-lambda-function-code/)