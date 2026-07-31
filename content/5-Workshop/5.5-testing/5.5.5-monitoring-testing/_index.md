---
title: "Testing Monitoring & Logging via Amazon CloudWatch"
date: 2026-06-01
weight: 5
chapter: false
pre: " <b> 5.5.5. </b> "
---

In this section, we validate the operational observability of our Serverless backend architecture using **Amazon CloudWatch**, focusing on **CloudWatch Logs** and **CloudWatch Metrics / Alarms**.

---

#### 1. Verifying Execution Traces on CloudWatch Logs

* **Execution Steps:**
  1. Open **Amazon CloudWatch Console** -> Select **Log groups**.
  2. Select the Log Group dedicated to backend Lambda functions and inspect the latest Log Stream.

* **Expected Results:**
  * CloudWatch records the complete execution trace: `START RequestId`, `REPORT Duration`, `Billed Duration`, `Memory Size`.

![CloudWatch Log Stream Tracing](../../../images/5-Workshop/5.5-testing/08-cloudwatch-logs.jpg)

---

#### 2. Inspecting Operational Metrics

* **Execution Steps:**
  1. Access CloudWatch Console -> **Metrics** -> Select **Lambda** / **API Gateway**.
  2. Observe real-time indicators: **Invocations**, **Duration**, **Error Count**.

* **Expected Results:**
  * Graphs reflect real-time request traffic accurately with 100% success rate.

![Lambda Metrics Dashboard](../../../images/5-Workshop/5.5-testing/09-cloudwatch-metrics.jpg)

---

#### 3. Verifying Billing Alarms

* **Execution Steps:**
  1. Access CloudWatch Console -> **Alarms**.
  2. Inspect the $1.00 USD threshold Billing Alarm state.

* **Expected Results:**
  * The Alarm remains in the **`OK`** state.

![Billing Alarm State Verification](../../../images/5-Workshop/5.5-testing/10-billing-alarm.jpg)