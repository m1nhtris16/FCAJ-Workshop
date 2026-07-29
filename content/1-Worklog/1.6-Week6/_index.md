---
title: "Worklog Week 6"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

### Week 6 Objectives (20/07/2026 – 26/07/2026):
* Execute **Phase 4: Load Testing & Security Hardening** for the **CodExecute** project.
* Execute Workshop 3.2 "SLA & Monitoring", mastering the 5-tier Monitoring Pyramid model.
* Conduct load testing (Locust/k6 simulating up to 1,000 Virtual Users), configure automated CloudWatch Alarms to Slack/Email, and optimize Lambda compute memory allocations.

### Weekly Tasks Breakdown:
| Day | Task Description | Start Date | End Date | Resource Links |
| --- | --- | --- | --- | --- |
| Mon | - **Workshop 3.2 SLA & Monitoring Study:** Study SLA principles, analyzing the gap between *Healthy Infrastructure* (green CPU metrics) and *Healthy User Experience* (successful login and submission execution). | 20/07/2026 | 20/07/2026 | [AWS Observability & CloudWatch Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) |
| Tue | - **CloudWatch Alerting Flow Setup:** Define Custom Metrics (`SubmissionLatency`, `SubmissionErrorRate`, `LoginFailure`), configuring CloudWatch Alarms routing via Amazon SNS to Slack channels when error rates exceed 1%. | 21/07/2026 | 21/07/2026 | [CloudWatch Alarms & SNS Alerting Docs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) |
| Wed | - **CodExecute Load Testing (1,000 VUs):** Script load test scenarios using Locust/k6 to simulate 1,000 concurrent Virtual Users submitting code during peak contest windows. | 22/07/2026 | 22/07/2026 | [Locust Load Testing Docs](https://locust.io/) |
| Thu | - **AWS Lambda Power Tuning Optimization:** Use AWS Lambda Power Tuning to identify the optimal memory/vCPU sweet spot for Lambda API and Worker handlers, maximizing speed while minimizing compute costs. | 23/07/2026 | 23/07/2026 | [AWS Lambda Power Tuning](https://github.com/alexcasalboni/aws-lambda-power-tuning) |
| Fri | - **Hands-on Phase 4 Finalization:** Evaluate load test results against target SLAs (API Latency < 200ms, Submission Evaluation < 2.0s), conducting security reviews and remediating 100% of High/Critical issues. | 24/07/2026 | 24/07/2026 | [AWS Performance Efficiency Pillar](https://docs.aws.amazon.com/wellarchitected/latest/performance-efficiency-pillar/welcome.html) |

### Week 6 Deliverables & Achievements:
* **Completed Workshop 3.2 SLA & Monitoring:** Mastered the 5-tier Monitoring Pyramid model, shifting focus from raw infrastructure metrics to real customer journey monitoring.
* **Automated Instant Alerting Pipeline (CloudWatch → SNS → Slack):** Provisioned Custom Metrics and CloudWatch Alarms delivering real-time Slack/Email alerts whenever authentication failures or submission errors spike abnormally.
* **1,000 VUs Load Testing Success:** Conducted load testing simulating 1,000 concurrent code submitters; the Serverless Lambda + SQS architecture auto-scaled smoothly without request drops or system outages.
* **Optimized Compute Performance & Cost:** Identified 512MB RAM as the optimal sweet spot using AWS Lambda Power Tuning, cutting average execution duration to 800ms and saving an additional 25% in compute costs.
* **Completed 100% of CodExecute Phase 4:** CodExecute system met all target SLA performance criteria and is ready for production deployment.
