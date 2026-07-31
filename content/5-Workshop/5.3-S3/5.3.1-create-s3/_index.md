---
title : "Create S3 Bucket"
date : 2026-07-31
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

#### Objective
The first step to host the static Frontend source code (HTML, CSS, JS from React/Vite) is to create an Amazon S3 Bucket. The crucial part of this architecture is that we will **not** enable Public Access for this Bucket, ensuring absolute security and forcing users to access it exclusively through CloudFront.

#### Implementation Steps

1. Log in to the **AWS Management Console** and search for the **S3** service.
2. In the S3 console, click the orange **Create bucket** button.

![Create S3 Bucket](../../../images/5-Workshop/5.3-S3/s3-step1.png)

3. In the **General configuration** section, set the following information:
   * **AWS Region:** Select `ap-southeast-1` (Singapore) for latency optimization.
   * **Bucket name:** Enter a name for your bucket. This name must be globally unique (e.g., `amzn-s3-demobucket-todo`).

![Create S3 Bucket](../../../images/5-Workshop/5.3-S3/s3-step2.png)

4. Under **Object Ownership**, keep the default option **ACLs disabled (recommended)**.
5. Under **Block Public Access settings for this bucket**:
   * **Ensure that "Block *all* public access" is checked.** This is a mandatory step to keep the bucket Private.

![Create S3 Bucket](../../../images/5-Workshop/5.3-S3/s3-step3.png)

6. For the remaining configurations, you can keep the default settings.
7. Scroll down to the bottom of the page and click the **Create bucket** button.

![Create S3 Bucket](../../../images/5-Workshop/5.3-S3/s3-step4.png)

Once successfully created, you will see your bucket appear in the list with the label `Buckets and objects not public`. Proceed to the next step to configure CloudFront to deliver content from this Private bucket.