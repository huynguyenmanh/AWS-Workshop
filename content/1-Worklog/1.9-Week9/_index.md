---
title: "Week 9 Worklog"
date: 2026-07-27
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:
* Provision Backend Infrastructure and Database layer for Serverless Todo/Note App.
* Develop AWS Lambda functions for Task/Note CRUD business logic and S3 Presigned URLs.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Create DynamoDB Table (`TodoNotesTable`) with PK `userId` and SK `taskId` | 07/27/2026 | 07/27/2026 | Project Architecture |
| 3 | - Create S3 Attachments Bucket for storing task image attachments | 07/28/2026 | 07/28/2026 | Project Architecture |
| 4 | - Develop Lambda Functions for APIs: `CreateTask`, `GetTasks`, `UpdateTask`, `DeleteTask` | 07/29/2026 | 07/30/2026 | AWS SDK Node.js/Python |
| 5 | - Develop Lambda Function generating S3 Presigned URLs for file upload handling | 07/30/2026 | 07/31/2026 | AWS SDK |
| 6 | - Create IAM Execution Roles enforcing Least Privilege principle for Lambda functions | 07/31/2026 | 07/31/2026 | AWS IAM Best Practices |

### Week 9 Achievements:
* Successfully provisioned DynamoDB Table and S3 Attachment storage bucket.
* Developed full suite of backend Lambda functions handling CRUD and Presigned URLs.
* Applied IAM Least-Privilege roles restricting Lambda access to intended resources only.