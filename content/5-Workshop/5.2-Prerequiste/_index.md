---
title : "Preparation Steps"
date : 2026-07-31  
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### 1. Set up IAM Permissions
To smoothly deploy and clean up resources in this workshop, you need to attach the IAM Policy below to your AWS User account. This policy has been optimized to include only the necessary Serverless services for the project.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ServerlessAppWorkshopPermissions",
            "Effect": "Allow",
            "Action": [
                "apigateway:*",
                "lambda:*",
                "dynamodb:*",
                "s3:*",
                "cloudfront:*",
                "cognito-idp:*",
                "cognito-identity:*",
                "logs:*",
                "cloudwatch:*",
                "sns:*",
                "iam:CreateRole",
                "iam:GetRole",
                "iam:PassRole",
                "iam:AttachRolePolicy",
                "iam:DetachRolePolicy",
                "iam:PutRolePolicy",
                "iam:DeleteRolePolicy",
                "iam:CreatePolicy",
                "iam:GetPolicy",
                "iam:DeletePolicy",
                "iam:ListAttachedRolePolicies"
            ],
            "Resource": "*"
        }
    ]
}
```

#### 2. Environment & Tool Requirements
Before diving into AWS configuration, ensure your local machine is equipped with the following foundational tools:

* **Node.js (v18+) & npm/pnpm:** The runtime environment required to initialize and build the Frontend source code (React/Vite).
* **Python (3.9+) & pip:** The environment for developing, installing dependencies, and packaging the Backend source code.
* **AWS CLI (v2):** The command-line interface to interact with AWS. Run the `aws configure` command to set up your credentials. (Recommended default Region: `ap-southeast-1` - Singapore).
* **Git:** Used to clone the workshop's source code repository to your local machine.

#### 3. Architecture Overview
In this workshop, we will build a complete Serverless Web Application in the **Singapore (`ap-southeast-1`)** region, comprising the following core components:

* **S3 Bucket:** Acts as the static web hosting for the Frontend. This bucket will be 100% secured (Private).
* **CloudFront:** A Content Delivery Network (CDN) connected to S3 via OAC (Origin Access Control) to enhance speed and security.
* **API Gateway & AWS Lambda:** Handles all the business logic (Backend) through RESTful APIs, packaged using Python.
* **DynamoDB:** A NoSQL database with single-digit millisecond latency, used to store user data and tasks.
* **CloudWatch:** The heart of the monitoring system, collecting all Logs and Metrics for real-time troubleshooting.