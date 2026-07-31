---
title : "Configure CloudFront & OAC"
date : 2026-07-31
weight : 2
chapter : false
pre : " <b> 5.3.2. </b> "
---

#### Objective
Use Amazon CloudFront as a Content Delivery Network (CDN) to accelerate page load times globally. Instead of making the S3 bucket Public, we will use the **OAC (Origin Access Control)** authentication mechanism to securely grant CloudFront permission to read data from S3.

#### Implementation Steps

1. Navigate to the **CloudFront** service on the AWS Management Console and click the **Create a CloudFront distribution** button.
2. In the **Specify Origin** section:
   * **Origin domain:** Click the empty field and select the S3 Bucket you created in step 5.3.1.
   * **Origin access:** Select **Origin access control settings (recommended)**.
   * Click the **Create control setting** button, keep the default parameters in the pop-up, and click **Create**.

![Configure CloudFront OAC](../../../images/5-Workshop/5.3-S3/cloudfront1.png)

3. In the **Web Application Firewall (WAF)** section:
   * Select **Do not enable security protections** (to avoid unnecessary costs within the scope of this workshop).

4. Click the **Create distribution** button.

5. Immediately after the Distribution is successfully created, CloudFront will display a blue banner at the top, prompting you to update the **S3 bucket policy**.

* Click the **Copy policy** button (or copy the JSON code block displayed on the screen). We will temporarily save this code and use it in the next step (5.3.3) to formally grant CloudFront access to the S3 bucket.

In case you don't see it, we can go to Origins -> Edit. Create OAC for our S3 bucket and click on **Copy Policies**
![Copy S3 Bucket Policy](../../../images/5-Workshop/5.3-S3/cloudfront2.png)