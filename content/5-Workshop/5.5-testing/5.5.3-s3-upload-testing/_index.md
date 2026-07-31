---
title: "Testing Attachment Uploads via S3 Presigned URLs"
date: 2026-06-01
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

In this section, we validate the attachment upload feature using **S3 Presigned URLs**. This architectural pattern empowers browser clients to stream image binaries directly to Amazon S3 without routing heavy payloads through backend Lambda functions, significantly reducing compute latency and bandwidth overhead.

---

#### 1. Attachment Upload Execution Steps

1. **Web UI Interaction:**
   * On the Task/Note creation or edit modal, click to select an image attachment (`.png` / `.jpg`).
   * The web frontend automatically dispatches a `POST /upload-url` request carrying the Cognito JWT Token to API Gateway.
   * API Gateway validates the token and triggers the backend Lambda function to return a temporary **S3 Presigned URL** (configured with a 5-minute expiration window).

2. **Direct Binary Stream to S3:**
   * The browser client uses the generated presigned URL to issue an HTTP `PUT` request transferring the raw image binary directly to the **S3 Attachments Bucket**.
   * The upload completes with HTTP Status Code **`200 OK`**.

---

#### 2. Amazon S3 Console Object State Verification

* **Execution Steps:**
  1. Navigate to the **Amazon S3 Console** -> Open the `Attachments Bucket`.
  2. Open the directory path matching the authenticated user's `userId`.
  3. Confirm the presence of the newly uploaded attachment file.

* **Expected Results:**
  * The image file is successfully persisted inside the target S3 folder path with accurate file sizes and metadata.
  * Direct public access without valid presigned signatures remains blocked by bucket security policies.

![S3 Attachment Upload Verification](../../../images/5-Workshop/5.5-testing/05-s3-upload.jpg)