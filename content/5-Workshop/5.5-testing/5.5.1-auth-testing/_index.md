---
title: "User Authentication Testing (Amazon Cognito)"
date: 2026-06-01
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

In this section, we will conduct end-to-end testing of the User Authentication flow using Amazon Cognito User Pools integrated with the Web Single Page Application hosted on Amazon S3 and distributed via AWS CloudFront HTTPS.

#### Testing Steps:

1. **Access the Web Application:**
   * Open your browser and navigate to the **CloudFront HTTPS Distribution** domain URL (e.g., `https://d3jw5vxof6iq0j.cloudfront.net`).

   ![CloudFront App Interface](../../../images/5-Workshop/5.5-testing/01-cloudfront-app.jpg)

2. **Sign Up a New Account:**
   * Click the **Sign Up** button.
   * Complete the registration form with a valid personal email address, username, and password (ensuring compliance with Cognito password policies: uppercase, lowercase, numbers, and special characters).

3. **Verify Confirmation Code:**
   * Check your personal email inbox for the 6-digit verification OTP code dispatched automatically by Amazon Cognito.
   * Enter the confirmation code into the application UI to verify and activate the user account.

4. **Log In to the Application:**
   * Authenticate using the newly created username/email and password credentials.
   * **Expected Results:**
     * The system successfully authenticates and issues a secure **Cognito JWT Access Token**.
     * The token is safely persisted in the browser's **LocalStorage** for subsequent authorized REST API requests sent to Amazon API Gateway.

   ![Successful Login & JWT Token in LocalStorage](../../../images/5-Workshop/5.5-testing/02-cognito-auth.jpg)