---
title: "Kiểm thử Xác thực người dùng (Amazon Cognito)"
date: 2026-06-01
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả cảnh báo này.
{{% /notice %}}

Trong phần này, chúng ta sẽ tiến hành kiểm thử luồng Xác thực người dùng (Authentication Flow) sử dụng Amazon Cognito User Pool kết hợp với giao diện Web Single Page App được lưu trữ trên Amazon S3 và phân phối qua AWS CloudFront HTTPS.

#### Các bước thực hiện kiểm thử:

1. **Truy cập ứng dụng Web:**
   * Mở trình duyệt web và truy cập vào đường dẫn tên miền phân phối **CloudFront HTTPS Distribution** (ví dụ: `https://d3jw5vxof6iq0j.cloudfront.net`).

   ![Giao diện ứng dụng CloudFront](/images/5-Workshop/5.5-testing/cloudfront-app.png)

2. **Đăng ký tài khoản mới (Sign Up):**
   * Nhấn vào nút **Sign Up / Đăng ký**.
   * Điền đầy đủ thông tin: Email cá nhân thực tế, Username và Mật khẩu (đảm bảo tuân thủ chính sách mật khẩu của Cognito: chữ hoa, chữ thường, số và ký tự đặc biệt).

   ![Form Đăng ký](/images/5-Workshop/5.5-testing/signup-form.png)

3. **Xác thực mã Confirmation Code:**
   * Kiểm tra hộp thư Email cá nhân để lấy mã xác nhận OTP (Verification Code) 6 chữ số do Amazon Cognito gửi tự động.
   * Nhập mã xác nhận vào giao diện ứng dụng để kích hoạt tài khoản.

   ![Nhập mã OTP Xác thực](/images/5-Workshop/5.5-testing/otp-verification.png)

4. **Đăng nhập hệ thống (Log In):**
   * Nhập Username/Email và Mật khẩu vừa đăng ký để tiến hành đăng nhập.
   * **Kết quả kỳ vọng:**
     * Hệ thống xác thực thành công và trả về chuỗi **Cognito JWT Access Token** bảo mật.
     * Token được tự động lưu trữ an toàn tại **LocalStorage** của trình duyệt để sẵn sàng gắn vào Header `Authorization` cho các HTTP request gọi đến API Gateway ở các bước tiếp theo.

   ![Đăng nhập thành công và JWT Token trong LocalStorage](/images/5-Workshop/5.5-testing/jwt-localstorage.png)