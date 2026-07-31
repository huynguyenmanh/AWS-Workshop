---
title: "Blogs Posted"
date: 2026-07-24
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

This section lists and introduces the technical blogs authored and published to the [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) community.

---

### 📌 [Blog 1 - LOCAL DATA STORAGE WITH AMAZON S3 IN AWS LOCAL ZONES](3.1-blog-1/)
This blog introduces Amazon S3 in AWS Local Zones (specifically the Hanoi Local Zone). It details how enterprise organizations in Finance, Healthcare, and Government sectors can achieve strict local Data Residency compliance within Vietnam while maintaining single-digit millisecond latency and leveraging familiar S3 management APIs.

---

### 📌 [Blog 2 - MANAGING AWS LAMBDA FUNCTION CODE WITH SELF-MANAGED AMAZON S3 BUCKETS](3.2-blog-2/)
This blog explores the Self-Managed Amazon S3 Buckets feature (`REFERENCE` mode) for AWS Lambda. It explains how large-scale Serverless architectures can directly reference `.zip` deployment packages from user-managed S3 buckets to bypass regional Lambda code storage quotas, enforce strict security controls, streamline CI/CD pipelines, and enable instant version rollbacks using S3 Versioning.

---

### 📌 [Blog 3 - PROCESS MILLIONS OF DYNAMODB RECORDS EASILY WITH BULK EXECUTOR](3.3-blog-3/)
This blog examines the open-source Bulk Executor for Amazon DynamoDB tool developed by AWS Labs. By pairing a lightweight command-line interface with AWS Glue's (Apache Spark) distributed parallel execution engine, administrators can safely and efficiently run bulk operations—such as count, find, update, delete, copy, and SQL analytics—across millions or billions of DynamoDB items.