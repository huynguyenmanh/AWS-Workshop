---
title: "Proposal"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Serverless Todo/Note Web Application on AWS
## A Modern Personal Productivity Platform Powered by AWS Serverless Architecture

---

### 1. Executive Summary
The **Serverless Todo/Note Web Application** project is designed to provide a modern, highly flexible, and secure personal task and note management platform. By fully leveraging **AWS Serverless architecture**, the system automatically scales with traffic, optimizes operational costs to near zero when idle, and completely eliminates the overhead of managing traditional server infrastructure.

---

### 2. Problem Statement

#### The Problem
* **Heavy Infrastructure Overhead:** Traditional task management apps hosted on virtual servers (EC2/VPS) maintain fixed monthly running costs regardless of actual traffic volume.
* **Security & Isolation Risks:** Lack of fine-grained access control mechanisms poses data privacy and leak risks between different users.
* **Inefficient File Attachments:** Uploading images directly through backend API servers causes bandwidth bottlenecks and increases system latency.

#### The Proposed Solution
A full-stack Serverless Web Application built on AWS:
* **Frontend:** Hosted on Amazon S3 and distributed via AWS CloudFront CDN (HTTPS).
* **Authentication:** Amazon Cognito handles user Registration/Login and issues secure JWT Tokens.
* **API & Compute:** Amazon API Gateway integrated with JWT Authorizer routes requests to AWS Lambda functions executing CRUD logic and generating S3 Presigned URLs.
* **Storage:** Amazon DynamoDB stores task/note metadata (NoSQL) while Amazon S3 manages image attachments.

#### Benefits & Return on Investment (ROI)
* **Cost-Efficient:** Maximizes the AWS Free Tier benefits (1M Lambda requests/month, 25 GB DynamoDB, 5 GB S3, 1 TB CloudFront), keeping monthly costs between $0.00 and $0.50 USD.
* **High Availability & Auto Scaling:** Seamlessly scales from 1 to thousands of concurrent users without manual server scaling configuration.

---

### 3. Solution Architecture

The application is structured into 7 core layers as illustrated in the architecture diagram:

![Serverless Todo/Note Architecture](../images/2-Proposal/architecture_diagram.png)

#### AWS Services Used
1. **Client Layer:** User Browser.
2. **Frontend Hosting Layer:**
   * **Amazon S3 (Frontend Static Bucket):** Hosts static web assets (HTML, CSS, JS).
   * **AWS CloudFront:** Global Content Delivery Network providing HTTPS encryption.
3. **Authentication Layer:**
   * **Amazon Cognito:** Manages User Pools, handles Register/Login flows, and issues JWT Tokens.
4. **API Layer:**
   * **Amazon API Gateway:** Entry point for REST HTTP requests validated via JWT Authorizer.
5. **Backend Compute Layer:**
   * **AWS Lambda:** Executes backend CRUD business logic and generates S3 Presigned URLs.
6. **Data Storage Layer:**
   * **Amazon DynamoDB:** NoSQL database storing task data (Primary Key: `userId`, Sort Key: `taskId`).
   * **Amazon S3 (Attachments Bucket):** Securely stores user-uploaded image attachments.
7. **Monitoring, Security & Cost Control Layer:**
   * **Amazon CloudWatch:** Captures execution Logs, tracks Metrics, and triggers Alarms.
   * **AWS IAM:** Enforces access control following the Principle of Least Privilege.
   * **AWS Budgets:** Configures threshold billing alerts for cost monitoring.

---

### 4. Technical Implementation

#### Key Features (8 User Story Epics)
1. **Authentication:** Register, Log In, Log Out, Change Password, Profile Update, Delete Account.
2. **Task & Note Management:** Create/Edit/Delete Tasks, attach images (S3 Presigned URL), Assign Categories/Tags, Auto-save.
3. **Time Organization:** Assign dates, view tasks by Day/Week/Month/Date Range.
4. **Search & Custom Filters:** Title search, filter by Status/Tag/Category, save custom filter presets.
5. **Multiple Views:** List View, Kanban Board, Timeline, and Calendar views.
6. **Custom Workflow:** Customize status columns, assign colors, drag-and-drop cards on Kanban Board.
7. **Import / Export:** Export and import task data via JSON/CSV files or Copy/Paste buffer.
8. **Statistics:** Total task metrics, distribution charts by Category, Status, Tag, and time trends.

---

### 5. Timeline & Milestones

The project is executed across the **12-week internship period** (from **June 1, 2026** to **August 21, 2026**):

* **Month 1 (Weeks 1 - 4):**
  * Learn AWS fundamentals (Console, CLI, EC2, IAM, VPC, S3, DynamoDB NoSQL).
  * Configure AWS Budgets for billing management.
* **Month 2 (Weeks 5 - 8):**
  * Study AWS Lambda, API Gateway, Amazon Cognito, and S3 Security best practices.
  * Setup CloudWatch Alarms and finalize Project Proposal for Mentor review.
  * Publish 3 Tech Blogs on the AWS Study Group community.
* **Month 3 (Weeks 9 - 12):**
  * **Week 9:** Provision DynamoDB Tables, S3 Attachments Bucket, and code Backend Lambda CRUD logic.
  * **Week 10:** Integrate API Gateway JWT Authorizer, connect Cognito Auth, and deploy Frontend to S3 + CloudFront CDN.
  * **Week 11:** Execute End-to-End testing across all 8 User Story Epics, audit IAM Roles, and document Clean-up steps.
  * **Week 12:** Author step-by-step Technical Workshop documentation and finalize Hugo Bilingual Report.

---

### 6. Budget Estimation

All utilized services fall well within the **AWS Free Tier** allocation:
* **AWS Lambda:** 1,000,000 free requests per month $\rightarrow$ **$0.00**
* **Amazon DynamoDB:** 25 GB storage & 25 WCU/RCU free $\rightarrow$ **$0.00**
* **Amazon S3:** 5 GB Standard Storage free $\rightarrow$ **$0.00**
* **AWS CloudFront:** 1 TB Data Transfer Out per month free $\rightarrow$ **$0.00**
* **Amazon Cognito:** 50,000 Monthly Active Users (MAUs) free $\rightarrow$ **$0.00**
* **Amazon API Gateway:** 1,000,000 HTTP API calls free per month $\rightarrow$ **$0.00**

👉 **Total Estimated Monthly Cost:** **$0.00 USD/month** (Max **$0.50 USD/month** for minimal overages).

---

### 7. Risk Assessment

| Risk Description | Severity | Mitigation Strategy |
| :--- | :--- | :--- |
| **CORS Origin Block on API Gateway** | Medium | Configure `Access-Control-Allow-Origin: '*'` headers on both API Gateway Responses and Lambda handlers. |
| **Auth Token Drop / Session Eviction** | Medium | Align Cognito ID Token usage with Authorization Headers stored in Frontend LocalStorage. |
| **Public S3 Data Exposure** | High | Enable S3 Block Public Access, KMS Encryption, and restrict uploads to short-lived Presigned URLs. |
| **Unexpected AWS Billings** | Low | Setup AWS Budgets alerts to trigger email notifications at $1.00 USD threshold. |

---

### 8. Expected Outcomes
* **Functional Deliverable:** A fully working Serverless Todo/Note Web Application deployed on CloudFront HTTPS covering all 8 User Story Epics.
* **Standardized Workshop Guide:** Detailed Step-by-Step technical instructions allowing peers to replicate the deployment end-to-end.
* **Professional Internship Report:** A bilingual (VI/EN) Hugo website fulfilling 100% of FCAJ internship sign-off requirements.