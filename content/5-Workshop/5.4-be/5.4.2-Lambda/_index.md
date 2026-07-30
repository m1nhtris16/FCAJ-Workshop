---
title: "Create & Configure AWS Lambda"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

<!-- # CREATING AND CONFIGURING AWS LAMBDA FUNCTIONS FROM CONTAINER IMAGES -->

In this section, we will create two **AWS Lambda Functions** (`codeexecute-worker` and `codeexecute-api`) directly on the **AWS Console** using the **Container Images** pushed to Amazon ECR in Section 5.4.1. Then, we will configure execution memory, timeouts, environment variables, and attach the SQS Trigger.

---

### Step 1: Create Lambda Function for Worker (`codeexecute-worker`)

1. Access the **AWS Lambda Console** and click **Create function**.
2. Configure basic parameters for the Lambda Worker:
   - **Options:** Select **Container image**.
   - **Function name:** Enter `codeexecute-worker`.
   - **Container image URI:** Click **Browse images**, choose repository `codexecute-lambda-worker`, and select image tag `latest`.
   - **Architecture:** Select **x86_64**.
   - **Execution role:** Select **Use an existing role** with policies granting access to S3, DynamoDB, and SQS messaging.
3. Click **Create function**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-worker-create.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-worker-create.jpg" alt="Create Lambda Worker from Container Image" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.2.1: Creating Lambda codeexecute-worker from ECR Container Image</i>
</p>

</div>

4. Configure execution settings (**General configuration**):
   - Switch to **Configuration** → **General configuration** and click **Edit**.
   - **Memory:** Set to **1024 MB** (or 2048 MB to accelerate g++/Corretto compilation inside the sandbox).
   - **Timeout:** Enter **30 seconds** (or 1 minute).
   - Click **Save**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-worker-config.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-worker-config.jpg" alt="Configure Memory and Timeout for Lambda Worker" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.2.2: Setting Memory capacity and Timeout for Lambda Worker</i>
</p>

</div>

5. Attach **Amazon SQS Queue** as an asynchronous submission trigger:
   - In the **Function overview** diagram, click **Add trigger**.
   - **Select a trigger:** Select **SQS**.
   - **SQS queue:** Select queue `codexecute-submissions-queue`.
   - **Batch size:** Enter `1` or `5`.
   - Click **Add**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-worker-sqs-trigger.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-worker-sqs-trigger.jpg" alt="Attach SQS Trigger to Lambda Worker" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.2.3: Attaching SQS Queue codexecute-submissions-queue as Trigger for Worker</i>
</p>

</div>

---

### Step 2: Create Lambda Function for API (`codeexecute-api`)

1. Click **Create function** on the **AWS Lambda Console**.
2. Configure basic parameters for the Lambda API Backend:
   - **Options:** Select **Container image**.
   - **Function name:** Enter `codeexecute-api`.
   - **Container image URI:** Click **Browse images**, choose repository `codexecute-lambda-api`, and select image tag `latest`.
   - **Architecture:** Select **x86_64**.
   - **Execution role:** Select **Use an existing role** granting access to DynamoDB, S3, and SQS.
3. Click **Create function**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-api-create.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-api-create.jpg" alt="Create Lambda API from Container Image" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.2.4: Creating Lambda codeexecute-api from ECR Container Image</i>
</p>

</div>

4. Configure execution settings (**General configuration**):
   - Switch to **Configuration** → **General configuration** → click **Edit**.
   - **Memory:** Set to **512 MB** (or 1024 MB).
   - **Timeout:** Enter **15 seconds** (or 30 seconds).
   - Click **Save**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-api-config.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-api-config.jpg" alt="Configure Memory and Timeout for Lambda API" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.2.5: Setting Memory capacity and Timeout for Lambda API</i>
</p>

</div>

5. Configure system **Environment variables**:
   - Switch to **Configuration** → **Environment variables** → click **Edit**.
   - Enter the environment key-value pairs required by FastAPI to interact with DynamoDB, S3, and SQS:

| Key | Value | Description |
| :--- | :--- | :--- |
| `AWS_REGION` | `ap-southeast-1` | Target AWS deployment region |
| `SQS_QUEUE_URL` | `https://sqs.ap-southeast-1.amazonaws.com/.../codexecute-submissions-queue` | SQS Submission Buffer Queue URL |
| `TESTCASES_BUCKET` | `codeexecute-testcases` | S3 Testcases repository bucket |
| `USER_MEDIA_BUCKET` | `codeexecute-user-media` | S3 Media and user avatar bucket |

   - Click **Save**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-api-env.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-api-updated.jpg" alt="Configure Environment Variables for Lambda API" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.2.6: Configuring Environment Variables for Lambda API</i>
</p>

</div>

---

### Step 3: Verify Operational Status of Both Lambda Functions

1. Return to the main **AWS Lambda Console** → **Functions** list.
2. Confirm both `codeexecute-worker` and `codeexecute-api` display status **Active** and reference valid ECR Container Image URIs.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-list-verify.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-list-verify.jpg" alt="Verify Lambda Functions List" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.2.7: Both Lambda functions codeexecute-worker and codeexecute-api displaying Active status</i>
</p>

</div>

---

### Verification

At this stage, both core Backend services for the CodExecute platform are provisioned on **AWS Lambda** as Container Images, fully equipped to receive requests from API Gateway and SQS.
