---
title : "Clean up"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Clean up

Congratulations on completing this workshop!
In this workshop, you have learned how to build and deploy a complete and secure Serverless Web architecture on AWS.

+ By combining Amazon S3, CloudFront, and OAC, you distributed a static web interface with high speed and absolute security without exposing it to the Public Internet.
+ By utilizing Amazon Cognito, API Gateway, AWS Lambda, and DynamoDB, you built a robust serverless Backend system, automatically scaling and optimizing operational costs.

#### Clean up steps

1. **Disable and delete CloudFront Distribution**
Navigate to the CloudFront service on the AWS console. Select the Distribution you created for this project. Click **Disable** (this process may take about 3-5 minutes). After the status changes to *Disabled*, select the Distribution again and click **Delete**.

![delete cloudfront](../../images/5-Workshop/5.6-Cleanup/delete-cloudfront.png)

2. **Delete S3 buckets**
Open the S3 console. Select the buckets we created for the lab (including the Frontend bucket and the Attachment bucket). 
+ First, click **Empty** to delete all files inside, and confirm by typing `permanently delete`.

![delete s3](../../images/5-Workshop/5.6-Cleanup/empty-s3.png)

+ After the bucket is empty, select the bucket again, click **Delete**, and enter the bucket name to delete it completely.

![delete s3](../../images/5-Workshop/5.6-Cleanup/delete-s3.png)

3. **Delete API Gateway**
Open the API Gateway console. Select the API for this project, click the **Delete** button at the top, and confirm the deletion.

![delete apigw](../../images/5-Workshop/5.6-Cleanup/delete-apigw.png)

![delete apigw](../../images/5-Workshop/5.6-Cleanup/delete-apigw-confirmation.png)

4. **Delete AWS Lambda functions**
Open the AWS Lambda console. Check the box next to the created Lambda function, click the **Actions** dropdown menu -> Select **Delete** and confirm.

![delete lambda](../../images/5-Workshop/5.6-Cleanup/delete-lambda.png)

5. **Delete DynamoDB tables**
Navigate to the DynamoDB service, and select **Tables** on the left menu. Select the created data tables, click the **Delete** button, and enter the confirmation text as requested by AWS.

![delete dynamodb](../../images/5-Workshop/5.6-Cleanup/delete-dynamodb.png)

6. **Delete Amazon Cognito User Pool**
Open the Amazon Cognito console. Select the project's User Pool, navigate to the Pool's settings, click **Delete**, and follow the on-screen confirmation instructions.

![delete cognito](../../images/5-Workshop/5.6-Cleanup/delete-cognito.png)
