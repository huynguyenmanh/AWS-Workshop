---
title: "Kiểm thử Tải file đính kèm với S3 Presigned URL"
date: 2026-06-01
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả cảnh báo này.
{{% /notice %}}

Trong mục này, chúng ta tiến hành kiểm thử tính năng tải tệp/hình ảnh đính kèm cho ghi chú thông qua **S3 Presigned URL**. Cơ chế này cho phép Trình duyệt Client tải tệp trực tiếp lên Amazon S3 mà không cần đi qua hàm Lambda, giúp tối ưu băng thông và giảm độ trễ xử lý Backend.

---

#### 1. Quy trình Kiểm thử Tải tệp đính kèm

1. **Chọn tệp đính kèm trên Giao diện Web:**
   * Tại form Tạo mới hoặc Chỉnh sửa Task/Note, nhấn vào nút **Attach Image / Tải ảnh lên**.
   * Chọn một tệp hình ảnh định dạng `.png` hoặc `.jpg` từ máy tính cá nhân.

   ![Chọn file đính kèm](/images/5-Workshop/5.5-testing/select-attachment.png)

2. **Yêu cầu sinh Presigned URL từ Backend:**
   * Web Frontend tự động gửi request `POST /upload-url` kèm theo Cognito JWT Token tới API Gateway.
   * API Gateway xác thực Token và chuyển tiếp request đến hàm Lambda `GetPresignedUrlHandler`.
   * Hàm Lambda gọi S3 API để tạo một đường dẫn **S3 Presigned URL** tạm thời (có thời hạn hết hạn trong 5 phút) và trả về cho Frontend.

   ![Request sinh Presigned URL](/images/5-Workshop/5.5-testing/get-presigned-url-response.png)

3. **Upload tệp trực tiếp từ Trình duyệt lên S3:**
   * Trình duyệt nhận Presigned URL và gửi phương thức HTTP `PUT` chứa dữ liệu nhị phân (binary) của tệp ảnh thẳng tới **S3 Attachments Bucket**.
   * Quá trình tải lên thành công với HTTP Status Code **`200 OK`**.

   ![Upload ảnh trực tiếp lên S3](/images/5-Workshop/5.5-testing/s3-direct-upload.png)

4. **Xác nhận Hiển thị & Kiểm tra Tài nguyên S3:**
   * Ảnh đính kèm lập tức được render và hiển thị sắc nét trên thẻ ghi chú của ứng dụng Web.
   * Truy cập **Amazon S3 Console** -> Mở `Attachments Bucket` -> Kiểm tra file vừa tải lên đã xuất hiện trong thư mục tương ứng với `userId`.

   ![Kiểm tra file lưu trữ trong S3 Bucket](/images/5-Workshop/5.5-testing/s3-bucket-verification.png)

---

#### 2. Xác minh Tính Bảo mật của S3 Presigned URL

* **Thử nghiệm mở Presigned URL hết hạn:**
  * Đợi sau 5 phút (thời gian hết hạn mặc định của Presigned URL) và thử gọi lại phương thức `PUT` với URL cũ.
  * **Kết quả kỳ vọng:** Amazon S3 chặn request và trả về lỗi **`403 Forbidden (RequestHasExpired)`**.
* **Thử nghiệm truy cập trực tiếp S3 Bucket:**
  * Thử truy cập trực tiếp S3 Object URL mà không qua Presigned URL.
  * **Kết quả kỳ vọng:** Do S3 Bucket đã được cấu hình **Block Public Access**, request bị từ chối với lỗi **`403 AccessDenied`**.