---
title : "Tổng quan dự án"
date : 2026-07-31
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Ứng dụng quản lý công việc và ghi chú
+ **Ứng dụng quản lý công việc và ghi chú** là một ứng dụng web serverless giúp người dùng tạo và tổ chức công việc, ghi chú, trạng thái, danh mục, bộ lọc tùy chỉnh, lịch trình và tệp đính kèm.
+ Người dùng đăng ký và đăng nhập thông qua **Amazon Cognito**. Sau khi xác thực, frontend gửi access token do Cognito cấp đến **Amazon API Gateway** trong mỗi API request được bảo vệ.
+ Ứng dụng hỗ trợ quản lý công việc, tự động lưu bản nháp, nhiều chế độ hiển thị, lọc dữ liệu, thống kê, import và export công việc, quản lý hồ sơ và tệp đính kèm riêng tư.

#### Tổng quan kiến trúc
Dự án chia các dịch vụ AWS thành bốn nhóm chức năng.
+ **Frontend & Authentication** sử dụng **Amazon S3** để lưu frontend tĩnh, **Amazon CloudFront** để phân phối nội dung và **Amazon Cognito** để đăng ký và xác thực người dùng.
+ **API & Compute** sử dụng **Amazon API Gateway HTTP API** làm điểm truy cập công khai của backend. API Gateway xác thực JWT của Cognito trước khi gọi hàm Python **AWS Lambda**.
+ **Data & Storage** sử dụng **Amazon DynamoDB** để lưu dữ liệu thuộc quyền sở hữu của từng người dùng và một **Amazon S3** bucket riêng tư để lưu ảnh đại diện cùng tệp đính kèm của công việc.
+ **Monitoring & Permissions** sử dụng **Amazon CloudWatch** để lưu log và metric, đồng thời sử dụng **AWS IAM** để chỉ cấp cho Lambda các quyền cần thiết đối với tài nguyên của dự án.

#### Luồng hoạt động của ứng dụng
1. Người dùng mở ứng dụng trong trình duyệt. Trình duyệt gửi yêu cầu tải frontend đến **Amazon CloudFront**.
2. CloudFront lấy các tệp HTML, CSS và JavaScript cần thiết từ **Amazon S3** bucket chứa frontend rồi trả chúng về trình duyệt.
3. Frontend đang chạy trong trình duyệt giao tiếp với **Amazon Cognito** để đăng ký, xác minh hoặc đăng nhập người dùng. Cognito trả về các JWT sau khi xác thực thành công.
4. Trình duyệt gọi **Amazon API Gateway** và đính kèm Cognito access token trong header `Authorization`.
5. API Gateway kiểm tra JWT dựa trên Cognito issuer, app client audience và public signing key đã được cấu hình.
6. Sau khi xác thực thành công, API Gateway gọi **AWS Lambda** và chuyển các JWT claim đã được kiểm tra cùng request.
7. Lambda xử lý nghiệp vụ và truy cập **Amazon DynamoDB**, **Amazon S3** hoặc các thao tác quản trị Cognito khi cần thiết.
8. Khi upload hoặc download tệp, Lambda trả về presigned URL tạm thời để trình duyệt giao tiếp trực tiếp với S3 bucket chứa tệp đính kèm riêng tư.
9. API Gateway và Lambda gửi log hoạt động cùng metric đến **Amazon CloudWatch**.

Người triển khai tự chọn tên và AWS Region cho các tài nguyên. Sử dụng cùng một Region là cách đơn giản nhất, nhưng vẫn có thể đặt tài nguyên ở nhiều Region nếu từng SDK client và biến môi trường được cấu hình chính xác.


