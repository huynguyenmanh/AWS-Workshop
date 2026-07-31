---
title: "Testing Attachment Uploads via S3 Presigned URLs"
date: 2026-06-01
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

In this section, we validate the image attachment upload feature using **S3 Presigned URLs**. This architecture pattern empowers browser clients to upload files directly to Amazon S3 without routing binary payloads through backend Lambda functions, significantly optimizing bandwidth and compute latency.

---

#### 1. Attachment Upload Execution Steps

1. **Select Attachment File on Web UI:**
   * On the Task/Note creation or edit modal, click the **Attach Image** button.
   * Select an image file (`.png` or `.jpg`) from your local storage.

   ![Select Attachment File](/images/5-Workshop/5.5-testing/select-attachment.png)

2. **Request Presigned URL Generation from Backend:**
   * The web frontend dispatches a `POST /upload-url` request carrying the Cognito JWT Token to API Gateway.
   * API Gateway verifies the token and routes the call to the `GetPresignedUrlHandler` Lambda function.
   * The Lambda function invokes S3 APIs to generate a temporary **S3 Presigned URL** (configured with a 5-minute time-to-live) and returns it to the client.

   ![Presigned URL Generation Response](/images/5-Workshop/5.5-testing/get-presigned-url-response.png)

3. **Direct Binary Upload from Browser to S3:**
   * The browser receives the presigned URL and issues an HTTP `PUT` request transferring the raw image binary directly to the **S3 Attachments Bucket**.
   * The upload completes with HTTP Status **`200 OK`**.

   ![Direct Upload to Amazon S3](/images/5-Workshop/5.5-testing/s3-direct-upload.png)

4. **Verify UI Rendering & S3 Console Object State:**
   * The image thumbnail immediately renders on the task/note card UI.
   * Navigate to the **Amazon S3 Console** -> Open the `Attachments Bucket` -> Confirm that the uploaded file resides under the correct `userId` folder path.

   ![S3 Bucket Console Verification](/images/5-Workshop/5.5-testing/s3-bucket-verification.png)

---

#### 2. S3 Presigned URL Security Verification

* **Expired Presigned URL Test:**
  * Wait 5 minutes (past the Presigned URL expiration threshold) and re-attempt issuing the `PUT` upload request using the old URL.
  * **Expected Result:** Amazon S3 rejects the call returning HTTP **`403 Forbidden (RequestHasExpired)`**.
* **Direct S3 Bucket Access Test:**
  * Attempt accessing the raw S3 Object URL directly in the browser without a signature.
  * **Expected Result:** Since **Block Public Access** is enforced on the S3 bucket, access is denied returning HTTP **`403 AccessDenied`**.