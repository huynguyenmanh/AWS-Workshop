---
title : "Update S3 Bucket Policy"
date : 2026-07-31
weight : 3
chapter : false
pre : " <b> 5.3.3. </b> "
---

#### Objective
Although CloudFront has been configured with OAC to access S3 securely, the S3 Bucket itself will default to denying all outside access requests. We need to update the **Bucket Policy** using the JSON code copied in the previous step to officially grant CloudFront permission to read the data.

#### Implementation Steps

1. Open a new tab and navigate to the **S3** service on the AWS Management Console.
2. From the Buckets list, click on the name of the frontend S3 Bucket you created in step 5.3.1.
3. Switch to the **Permissions** tab.
4. Scroll down to the **Bucket policy** section and click the **Edit** button.

![Edit S3 Bucket Policy](../../../images/5-Workshop/5.3-S3/s3-policy-edit.png)

5. In the **Policy** editor box, delete any existing content (if any) and **Paste** the JSON code that you copied from CloudFront in step 5.3.2.

6. Scroll down to the bottom of the page and click the **Save changes** button.

![Paste Policy and Save](../../../images/5-Workshop/5.3-S3/s3-policy-save.png)

Once successfully saved, you will see a green confirmation message indicating the policy has been updated. At this point, the CDN delivery flow from CloudFront to S3 is completely clear and secure.