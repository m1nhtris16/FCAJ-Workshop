---
title: "Connect SQS to Lambda Worker"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---



In this step, we configure the **SQS Event Source Mapping** that connects `codeexecute-submission-queue` to the `codeexecute-worker` Lambda function. This mapping causes Lambda to automatically poll the SQS queue and invoke the Worker with a batch of messages whenever new submissions arrive.

---

### Step 1: Add SQS Trigger to Lambda Worker

1. Open **AWS Lambda Console** → select the `codeexecute-worker` function.
2. In the **Function overview** panel, click **Add trigger**.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/esm-add-trigger.jpg" alt="Add trigger button on Lambda Worker" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.7.6: Clicking Add trigger on the codeexecute-worker Lambda overview</i>
</p>

</div>

3. In the **Add trigger** configuration panel:
   - **Trigger source**: Select **SQS**
   - **SQS queue**: Select `codeexecute-submission-queue`
   - **Batch size**: `1`
   - **Batch window**: `0` seconds
   - **Enabled**: ✅ Checked

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/esm-config.jpg" alt="Configure SQS Event Source Mapping" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.7.7: Configuring the SQS trigger with batch size 1</i>
</p>

</div>

> **Why Batch Size = 1?**  
> Each Lambda invocation processes exactly one code submission. Running multiple submissions per invocation would cause resource contention inside the sandbox container (`/tmp` filesystem, compiler processes). Batch size 1 ensures clean isolation per submission.

4. Click **Add**.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/esm-created.jpg" alt="SQS trigger added to Lambda Worker" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.7.8: SQS trigger successfully connected to codeexecute-worker Lambda</i>
</p>

</div>

---

### Step 2: Verify the Event Source Mapping

After the trigger is added, the **Function overview** will show `codeexecute-submission-queue` as an active trigger source for the `codeexecute-worker` function.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/esm-active.jpg" alt="Active SQS trigger in Lambda function overview" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.7.9: SQS Event Source Mapping active in the Lambda Worker function overview</i>
</p>

</div>

Navigate to **Configuration** → **Triggers** to confirm the Event Source Mapping details:

| Field | Value |
|---|---|
| **Source** | `codeexecute-submission-queue` |
| **Batch size** | 1 |
| **State** | Enabled |

---

### Step 3: Verify the End-to-End Submission Flow

With SQS and the Event Source Mapping active, submit a code solution from the CodExecute frontend:

1. Navigate to a problem in the frontend at `https://d1hsp5bm4hkjmb.cloudfront.net`.
2. Write a solution and click **SUBMIT**.
3. The API Lambda immediately returns a `SubmissionID` and status `Pending`.
4. Monitor the **SQS Console** → select `codeexecute-submission-queue` → **Send and receive messages** → **Poll for messages** to observe the message being consumed.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/sqs-monitor.jpg" alt="SQS queue monitoring during submission" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.7.10: Monitoring SQS message delivery and consumption during a code submission</i>
</p>

</div>

---

### Verification

The complete asynchronous submission pipeline is now operational:

| Component | Role |
|---|---|
| **`codeexecute-api` Lambda** | Pushes `{"submission_id", "problem_id", "language", "code"}` to SQS on every SUBMIT |
| **`codeexecute-submission-queue`** | Buffers and delivers submission jobs with visibility timeout = 300s |
| **Event Source Mapping** | Polls SQS and triggers `codeexecute-worker` with batch size = 1 |
| **`codeexecute-worker` Lambda** | Dequeues the message, runs sandbox code execution, writes result to DynamoDB |

