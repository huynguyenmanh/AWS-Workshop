---
title : "Setup S3 Frontend & CloudFront"
date : 2024-01-01 
weight : 3
chapter : true
pre : " <b> 5.3. </b> "
---

### Objectives of this section

In this section, we will build the user interface (Frontend) layer for the Serverless Web application. The most critical goal is to ensure the static source code is distributed globally with high speed, while the hosting S3 Bucket is **completely locked down from the Public Internet** for security.

We will solve this by combining **Amazon S3** for static hosting and **Amazon CloudFront** as the Content Delivery Network (CDN), utilizing the **OAC (Origin Access Control)** security mechanism.

### Implementation Roadmap

This section is divided into the following detailed steps:

1. **[5.3.1. Create Amazon S3 Bucket](5.3.1-create-s3/)** 
   Create a secure storage (Private) for the Frontend source code (React/Vite).
2. **[5.3.2. Configure CloudFront Distribution & OAC](5.3.2-config-cloudfront/)** 
   Set up the CDN network and create an OAC certificate to grant CloudFront permission to read data from S3.
3. **[5.3.3. Update S3 Bucket Policy](5.3.3-update-s3-policy/)** 
   Configure the policy for the S3 Bucket to officially allow CloudFront access via OAC.
4. **[5.3.4. Build and Upload Frontend Source Code](5.3.4-build-upload-frontend/)** 
   Compile the Frontend source code on your local machine and upload it to the S3 Bucket to make the website live.