---
title: "Worklog Week 2"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

### Week 2 Objectives (22/06/2026 – 28/06/2026):
* Master AWS IAM Roles security and enforce the Principle of Least Privilege Access across all **CodExecute** system components.
* Design and implement an **Amazon SQS** asynchronous message queue (`Submissions Queue`) acting as a buffer against traffic spikes during programming contests.
* Configure **Amazon API Gateway** (HTTP API) and **Amazon CloudFront CDN Distribution** for global static asset delivery.

### Weekly Tasks Breakdown:
| Day | Task Description | Start Date | End Date | Resource Links |
| --- | --- | --- | --- | --- |
| Mon | - **CodExecute IAM Security Roles:** Analyze minimal permissions required between services. <br> - Author JSON IAM Policies granting scoped access to Lambda API, Lambda Worker, S3, and DynamoDB without hardcoded credentials. | 22/06/2026 | 22/06/2026 | [Access Management with IAM](https://000002.awsstudygroup.com/) <br> [IAM Policies Developer Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/) |
| Tue | - **Amazon SQS Asynchronous Queueing:** Study queue buffering principles for high-concurrency contest submissions. <br> - Provision `Submissions Queue` on Amazon SQS, configuring Long Polling (`ReceiveMessageWaitTimeSeconds = 20`) to save 90% in empty polling costs. | 23/06/2026 | 23/06/2026 | [Amazon SQS Developer Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) |
| Wed | - **Dead-Letter Queue (DLQ) Failover:** Configure SQS Dead-Letter Queues to automatically capture failed evaluation jobs retried > 3 times, guaranteeing zero user data loss. | 24/06/2026 | 24/06/2026 | [SQS Dead-Letter Queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) |
| Thu | - **Amazon API Gateway & CloudFront CDN Integration:** Provision API Gateway HTTP API (70% cheaper than REST API), configure JWT auth routes. <br> - Deploy CloudFront CDN distributing React SPA from S3 and proxying `/api/*` to API Gateway. | 25/06/2026 | 25/06/2026 | [Amazon API Gateway Docs](https://docs.aws.amazon.com/apigateway/) <br> [Amazon CloudFront Docs](https://docs.aws.amazon.com/cloudfront/) |
| Fri | - **Hands-on & Phase 1 Finalization:** Deploy full SQS + DLQ + IAM Roles + CloudFront CDN infrastructure stack using AWS SAM Templates. | 26/06/2026 | 26/06/2026 | [AWS SAM Deployment Guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-deploy.html) |

### Week 2 Deliverables & Achievements:
* **Zero-Trust IAM Roles Enforcement:** Successfully attached scoped IAM Roles to Lambda API and Worker handlers, eliminating hardcoded access keys.
* **Built SQS Asynchronous Submission Buffer:** Provisioned Amazon SQS `Submissions Queue` capable of buffering thousands of submissions/minute, utilizing Long Polling to eliminate 90% of empty API request charges.
* **Guaranteed Zero Data Loss:** Implemented Dead-Letter Queue (DLQ) safeguards capturing failed messages for developer inspection, guaranteeing 100% data reliability.
* **API Gateway & CloudFront Edge Distribution:** Deployed API Gateway HTTP API with JWT Token authentication, integrated with CloudFront CDN for ultra-low latency edge asset delivery and automated AWS Shield Layer 3/4 DDoS protection.
* **Completed 100% of CodExecute Phase 1:** All core infrastructure resources (VPC, S3, DynamoDB, SQS, IAM, CloudFront, API Gateway) are live on AWS Dev environment following Well-Architected standards.
