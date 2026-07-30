---
title: "Create SQS Queue"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

# CREATE THE AMAZON SQS SUBMISSION QUEUE

In this step, we create the **Amazon SQS Standard Queue** (`codeexecute-submission-queue`) that acts as the asynchronous buffer between the API Lambda and the Worker Lambda. The queue name is referenced in the application via the environment variable `SQS_QUEUE_URL` in `app/core/config.py`.

---

### Step 1: Create the SQS Queue

1. Access the **Amazon SQS Console** → click **Create queue**.
2. Select **Standard** queue type (not FIFO — Standard is sufficient for submission workloads and provides higher throughput).
3. Enter the queue name: `codeexecute-submission-queue`.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/sqs-create-1.jpg" alt="Create SQS queue - name and type" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.7.1: Selecting Standard queue type and entering the queue name</i>
</p>

</div>

4. Configure the queue parameters:

   | Parameter | Value | Reason |
   |---|---|---|
   | **Visibility timeout** | `300` seconds (5 min) | Must be ≥ Lambda Worker timeout to prevent duplicate processing |
   | **Message retention period** | `86400` seconds (1 day) | Retains unprocessed messages for 24 hours |
   | **Maximum message size** | `256 KB` (default) | Sufficient for code submission payloads |
   | **Delivery delay** | `0` seconds | Immediate delivery to Worker |
   | **Receive message wait time** | `0` seconds | Short polling (Lambda uses its own polling mechanism) |

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/sqs-create-2.jpg" alt="SQS queue configuration parameters" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.7.2: Configuring visibility timeout and message retention period</i>
</p>

</div>

5. Leave **Access policy** as default (only the Lambda execution roles will access this queue via IAM).
6. Click **Create queue**.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/sqs-created.jpg" alt="SQS queue created successfully" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.7.3: codeexecute-submission-queue created successfully</i>
</p>

</div>

---

### Step 2: Copy the Queue URL

After creation, copy the **Queue URL** from the queue detail page. This URL is required by the application:

```
https://sqs.ap-southeast-1.amazonaws.com/014936669466/codeexecute-submission-queue
```

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/sqs-url.jpg" alt="SQS Queue URL" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.7.4: Copying the Queue URL from the SQS Console</i>
</p>

</div>

---

### Step 3: Set the Queue URL in Lambda Environment Variables

The `codeexecute-api` Lambda reads `SQS_QUEUE_URL` from its environment variables to know where to push submission jobs.

1. Open **AWS Lambda Console** → select the `codeexecute-api` function → **Configuration** → **Environment variables** → **Edit**.
2. Add the environment variable:

   - **Key**: `SQS_QUEUE_URL`
   - **Value**: `https://sqs.ap-southeast-1.amazonaws.com/014936669466/codeexecute-submission-queue`

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/lambda-env-sqs.jpg" alt="Setting SQS_QUEUE_URL in Lambda environment variables" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.7.5: Adding SQS_QUEUE_URL environment variable to codeexecute-api Lambda</i>
</p>

</div>

3. Click **Save**.

This environment variable maps directly to the application setting in `app/core/config.py`:

```python
SQS_QUEUE_URL: str = ""   # loaded from Lambda environment variable
```

And is consumed in `app/services/sqs_service.py`:

```python
response = sqs_client.send_message(
    QueueUrl=settings.SQS_QUEUE_URL,
    MessageBody=json.dumps(message_body)
)
```

---

### Verification

The `codeexecute-submission-queue` SQS Standard Queue is now active. The next step connects this queue to the `codeexecute-worker` Lambda via an **Event Source Mapping**, so that every message pushed to the queue automatically triggers the Worker to process it.
