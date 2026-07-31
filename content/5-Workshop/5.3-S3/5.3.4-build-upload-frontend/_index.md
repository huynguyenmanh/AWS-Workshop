---
title : "Build and Upload Frontend"
date : 2026-07-31
weight : 4
chapter : false
pre : " <b> 5.3.4. </b> "
---

#### Objective
The Frontend source code (React/Vite) must be compiled (built) into static files (HTML, CSS, JS) before deploying to production. This step guides you on how to build the source code on your local machine and upload those static files to the S3 Bucket to make the website live via CloudFront.

#### Implementation Steps

1. **Build the source code:**
   * Open your Terminal (or Command Prompt) and navigate to your Frontend source code directory.
   * Run the following command to install the required dependencies (if you haven't already):
     ```bash
     npm install
     ```
   * Run the following command to bundle and build the project:
     ```bash
     npm run build
     ```
   * Once completed, the system will generate a new folder named `dist` (for Vite) or `build` (for Create React App). This folder contains all the optimized static files.

2. **Upload to S3 Bucket:**
   * Open your browser, navigate to the **S3** service on the AWS Console, and open the bucket you created in step 5.3.1.
   * Click the **Upload** button.
   * Click **Add files** and **Add folder** to select **all the contents inside** the `dist` (or `build`) folder. 
   * *Important note: You must upload the files inside, not the `dist` folder itself. Ensure the `index.html` file is located at the absolute root of the bucket.*
   * Scroll to the bottom and click the **Upload** button. Wait a few moments for the upload process to finish, then click **Close**.

![Upload files to S3](../../../images/5-Workshop/5.3-S3/s3-upload-files.png)

3. **Verify the result:**
   * Open the **CloudFront** service on the AWS Console.
   * Find the Distribution you created in step 5.3.2 and copy the URL from the **Domain name** column (e.g., `d123456...cloudfront.net`).
   * Open a new tab in your browser and paste this URL. You should see your Frontend website loading quickly and securely over HTTPS!

![Test CloudFront Domain](../../../images/5-Workshop/5.3-S3/cloudfront-test.png)