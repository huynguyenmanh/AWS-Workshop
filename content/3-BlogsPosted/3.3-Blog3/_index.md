---
title: "Blog 3: Process Millions of DynamoDB Records Easily with Bulk Executor"
date: 2026-07-24
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---



# PROCESS MILLIONS OF DYNAMODB RECORDS EASILY WITH BULK EXECUTOR

Amazon DynamoDB is designed to handle high-velocity data workloads and automatically scale with application traffic. In everyday operations, applications typically query or update individual items using Partition Keys and Sort Keys.

However, organizations occasionally need to execute bulk operations across entire tables containing millions or billions of items: counting aged records, backfilling TTL attributes, copying data to another table/account, or loading massive datasets from Amazon S3. Previously, this required developers to build custom, complex scripts handling Segmented Scans, multi-threaded parallel execution, rate limiting, retry logic on throttling, and progress tracking.

**Bulk Executor for Amazon DynamoDB** is an open-source tool developed by AWS Labs that dramatically simplifies this process. Developers execute straightforward CLI commands on their terminal, while distributed parallel processing is handled behind the scenes by **AWS Glue (Apache Spark)** on the cloud.

---

## 🌟 KEY HIGHLIGHTS

1. **Simple Terminal (CLI) Experience:** Users interact via Terminal (local machine, EC2 instance, or container). The tool behaves like a local command-line app, while heavy data processing is offloaded to AWS Glue on Cloud.
2. **No Custom Parallel Processing Required:** Bulk Executor automatically manages Segmented Scans, Spark DataFrames, job partitioning, result aggregation, and error handling. Users do not need deep Apache Spark or Glue expertise.
3. **Rich Pre-built Utility Commands:** Out-of-the-box support for common bulk tasks without writing code: `count`, `find`, `update`, `delete`, `copy`, `fill`, `load`, `diff`, and `sql`.
4. **Massive Scale Capability:** Utilizes AWS Glue Spark jobs to split and distribute workloads across multiple Workers, easily scaling from small tables to tables with millions or billions of items.
5. **Real-time Progress Monitoring:** Integrated with **Amazon CloudWatch Live Tail** to stream Glue logs directly back to the local terminal, displaying Job IDs, runtime progress, status, and cost estimates without switching to the Glue Console.
6. **Proactive Rate Limiting:** Allows users to set maximum Read/Write Capacity limits to prevent bulk jobs from consuming excessive throughput and throttling live production traffic.
7. **Extensible with Python:** Developers can supply custom Python functions for specialized business logic. Bulk Executor handles running that logic concurrently across the entire table.
8. **Fully Open Source:** Released officially in the `amazon-dynamodb-tools` repository maintained by AWS Labs on GitHub.

---

## 🛠️ COMMON BULK OPERATIONS

| Command | Description |
| :--- | :--- |
| **`COUNT`** | Counts total items in a table or filters with Spark SQL conditions (e.g., counting orders created before a specific timestamp). |
| **`FIND`** | Scans the full table to locate matching items when an appropriate Global Secondary Index (GSI) is missing. Large result sets are automatically saved to Amazon S3. |
| **`UPDATE`** | Scans and passes each item to a user-supplied Python function to modify attributes (e.g., backfilling TTL attributes, normalizing formats). |
| **`DELETE`** | Deletes items matching specified criteria in bulk. *Requires Point-in-Time Recovery (PITR) enabled prior to execution.* |
| **`COPY`** | Copies items from a source table to a destination table across different AWS Accounts or Regions. |
| **`FILL`** | Generates large volumes of realistic mock test data using the `Faker` library for performance and Partition Key design testing. |
| **`LOAD`** | Ingests CSV, JSON, or Parquet datasets from Amazon S3 directly into an existing DynamoDB table with custom column-to-attribute mapping. |
| **`DIFF`** | Compares two tables item-by-item to report additions, deletions, or attribute-level content discrepancies. |
| **`SQL`** | Executes Spark SQL queries (`SELECT`) directly against DynamoDB data to perform complex aggregations and statistics. |

---

## 📐 ARCHITECTURE & REAL-WORLD USE CASE

### Operating Architecture
```text
[Administrator (Terminal / CLI)]
             │
             ▼
    [Bulk Executor CLI]
             │
             ▼
     [AWS Glue Job (Spark)] ◄──► [Amazon S3 (Scripts & Output Logs)]
             │
             ▼
    [Amazon DynamoDB Table]
```

### Real-World Use Case:
An e-commerce platform needs to manage millions of historical order records:
1. **Step 1:** Run **`count`** to estimate the volume of legacy order records.
2. **Step 2:** Execute **`update`** with a Python function to attach TTL (Time To Live) attributes for automated item expiration.
3. **Step 3:** Perform **`copy`** to replicate data to a staging table and run **`diff`** to verify data fidelity post-migration.

---

## 🔐 SECURITY, COST CONTROL & CONSIDERATIONS

* **Data Protection via PITR:** Before executing destructive or modifying commands (`update`, `delete`), Bulk Executor enforces that **Point-in-Time Recovery (PITR)** is enabled on the target table for safety rollbacks.
* **Least Privilege IAM Roles:** Bootstrapping creates required Cloud infrastructure (Glue Job, S3 Bucket, CloudWatch Log Group). It strictly separates roles between *Bulk Admin* (environment setup) and *Bulk User* (job execution/logs).
* **Cost Management:** Bulk operations consume DynamoDB RCU/WCU, AWS Glue DPUs, and S3/CloudWatch storage. It is recommended to schedule bulk jobs during off-peak hours and enforce throughput throttling limits.

---

## 📝 CONCLUSION

**Bulk Executor for Amazon DynamoDB** transforms complex, distributed data processing tasks into simple, familiar terminal commands. By pairing a lightweight command-line interface with AWS Glue's distributed processing power, engineering teams can save significant development effort while safely operating on large-scale DynamoDB datasets.

🔗 **Original Reference Documents:**
* [AWS Database Blog: Introducing open source Bulk Executor for Amazon DynamoDB](https://aws.amazon.com/blogs/database/introducing-open-source-bulk-executor-for-amazon-dynamodb/)
* [GitHub Repository: awslabs/amazon-dynamodb-tools](https://github.com/awslabs/amazon-dynamodb-tools/tree/main/tools/bulk_executor)


![Blog](<../../images/3-Blogs/Blog3.png>)