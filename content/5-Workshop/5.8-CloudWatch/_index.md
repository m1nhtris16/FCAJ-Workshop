---
title: "Monitor with CloudWatch"
date: 2026-07-30
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

# MONITORING CODEXECUTE WITH AMAZON CLOUDWATCH

In this section, we document how **Amazon CloudWatch** was used to monitor the CodExecute serverless system — covering Lambda execution logs, SQS queue metrics, and setting up alarms to detect anomalies in the grading pipeline.

All Lambda functions automatically emit structured logs to **CloudWatch Logs** through the Python `logging` module configured in `app/main.py`:

```python
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(name)s: %(message)s",
    handlers=[logging.StreamHandler(sys.stdout)]
)
```

On Lambda, `stdout` is automatically captured and streamed to CloudWatch Logs without any additional configuration.

---

### Step 1: Access Lambda Log Groups

Each Lambda function gets its own dedicated **Log Group** created automatically on first invocation:

| Lambda Function | Log Group |
|---|---|
| `codeexecute-api` | `/aws/lambda/codeexecute-api` |
| `codeexecute-worker` | `/aws/lambda/codeexecute-worker` |

1. Open **Amazon CloudWatch Console** → **Logs** → **Log groups**.
2. Search for `/aws/lambda/codeexecute` to find both log groups.

<div align="center">

<img src="/images/5-Workshop/5.8-CloudWatch/log-groups.jpg" alt="Lambda Log Groups in CloudWatch" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.8.1: CloudWatch Log Groups for codeexecute-api and codeexecute-worker</i>
</p>

</div>

---

### Step 2: Monitor Lambda Worker Execution Logs

The Worker Lambda emits detailed structured logs at each stage of code grading. Select the `/aws/lambda/codeexecute-worker` log group → open the most recent **Log stream**.

A typical successful grading log stream looks like:

```
📥 Lambda Worker invoked with event: {"Records": [...]}
[Worker] Starting execution for submission abc-123 (Problem: two-sum, Lang: python)
Loaded 3 testcases for problem two-sum
[Worker] Execution finished for submission abc-123. Result: Accepted
```

A failed submission (e.g., Wrong Answer) looks like:

```
📥 Lambda Worker invoked with event: {"Records": [...]}
[Worker] Starting execution for submission def-456 (Problem: two-sum, Lang: cpp)
Loaded 3 testcases for problem two-sum
[Worker] Execution finished for submission def-456. Result: Wrong Answer
```

<div align="center">

<img src="/images/5-Workshop/5.8-CloudWatch/worker-logs.jpg" alt="CloudWatch log stream for Lambda Worker grading" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.8.2: Lambda Worker log stream showing a complete submission grading cycle</i>
</p>

</div>

---

### Step 3: Monitor Lambda API Logs

The API Lambda logs every incoming HTTP request via Mangum/FastAPI, including the endpoint path, execution time, and any service-level errors (DynamoDB, SQS). Select the `/aws/lambda/codeexecute-api` log group → open the latest log stream.

Key log patterns to watch:

```
[INFO] sqs_service: Starting push_submission_to_queue for submission_id: abc-123
[INFO] sqs_service: Successfully pushed submission abc-123 to SQS Queue. MessageId: ...
[WARNING] sqs_service: SQS_QUEUE_URL is not configured. Skipping SQS message push.
[ERROR] sqs_service: Error sending message to SQS (falling back to local background execution): ...
```

<div align="center">

<img src="/images/5-Workshop/5.8-CloudWatch/api-logs.jpg" alt="CloudWatch log stream for Lambda API" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.8.3: Lambda API log stream showing SQS push events and request handling</i>
</p>

</div>

---

### Step 4: View Lambda Metrics Dashboard

CloudWatch automatically collects key execution metrics for each Lambda function without any instrumentation required.

1. In the CloudWatch Console → **Metrics** → **Lambda** → select `codeexecute-worker`.
2. Key metrics to monitor:

   | Metric | Description | Alert Threshold |
   |---|---|---|
   | **Invocations** | Total number of Lambda invocations | Baseline monitoring |
   | **Errors** | Number of failed invocations | > 0 triggers alert |
   | **Duration** | Execution time per invocation (ms) | > 200,000ms (Lambda Worker timeout) |
   | **Throttles** | Invocations rejected due to concurrency limit | > 0 triggers alert |
   | **ConcurrentExecutions** | Simultaneous active invocations | Monitor for spikes |

<div align="center">

<img src="/images/5-Workshop/5.8-CloudWatch/lambda-metrics.jpg" alt="Lambda metrics in CloudWatch" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.8.4: CloudWatch metrics dashboard for codeexecute-worker Lambda</i>
</p>

</div>

---

### Step 5: Monitor SQS Queue Metrics

SQS also emits built-in CloudWatch metrics for the `codeexecute-submission-queue`.

1. In CloudWatch → **Metrics** → **SQS** → select `codeexecute-submission-queue`.
2. Key metrics to monitor:

   | Metric | Description |
   |---|---|
   | **NumberOfMessagesSent** | Submissions pushed by the API Lambda |
   | **NumberOfMessagesDeleted** | Submissions successfully processed by the Worker |
   | **ApproximateNumberOfMessagesVisible** | Messages currently waiting in queue (backlog) |
   | **ApproximateAgeOfOldestMessage** | Age of the oldest unprocessed message — critical alert indicator |

<div align="center">

<img src="/images/5-Workshop/5.8-CloudWatch/sqs-metrics.jpg" alt="SQS queue metrics in CloudWatch" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.8.5: CloudWatch SQS metrics for codeexecute-submission-queue</i>
</p>

</div>



### Verification

CloudWatch monitoring is now fully configured for the CodExecute system:

| Component | CloudWatch Resource | Purpose |
|---|---|---|
| `codeexecute-api` | Log Group `/aws/lambda/codeexecute-api` | API request logs, SQS push events |
| `codeexecute-worker` | Log Group `/aws/lambda/codeexecute-worker` | Grading execution logs, sandbox output |
| `codeexecute-worker` | Metrics: Errors, Duration, Throttles | Performance and reliability monitoring |
| `codeexecute-submission-queue` | SQS Metrics | Queue backlog and throughput monitoring |

All log data and metrics are retained in CloudWatch and can be used for debugging failed submissions, optimizing Lambda timeout values, and tracking system health over time.
