---
title : "Project Overview"
date : 2026-07-31
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Todo and Note application
+ The **Todo and Note application** is a serverless web application that helps users create and organize tasks, notes, statuses, categories, custom filters, schedules, and file attachments.
+ Users register and sign in with **Amazon Cognito**. After authentication, the frontend sends the Cognito access token to **Amazon API Gateway** with each protected API request.
+ The application supports task management, automatic draft saving, multiple task views, filtering, statistics, task import and export, profile management, and private attachments.

#### Architecture overview
The project separates its AWS services into four functional groups.
+ **Frontend & Authentication** uses **Amazon S3** to store the static frontend, **Amazon CloudFront** to deliver it, and **Amazon Cognito** to register and authenticate users.
+ **API & Compute** uses **Amazon API Gateway HTTP API** as the public backend entry point. API Gateway validates Cognito JWTs and invokes the Python **AWS Lambda** function.
+ **Data & Storage** uses **Amazon DynamoDB** to store user-owned application data and a private **Amazon S3** bucket to store profile pictures and task attachments.
+ **Monitoring & Permissions** uses **Amazon CloudWatch** for logs and metrics and **AWS IAM** to give Lambda only the permissions required to access the project resources.

#### Application flow
1. The user opens the application in a browser, which requests the frontend through **Amazon CloudFront**.
2. CloudFront retrieves the required HTML, CSS, and JavaScript files from the frontend **Amazon S3** bucket and returns them to the browser.
3. The frontend running in the browser communicates with **Amazon Cognito** to register, verify, or sign in the user. Cognito returns JWTs after successful authentication.
4. The browser calls **Amazon API Gateway** and includes the Cognito access token in the `Authorization` header.
5. API Gateway validates the JWT using the configured Cognito issuer, app client audience, and public signing keys.
6. After authorization succeeds, API Gateway invokes **AWS Lambda** and forwards the validated JWT claims with the request.
7. Lambda processes the application logic and accesses **Amazon DynamoDB**, **Amazon S3**, or Cognito administrative operations when required.
8. For file uploads and downloads, Lambda returns a temporary presigned URL so the browser can communicate directly with the private attachment bucket.
9. API Gateway and Lambda send operational logs and metrics to **Amazon CloudWatch**.

The person deploying the application chooses the names and AWS Regions of the resources. Using one Region is the simplest setup, while separate Regions can also be used when every SDK client and environment variable is configured correctly.

