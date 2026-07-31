---
title: "Testing Monitoring & Logging via Amazon CloudWatch"
date: 2026-06-01
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

In this section, we validate the operational visibility and observability of our Serverless backend architecture using **Amazon CloudWatch**, focusing on **CloudWatch Logs** (tracing Lambda execution events and errors) and **CloudWatch Metrics / Alarms** (performance analytics and cost control alerts).

---

#### 1. Verifying Execution Traces on CloudWatch Logs

* **Execution Steps:**
  1. Open the **Amazon CloudWatch Console** -> Select **Log groups** from the left navigation panel.
  2. Click the Log Group dedicated to the backend Lambda functions (e.g., `/aws/lambda/TodoCRUDHandler` or `/aws/lambda/GetPresignedUrlHandler`).
  3. Select the most recent **Log Stream** generated immediately after triggering CRUD operations on the web client.

* **Expected Results:**
  * CloudWatch records the complete invocation lifecycle: `START RequestId: ...`, application debug logs (`console.log`), and `REPORT RequestId: ... Duration: ... ms Billed Duration: ... ms Memory Size: ... MB`.
  * Zero unhandled exceptions or `Task timed out` error events are observed in the log trace.

![Verifying Log Streams on CloudWatch Logs](/images/5-Workshop/5.4-testing/cloudwatch-logs-verification.png)

---

#### 2. Inspecting CloudWatch Operational Metrics

* **Execution Steps:**
  1. Navigate to CloudWatch Console -> **Metrics** -> **All metrics** -> Select **Lambda** or **API Gateway** namespace.
  2. Observe real-time key performance indicators:
     * **Invocations:** Total count of Lambda function invocations.
     * **Duration:** Average execution latency (ms).
     * **Error count & Success rate (%):** Total request failures (target: 0).
     * **Throttles:** Concurrency limit bottleneck checks.

* **Expected Results:**
  * Graphs reflect real-time request traffic accurately.
  * Backend Lambda latency remains optimal (under $200\text{ ms}$ for DynamoDB CRUD queries).

![Inspecting Lambda & API Gateway Metrics Dashboard](/images/5-Workshop/5.4-testing/cloudwatch-metrics-dashboard.png)

---

#### 3. Verifying Billing Alarms & AWS Budget Controls

* **Execution Steps:**
  1. Access CloudWatch Console -> **Alarms** -> **In alarm** / **OK**.
  2. Inspect the operational state of the $1.00\text{ USD}$ threshold Billing Alarm.

* **Expected Results:**
  * The Alarm remains in the **`OK`** state as all test workloads fall well within the **AWS Free Tier** limits.
  * Email notification integration via **Amazon SNS** is active and ready to fire should unexpected billings occur.

![Verifying CloudWatch Billing Alarm State](/images/5-Workshop/5.4-testing/cloudwatch-billing-alarm.png)