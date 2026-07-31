---
title: "Testing API Gateway JWT Authorizer Security & CORS"
date: 2026-06-01
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

In this section, we validate the security controls at the **Amazon API Gateway** layer by verifying the **Cognito JWT Authorizer** mechanism (rejecting unauthorized requests) and inspecting **CORS (Cross-Origin Resource Sharing)** headers to ensure smooth client-browser API interaction.

---

#### 1. Testing Unauthorized Access Rejection (401 Unauthorized)

* **Execution Steps:**
  1. Open an API testing tool (Postman / cURL).
  2. Send a `GET /tasks` HTTP request to the API Gateway invocation URL **without providing an `Authorization` header** (or providing an invalid/fake token string).

* **cURL Command:**
  ```bash
  curl -X GET "[https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks](https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks)" \
    -H "Content-Type: application/json"
  ```

* **Expected Results:**
  * API Gateway intercepts and rejects the request at the entry layer without triggering backend Lambda invocations.
  * Responds with HTTP Status **`401 Unauthorized`** and payload `{"message": "Unauthorized"}`.

![401 Unauthorized Test in Postman](/images/5-Workshop/5.5-testing/api-unauthorized-test.png)

---

#### 2. Testing Authorized Access with Cognito JWT Token (200 OK)

* **Execution Steps:**
  1. Retrieve the valid **Cognito JWT Access Token** obtained during the login phase (stored in LocalStorage).
  2. Re-issue the `GET /tasks` request attaching the `Authorization: Bearer <COGNITO_JWT_TOKEN>` header.

* **cURL Command:**
  ```bash
  curl -X GET "[https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks](https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks)" \
    -H "Authorization: Bearer eyJraWQiOiJ..." \
    -H "Content-Type: application/json"
  ```

* **Expected Results:**
  * API Gateway successfully decodes and validates the JWT Token against the Cognito User Pool.
  * The call routes to the backend Lambda function, returning HTTP Status **`200 OK`** alongside JSON task payloads belonging strictly to that user identity.

![200 OK Authorized Access Test](/images/5-Workshop/5.5-testing/api-authorized-test.png)

---

#### 3. Testing CORS (Cross-Origin Resource Sharing) Headers

* **Execution Steps:**
  1. Send an HTTP **Preflight request (`OPTIONS`)** to the `/tasks` endpoint to inspect the CORS negotiation headers returned to the browser.

* **cURL Preflight Command:**
  ```bash
  curl -v -X OPTIONS "[https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks](https://api-id.execute-api.us-east-1.amazonaws.com/prod/tasks)" \
    -H "Access-Control-Request-Method: POST" \
    -H "Access-Control-Request-Headers: Authorization,Content-Type" \
    -H "Origin: [https://d3jw5vxof6iq0j.cloudfront.net](https://d3jw5vxof6iq0j.cloudfront.net)"
  ```

* **Expected Results:**
  * API Gateway responds with HTTP Status **`200 OK`** or **`204 No Content`**.
  * The Response Headers contain required CORS policy declarations:
    * `Access-Control-Allow-Origin: *` (or CloudFront distribution origin)
    * `Access-Control-Allow-Headers: Content-Type,X-Amz-Date,Authorization,X-Api-Key`
    * `Access-Control-Allow-Methods: GET,POST,PUT,DELETE,OPTIONS`

![CORS Response Headers Verification](/images/5-Workshop/5.5-testing/cors-headers-verification.png)