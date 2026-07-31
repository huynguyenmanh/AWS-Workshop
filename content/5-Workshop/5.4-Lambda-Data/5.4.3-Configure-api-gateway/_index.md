---
title: "Configure API Gateway"
date: 2026-07-30
weight: 3
chapter: false
pre: "<b> 5.4.3 </b>"
---

Use an Amazon API Gateway **HTTP API**, because the Lambda router expects the
HTTP API v2 event structure:

```python
event["routeKey"]
event["requestContext"]["authorizer"]["jwt"]["claims"]
```

## 1. Create the HTTP API

Open **API Gateway → Create API → HTTP API → Build**.

1. Add a Lambda integration.
2. Select the Region and function created in section 5.4.2.
3. Name the API, for example `YOUR_API_NAME`.
4. Use the `$default` stage with automatic deployment, or record the name of
   the explicit stage you create.

When the integration is created through the console, API Gateway can add the
Lambda invoke permission to the function resource policy.
![API](<../../../images/5-Workshop/5.4-Lambda-S3/Screenshot 2026-07-30 173911.png>)
## 2. Create the routes

Point every route below to the same Lambda integration.

For each route, open **Routes → Create**, select the HTTP method, enter the
resource path, and attach the Lambda integration. API Gateway treats the method
and path together as the route key.

| Group | Routes |
|---|---|
| Profile | `GET /profile`, `PATCH /profile`, `POST /profile/avatar/upload-url`, `POST /profile/avatar/confirm`, `DELETE /profile/avatar`, `DELETE /account` |
| Tasks | `POST /tasks`, `GET /tasks`, `GET /tasks/{taskId}`, `PATCH /tasks/{taskId}`, `DELETE /tasks/{taskId}` |
| Attachments | `POST /tasks/{taskId}/attachments/upload-url`, `POST /tasks/{taskId}/attachments/{attachmentId}/confirm`, `GET /tasks/{taskId}/attachments`, `DELETE /tasks/{taskId}/attachments/{attachmentId}` |
| Statuses | `POST /statuses`, `GET /statuses`, `PATCH /statuses/{statusId}`, `DELETE /statuses/{statusId}`, `PATCH /statuses/order` |
| Categories | `POST /categories`, `GET /categories`, `PATCH /categories/{categoryId}`, `DELETE /categories/{categoryId}` |
| Filters | `POST /filters`, `GET /filters`, `PATCH /filters/{filterId}`, `DELETE /filters/{filterId}` |
| Transfer | `POST /imports/preview`, `POST /imports/confirm`, `GET /exports/tasks` |
| Statistics | `GET /statistics`, `GET /statistics/export` |

Use the methods and parameter names exactly. For example,
`{attachmentId}` must not be changed to `{id}` because the Lambda handler reads
the documented parameter name.

The current `openapi.yaml` is documentation only. It does not contain the AWS
integration URI needed to create these integrations automatically.
![Routes](<../../../images/5-Workshop/5.4-Lambda-S3/Screenshot 2026-07-30 173822.png>)
## 3. Create the Cognito JWT authorizer

Open the HTTP API and choose **Authorization → Manage authorizers → Create**.
Configure:

| Setting | Value |
|---|---|
| Authorizer type | JWT |
| Identity source | `$request.header.Authorization` |
| Issuer URL | `https://cognito-idp.YOUR_COGNITO_REGION.amazonaws.com/YOUR_USER_POOL_ID` |
| Audience | `YOUR_APP_CLIENT_ID` |

The audience is the Cognito app client ID, not the user-pool ID. API Gateway
accepts an access token when its `client_id` matches the configured audience
and no `aud` claim is present.

Attach this authorizer to every application route in the table. Do not attach
it to a CORS preflight `OPTIONS` route.

The frontend must send:

```http
Authorization: Bearer COGNITO_ACCESS_TOKEN
```

Use the access token, not the Cognito ID token. The backend passes this token to
Cognito `GetUser`.
![JWT Authorizier](<../../../images/5-Workshop/5.4-Lambda-S3/Screenshot 2026-07-30 173931.png>)
## 4. Configure CORS on the HTTP API

Open **CORS** for the HTTP API and use:

| Setting | Value |
|---|---|
| Allow origins | `https://YOUR_FRONTEND_DOMAIN` |
| Allow methods | `GET`, `POST`, `PATCH`, `DELETE`, `OPTIONS` |
| Allow headers | `Authorization`, `Content-Type` |
| Max age | `3600` |

For local development, add the exact local frontend origin separately, for
example:

```text
http://localhost:5173
```

Do not write a path after the origin. `https://example.com/app` is not an
origin; `https://example.com` is.

API Gateway HTTP API CORS can answer preflight requests before they reach
Lambda. The Lambda `OPTIONS` branch remains harmless but should not be the only
CORS configuration, because successful and error responses also need the CORS
headers applied consistently.
![CORS](<../../../images/5-Workshop/5.4-Lambda-S3/Screenshot 2026-07-30 173959.png>)
## 5. Deploy and record the invoke URL

If using `$default` with automatic deployment, route changes are published
automatically. Otherwise, deploy the named stage after changing routes,
authorization, or CORS.

The base URL is:

```text
https://YOUR_API_ID.execute-api.YOUR_LAMBDA_REGION.amazonaws.com
```

For a named stage:

```text
https://YOUR_API_ID.execute-api.YOUR_LAMBDA_REGION.amazonaws.com/YOUR_STAGE
```

Use this exact base URL as the frontend API environment variable. Do not append
an individual route such as `/profile`.

## References

- [Create routes for HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-develop-routes.html)
- [JWT authorizers for HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html)
- [Configure CORS for HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-cors.html)
