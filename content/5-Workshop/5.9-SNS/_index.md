---
title: "Setup SNS & Lambda Alarms"
date: 2026-07-30
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

# SETTING UP AMAZON SNS & CLOUDWATCH ALARMS FOR CODEXECUTE

In this section, we document how **Amazon Simple Notification Service (SNS)** was configured to deliver real-time email alerts when the CodExecute Lambda functions encounter errors. SNS acts as the notification channel that CloudWatch Alarms use to fan out alerts to subscribers.

The overall alerting architecture:

```
CloudWatch Alarm (Lambda Errors / Throttles)
          │
          ▼ triggers
    SNS Topic: codeexecute-alerts
          │
          ├─► Email: team@example.com
          └─► (extensible: Slack, PagerDuty, SMS, etc.)
```

---

### Step 1: Create the SNS Topic

1. Open the **Amazon SNS Console** → **Topics** → click **Create topic**.
2. Select **Standard** type (not FIFO).
3. Enter the topic name: `codeexecute-alerts`.
4. Leave all other settings as default.
5. Click **Create topic**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-create.jpg" alt="Create SNS topic codeexecute-alerts" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.1: Creating the codeexecute-alerts SNS Standard topic</i>
</p>

</div>

6. After creation, copy the **Topic ARN** — it will be needed when configuring CloudWatch Alarms:

```
arn:aws:sns:ap-southeast-1:014936669466:codeexecute-alerts
```

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-arn.jpg" alt="SNS Topic ARN" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.2: codeexecute-alerts topic created — Topic ARN copied</i>
</p>

</div>

---

### Step 2: Subscribe an Email Endpoint

1. In the `codeexecute-alerts` topic detail page → click **Create subscription**.
2. Configure the subscription:
   - **Protocol**: Email
   - **Endpoint**: Enter your email address (e.g., `your-email@example.com`)
3. Click **Create subscription**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-sub-create.jpg" alt="Create SNS email subscription" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.3: Creating an email subscription on the codeexecute-alerts SNS topic</i>
</p>

</div>

4. The subscription status will show **Pending confirmation**. Check your inbox for the AWS confirmation email and click **Confirm subscription**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-sub-pending.jpg" alt="SNS subscription pending confirmation" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.4: Email subscription pending — confirm via the link in the AWS notification email</i>
</p>

</div>

5. After confirming, the subscription status changes to **Confirmed**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-sub-confirmed.jpg" alt="SNS subscription confirmed" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.5: Email subscription successfully confirmed and active</i>
</p>

</div>

---

### Step 3: Create Alarm — Lambda Worker Errors

This alarm fires whenever the `codeexecute-worker` Lambda encounters any invocation error, triggering an immediate SNS email notification.

1. Open **CloudWatch Console** → **Alarms** → **Create alarm** → **Select metric**.
2. Navigate to **Lambda** → **By Function Name** → select `codeexecute-worker` → **Errors**.
3. Configure the alarm conditions:
   - **Statistic**: Sum
   - **Period**: 1 minute
   - **Threshold type**: Static
   - **Condition**: Greater than or equal to `1`
   - **Datapoints to alarm**: 1 out of 1

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/alarm-worker-condition.jpg" alt="Lambda Worker error alarm condition" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.6: Configuring alarm condition — trigger when Worker errors ≥ 1 within 1 minute</i>
</p>

</div>

4. Under **Notification** → **In alarm** → **Select an existing SNS topic** → choose `codeexecute-alerts`.
5. Name the alarm: `codeexecute-worker-errors`.
6. Click **Create alarm**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/alarm-worker-sns.jpg" alt="Connecting alarm to SNS topic" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.7: Selecting codeexecute-alerts SNS topic as the notification target for the Worker alarm</i>
</p>

</div>

---

### Step 4: Create Alarm — Lambda Worker Throttles

This alarm detects when the Worker Lambda is being throttled due to concurrency limits — which would cause submissions to silently queue up and not be graded.

1. **Select metric** → **Lambda** → **By Function Name** → `codeexecute-worker` → **Throttles**.
2. Configure:
   - **Statistic**: Sum
   - **Period**: 5 minutes
   - **Threshold**: ≥ 1
   - **Datapoints to alarm**: 1 out of 1
3. **Notification**: Select `codeexecute-alerts` SNS topic.
4. Name the alarm: `codeexecute-worker-throttles`.
5. Click **Create alarm**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/alarm-throttle.jpg" alt="Lambda throttle alarm configuration" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.8: Creating the throttle alarm for codeexecute-worker with SNS notification</i>
</p>

</div>

---

### Step 5: Create Alarm — SQS Queue Depth (Backlog)

This alarm fires when submissions are accumulating in the SQS queue faster than the Worker can process them, indicating the Worker may be failing or under-provisioned.

1. **Select metric** → **SQS** → select `codeexecute-submission-queue` → **ApproximateNumberOfMessagesVisible**.
2. Configure:
   - **Statistic**: Maximum
   - **Period**: 5 minutes
   - **Threshold**: ≥ 10 (adjust based on expected load)
   - **Datapoints to alarm**: 2 out of 3
3. **Notification**: Select `codeexecute-alerts` SNS topic.
4. Name the alarm: `codeexecute-queue-depth`.
5. Click **Create alarm**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/alarm-sqs-depth.jpg" alt="SQS queue depth alarm" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.9: Creating SQS queue depth alarm — alerts when backlog ≥ 10 messages</i>
</p>

</div>

---

### Step 6: Verify Alarms and Test SNS Notification

1. In **CloudWatch** → **Alarms**, confirm all three alarms are in **OK** state.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/alarms-ok.jpg" alt="All CloudWatch alarms in OK state" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.10: All three CloudWatch alarms active and in OK state</i>
</p>

</div>

2. To verify the SNS email pipeline works, manually publish a test message to the topic:
   - In **SNS Console** → `codeexecute-alerts` → **Publish message**
   - **Subject**: `[CodExecute] Test Alert`
   - **Message**: `This is a test notification from codeexecute-alerts SNS topic.`
   - Click **Publish message**

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-test.jpg" alt="Publishing test message to SNS topic" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.11: Publishing a test message to verify the SNS email subscription is working</i>
</p>

</div>

3. Confirm the test email arrives in your inbox from `no-reply@sns.amazonaws.com`.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-test-result.jpg" alt="SNS test result" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.9.12: Test notification received successfully</i>
</p>

</div>

---

### Verification

The complete alerting pipeline is now operational:

| Alarm | Metric | Threshold | SNS Action |
|---|---|---|---|
| `codeexecute-worker-errors` | Lambda `Errors` | ≥ 1 per minute | Email via `codeexecute-alerts` |
| `codeexecute-worker-throttles` | Lambda `Throttles` | ≥ 1 per 5 min | Email via `codeexecute-alerts` |
| `codeexecute-queue-depth` | SQS `MessagesVisible` | ≥ 10 per 5 min | Email via `codeexecute-alerts` |

Any failure in the grading pipeline — Lambda crash, concurrency throttling, or submission backlog — will now trigger an immediate email alert to all confirmed subscribers of the `codeexecute-alerts` SNS topic.
