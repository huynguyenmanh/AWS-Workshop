---
title: "Kiểm thử Bảo mật API Gateway JWT Authorizer & CORS"
date: 2026-06-01
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả cảnh báo này.
{{% /notice %}}

Trong phần này, chúng ta sẽ kiểm thử tính an toàn của hệ thống tại tầng **Amazon API Gateway** bằng cách xác minh cơ chế **Cognito JWT Authorizer** (chặn các request không hợp lệ) và kiểm tra cấu hình **CORS (Cross-Origin Resource Sharing)** để đảm bảo Trình duyệt Web có thể tương tác mượt mà với API mà không bị lỗi giao thức cross-origin.

---

#### 1. Kiểm thử Chặn Truy cập Trái phép (Unauthorized Access - 401 Unauthorized)

* **Các bước thực hiện:**
  1. Mở công cụ kiểm thử API (Postman / cURL).
  2. Gửi một HTTP request `GET /tasks` đến URL endpoint của API Gateway nhưng **không truyền Header `Authorization`** (hoặc truyền một chuỗi Token giả lập không hợp lệ).

* **Lệnh cURL kiểm thử:**
  ```bash
  curl -X GET "[https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks](https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks)" \
    -H "Content-Type: application/json"
  ```

* **Kết quả kỳ vọng:**
  * API Gateway lập tức từ chối request ở tầng Gateway mà không cần kích hoạt hàm Lambda backend.
  * Trả về HTTP Status Code **`401 Unauthorized`** kèm thông báo `{"message": "Unauthorized"}`.

![Kiểm thử 401 Unauthorized trên Postman](/images/5-Workshop/5.5-testing/api-unauthorized-test.png)

---

#### 2. Kiểm thử Truy cập Hợp lệ với Cognito JWT Token (Authorized Access - 200 OK)

* **Các bước thực hiện:**
  1. Sử dụng chuỗi **Cognito JWT Access Token** thu được từ bước Đăng nhập (trong LocalStorage).
  2. Gửi lại request `GET /tasks` với Header `Authorization: Bearer <COGNITO_JWT_TOKEN>`.

* **Lệnh cURL kiểm thử:**
  ```bash
  curl -X GET "[https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks](https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks)" \
    -H "Authorization: Bearer eyJraWQiOiJ..." \
    -H "Content-Type: application/json"
  ```

* **Kết quả kỳ vọng:**
  * API Gateway giải mã và xác thực JWT Token thành công qua Cognito User Pool.
  * Request được chuyển tiếp tới hàm Lambda backend xử lý, trả về HTTP Status Code **`200 OK`** cùng dữ liệu JSON chứa danh sách công việc của đúng người dùng đó.

![Kiểm thử 200 OK với JWT Token hợp lệ](/images/5-Workshop/5.5-testing/api-authorized-test.png)

---

#### 3. Kiểm thử Cấu hình CORS (Cross-Origin Resource Sharing)

* **Các bước thực hiện:**
  1. Gửi một HTTP **Preflight request (`OPTIONS`)** đến endpoint `/tasks` để kiểm tra các Header điều kiện do API Gateway trả về cho Trình duyệt Web.

* **Lệnh cURL kiểm thử Preflight:**
  ```bash
  curl -v -X OPTIONS "[https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks](https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks)" \
    -H "Access-Control-Request-Method: POST" \
    -H "Access-Control-Request-Headers: Authorization,Content-Type" \
    -H "Origin: [https://d3jw5vxof6iq0j.cloudfront.net](https://d3jw5vxof6iq0j.cloudfront.net)"
  ```

* **Kết quả kỳ vọng:**
  * API Gateway trả về HTTP Status **`200 OK`** hoặc **`204 No Content`**.
  * Dãy Response Headers bao gồm đầy đủ các cấu hình CORS hợp lệ:
    * `Access-Control-Allow-Origin: *` (hoặc tên miền CloudFront)
    * `Access-Control-Allow-Headers: Content-Type,X-Amz-Date,Authorization,X-Api-Key`
    * `Access-Control-Allow-Methods: GET,POST,PUT,DELETE,OPTIONS`

![Xác minh Response Headers CORS](/images/5-Workshop/5.5-testing/cors-headers-verification.png)