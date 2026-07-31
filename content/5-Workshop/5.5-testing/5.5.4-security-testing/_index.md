---
title: "Testing API Gateway JWT Security & CORS"
date: 2026-06-01
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

In this section, we validate the security controls at the **Amazon API Gateway** layer by verifying the **Cognito JWT Authorizer** mechanism (rejecting unauthorized requests) and inspecting **CORS (Cross-Origin Resource Sharing)** headers.

---

#### 1. Testing Unauthorized Access Rejection (401 Unauthorized)

* **Execution Steps:**
  1. Open Postman.
  2. Send a `GET /tasks` HTTP request to the API Gateway invocation URL **without providing an `Authorization` header**.

* **Expected Results:**
  * API Gateway intercepts and rejects the request at the entry layer.
  * Responds with HTTP Status **`401 Unauthorized`**.

![401 Unauthorized Test Result](../../../images/5-Workshop/5.5-testing/06-api-unauthorized.jpg)

---

#### 2. Testing CORS Header Negotiation

* **Execution Steps:**
  1. Send an HTTP **Preflight request (`OPTIONS`)** to the API Gateway endpoint.

* **Expected Results:**
  * API Gateway responds with HTTP Status **`200 OK`** and valid CORS headers: `Access-Control-Allow-Origin`, `Access-Control-Allow-Headers`, `Access-Control-Allow-Methods`.

![CORS Response Headers Verification](../../../images/5-Workshop/5.5-testing/07-cors-headers.jpg)