---
title: "Clean Up Resources"
date: 2026-07-30
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

<!-- # CLEANING UP CODEXECUTE PLATFORM RESOURCES -->

Upon completing all practical deployment labs for the **CodExecute** platform, deleting all provisioned AWS resources is critical to prevent unexpected ongoing account charges.

In this section, we will systematically delete all infrastructure components adhering to systemic dependencies: **CloudFront**, **API Gateway**, **AWS Lambda**, **Amazon ECR**, **Amazon SQS**, **Amazon DynamoDB**, **Amazon S3**, **Amazon CloudWatch Log Groups**, **Amazon CloudWatch Alarms**, and **Amazon SNS Topics**.

---

### Step 1: Disable & Delete Amazon CloudFront Distribution

1. Open the **Amazon CloudFront Console** → **Distributions**.
2. Select the Distribution provisioned for the CodExecute platform (ID: `E14SU7QS7NEEO8`).
3. Click **Disable** and wait until status updates from *Enabled* to *Disabled*.
4. Once disabled, select the Distribution and click **Delete**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: disable-cloudfront.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/disable-cloudfront.jpg" alt="Disable CloudFront Distribution" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.1.1: Disabling CloudFront Distribution E14SU7QS7NEEO8</i>
</p>

<!-- PLACEHOLDER FOR IMAGE: cleanup-cloudfront.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-cloudfront.jpg" alt="Delete CloudFront Distribution" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.1.2: Deleting CloudFront Distribution E14SU7QS7NEEO8</i>
</p>

</div>

---

### Step 2: Delete Amazon API Gateway

1. Open the **Amazon API Gateway Console**.
2. Select the API Gateway `codeexecute-api-gateway`.
3. Click **Actions** (or the **Delete** button) → Enter the API name to confirm.
4. Click **Delete API**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-apigateway.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-apigateway.jpg" alt="Delete API Gateway" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.2: Deleting HTTP API Gateway codeexecute-api-gateway</i>
</p>

</div>

---

### Step 3: Delete AWS Lambda Functions

1. Open the **AWS Lambda Console** → **Functions**.
2. Select both Backend Lambda Functions:
   - `codeexecute-worker`
   - `codeexecute-api`
3. Click **Actions** → Select **Delete**.
4. Type `delete` to confirm deletion of both Lambda functions.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-lambda.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-lambda.jpg" alt="Delete AWS Lambda Functions" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.3: Deleting Lambda functions codeexecute-worker and codeexecute-api</i>
</p>

</div>

---

### Step 4: Delete Amazon ECR Repositories

1. Open the **Amazon ECR Console** → **Repositories**.
2. Select both private repositories:
   - `codexecute-lambda-worker`
   - `codexecute-lambda-api`
3. Click **Delete** → Check the option to delete all container images within the repositories.
4. Type `delete` to confirm permanent deletion.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-ecr.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-ecr.jpg" alt="Delete ECR Repositories" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.4: Deleting ECR Repositories codexecute-lambda-worker and codexecute-lambda-api</i>
</p>

</div>

---

### Step 5: Delete Amazon SQS Queue

1. Open the **Amazon SQS Console** → **Queues**.
2. Select submission queue: `codexecute-submissions-queue`.
3. Click **Delete**.
4. Type `delete` to confirm SQS Queue deletion.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-sqs.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-sqs.jpg" alt="Delete SQS Queue" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.5: Deleting SQS Queue codexecute-submissions-queue</i>
</p>

</div>

---

### Step 6: Delete Amazon DynamoDB Tables

1. Open the **Amazon DynamoDB Console** → **Tables**.
2. Select all 8 system DynamoDB tables:
   - `Users`, `Problems`, `Submissions`, `TestCases`, `Posts`, `Notifications`, `UserFollows`, `Solutions`.
3. Click **Delete table**.
4. Type `delete` to confirm deletion of all database tables.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-dynamodb.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-dynamodb.jpg" alt="Delete DynamoDB Tables" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.6: Deleting 8 Amazon DynamoDB database tables</i>
</p>

</div>

---

### Step 7: Empty & Delete Amazon S3 Buckets

1. Open the **Amazon S3 Console** → **Buckets**.
2. Perform the following steps for all 3 project S3 Buckets:
   - `codexecute-prod-frontend`
   - `codeexecute-testcases`
   - `codeexecute-user-media`
3. Click **Empty** → Type `permanently delete` to empty all objects inside the bucket.
4. Re-select the empty bucket → Click **Delete** → Enter the bucket name to confirm deletion.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: empty-s3.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/empty-s3.jpg" alt="Empty S3 Buckets" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.7.1: Emptying Amazon S3 project buckets</i>
</p>

<!-- PLACEHOLDER FOR IMAGE: cleanup-s3.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-s3.jpg" alt="Delete S3 Buckets" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.7.2: Deleting Amazon S3 project buckets</i>
</p>

</div>

---

### Step 8: Delete Amazon CloudWatch Log Groups

1. Open the **Amazon CloudWatch Console** → **Logs** → **Log groups**.
2. Search for and select log groups associated with Lambda and API Gateway:
   - `/aws/lambda/codeexecute-worker`
   - `/aws/lambda/codeexecute-api`
   - API Gateway log groups.
3. Click **Actions** → Select **Delete log group(s)** and confirm deletion.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-cloudwatch.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-cloudwatch.jpg" alt="Delete CloudWatch Log Groups" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.8: Deleting CloudWatch Log Groups generated during execution</i>
</p>

</div>

---

### Step 9: Delete Amazon CloudWatch Alarms

1. Open the **Amazon CloudWatch Console** → **Alarms** → **All alarms**.
2. Select the alarms provisioned for the CodExecute platform (e.g., Lambda error alert `codeexecute-api-errors`, SQS high queue depth alert `sqs-high-message-count-alarm`).
3. Click **Actions** → Select **Delete**.
4. Confirm CloudWatch Alarms deletion.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-cloudwatch-alarms.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-cloudwatch-alarms.jpg" alt="Delete CloudWatch Alarms" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.9: Deleting CloudWatch Alarms</i>
</p>

</div>

---

### Step 10: Delete Amazon SNS Topics & Subscriptions

1. Open the **Amazon SNS Console** → **Topics**.
2. Select the system notification topic (e.g., `codexecute-system-alerts-topic`).
3. Click **Delete** → Type `delete me` to confirm deletion.
4. Navigate to **Subscriptions** → Select any associated email/SMS subscriptions and click **Delete**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-sns.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-sns.jpg" alt="Delete Amazon SNS Topics" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.10.10: Deleting Amazon SNS Topics and Subscriptions</i>
</p>

</div>

---

### Verification

Ultimately, all Serverless resources and services belonging to the **CodExecute** platform have been thoroughly cleaned up and removed. Your AWS account is now completely freed from temporary infrastructure, preventing any unexpected ongoing maintenance charges.
