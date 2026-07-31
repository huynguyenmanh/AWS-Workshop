---
title: "Kiểm thử Xác thực người dùng (Amazon Cognito)"
date: 2026-06-01
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

Trong phần này, chúng ta tiến hành kiểm thử toàn bộ luồng Xác thực người dùng (Authentication Flow) sử dụng **Amazon Cognito User Pool** kết hợp với ứng dụng Web Single Page App được lưu trữ trên Amazon S3 và phân phối qua AWS CloudFront HTTPS.

#### Các bước thực hiện kiểm thử:

1. **Truy cập ứng dụng Web:**
   * Mở trình duyệt web và truy cập vào đường dẫn tên miền **CloudFront HTTPS Distribution** (ví dụ: `https://d3jw5vxof6iq0j.cloudfront.net`).

   ![Giao diện CloudFront App](../../../../images/5-Workshop/5.5-testing/01-cloudfront-app.jpg)

2. **Đăng ký tài khoản mới (Sign Up):**
   * Nhấn vào nút **Sign Up**.
   * Điền đầy đủ thông tin đăng ký bao gồm Email cá nhân thực tế, Username và Mật khẩu (tuân thủ chính sách mật khẩu của Cognito: chữ hoa, chữ thường, số và ký tự đặc biệt).

3. **Xác thực mã Confirmation Code (OTP):**
   * Kiểm tra hộp thư Email cá nhân để lấy mã xác thực OTP 6 chữ số do Amazon Cognito gửi tự động.
   * Nhập mã xác nhận vào giao diện ứng dụng để kích hoạt tài khoản.

4. **Đăng nhập hệ thống (Log In):**
   * Tiến hành đăng nhập bằng Email/Username và Mật khẩu vừa khởi tạo.
   * **Kết quả kỳ vọng:**
     * Hệ thống xác thực thành công và cấp phát chuỗi **Cognito JWT Access Token** bảo mật.
     * Token được tự động lưu trữ an toàn tại **LocalStorage** của trình duyệt để sẵn sàng gắn vào Header `Authorization` cho các REST API request tiếp theo.

   ![Xác thực và Đăng nhập Cognito thành công](../../../../images/5-Workshop/5.5-testing/02-cognito-auth.jpg)