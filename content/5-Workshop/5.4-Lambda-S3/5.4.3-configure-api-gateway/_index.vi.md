---
title: "5.4.3 Cấu hình API Gateway"
date: 2026-07-30
weight: 3
chapter: false
pre: "<b>5.4.3 </b>"
---

# Cấu hình API Gateway

Sử dụng Amazon API Gateway **HTTP API** vì Lambda router đang đọc event theo
cấu trúc HTTP API v2:

```python
event["routeKey"]
event["requestContext"]["authorizer"]["jwt"]["claims"]
```

## 1. Tạo HTTP API

Mở **API Gateway → Create API → HTTP API → Build**.

1. Thêm Lambda integration.
2. Chọn Region và function đã tạo ở phần 5.4.2.
3. Đặt tên API, ví dụ `YOUR_API_NAME`.
4. Dùng stage `$default` với automatic deployment, hoặc ghi lại tên stage riêng
   mà bạn tạo.

Khi tạo integration bằng console, API Gateway có thể tự thêm quyền invoke vào
resource policy của Lambda.

## 2. Tạo route

Trỏ tất cả route sau vào cùng một Lambda integration.

Với từng route, mở **Routes → Create**, chọn HTTP method, nhập resource path và
gán Lambda integration. API Gateway kết hợp method và path thành route key.

| Nhóm | Routes |
|---|---|
| Profile | `GET /profile`, `PATCH /profile`, `POST /profile/avatar/upload-url`, `POST /profile/avatar/confirm`, `DELETE /profile/avatar`, `DELETE /account` |
| Tasks | `POST /tasks`, `GET /tasks`, `GET /tasks/{taskId}`, `PATCH /tasks/{taskId}`, `DELETE /tasks/{taskId}` |
| Attachments | `POST /tasks/{taskId}/attachments/upload-url`, `POST /tasks/{taskId}/attachments/{attachmentId}/confirm`, `GET /tasks/{taskId}/attachments`, `DELETE /tasks/{taskId}/attachments/{attachmentId}` |
| Statuses | `POST /statuses`, `GET /statuses`, `PATCH /statuses/{statusId}`, `DELETE /statuses/{statusId}`, `PATCH /statuses/order` |
| Categories | `POST /categories`, `GET /categories`, `PATCH /categories/{categoryId}`, `DELETE /categories/{categoryId}` |
| Filters | `POST /filters`, `GET /filters`, `PATCH /filters/{filterId}`, `DELETE /filters/{filterId}` |
| Transfer | `POST /imports/preview`, `POST /imports/confirm`, `GET /exports/tasks` |
| Statistics | `GET /statistics`, `GET /statistics/export` |

Phải dùng đúng method và tên path parameter. Ví dụ không đổi
`{attachmentId}` thành `{id}` vì Lambda đọc đúng tên parameter trong tài liệu.

`openapi.yaml` hiện tại chỉ dùng để mô tả API. Tệp này chưa có AWS integration
URI để tự động tạo đầy đủ integration.

## 3. Tạo Cognito JWT authorizer

Mở HTTP API, chọn **Authorization → Manage authorizers → Create** và cấu hình:

| Cấu hình | Giá trị |
|---|---|
| Authorizer type | JWT |
| Identity source | `$request.header.Authorization` |
| Issuer URL | `https://cognito-idp.YOUR_COGNITO_REGION.amazonaws.com/YOUR_USER_POOL_ID` |
| Audience | `YOUR_APP_CLIENT_ID` |

Audience là Cognito app client ID, không phải user-pool ID. API Gateway có thể
kiểm tra access token bằng claim `client_id` khi token không có `aud`.

Gán authorizer cho toàn bộ application route trong bảng. Không gán authorizer
cho CORS preflight `OPTIONS`.

Frontend phải gửi:

```http
Authorization: Bearer COGNITO_ACCESS_TOKEN
```

Phải dùng access token, không dùng Cognito ID token. Backend truyền token này
cho Cognito `GetUser`.

## 4. Cấu hình CORS cho HTTP API

Mở **CORS** và dùng:

| Cấu hình | Giá trị |
|---|---|
| Allow origins | `https://YOUR_FRONTEND_DOMAIN` |
| Allow methods | `GET`, `POST`, `PATCH`, `DELETE`, `OPTIONS` |
| Allow headers | `Authorization`, `Content-Type` |
| Max age | `3600` |

Khi phát triển ở máy cá nhân, thêm chính xác local origin, ví dụ:

```text
http://localhost:5173
```

Không thêm path vào origin. `https://example.com/app` không phải origin;
`https://example.com` mới là origin.

CORS của HTTP API có thể xử lý preflight trước khi request tới Lambda. Nhánh
`OPTIONS` trong Lambda vẫn có thể giữ lại nhưng không nên là cấu hình CORS duy
nhất, vì cả response thành công và response lỗi đều cần CORS header nhất quán.

## 5. Deploy và ghi lại invoke URL

Nếu dùng `$default` cùng automatic deployment, route mới sẽ được publish tự
động. Nếu dùng named stage, hãy deploy lại stage sau khi thay route,
authorization hoặc CORS.

Base URL:

```text
https://YOUR_API_ID.execute-api.YOUR_LAMBDA_REGION.amazonaws.com
```

Nếu có named stage:

```text
https://YOUR_API_ID.execute-api.YOUR_LAMBDA_REGION.amazonaws.com/YOUR_STAGE
```

Dùng chính xác base URL này cho biến môi trường API của frontend. Không thêm
route riêng như `/profile`.

## Tài liệu tham khảo

- [Tạo route cho HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-develop-routes.html)
- [JWT authorizer cho HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html)
- [CORS cho HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-cors.html)
