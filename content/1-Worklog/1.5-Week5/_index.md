---
title: "Worklog Week 5"
date: 2026-07-13
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

### Week 5 Objectives (13/07/2026 – 19/07/2026):
* Understand AWS PrivateLink architecture and VPC Endpoints (Gateway vs Interface Endpoints).
* Execute Workshop 3.1 "Securing Hybrid Access to S3 using VPC Endpoints" and configure VPC Endpoint Access Policies.
* Audit CodExecute infrastructure security and verify SQS Dead-Letter Queue (DLQ) failover behavior to guarantee submission data integrity.

### Weekly Tasks Breakdown:
| Day | Task Description | Start Date | End Date | Resource Links |
| --- | --- | --- | --- | --- |
| Mon | - **AWS PrivateLink & VPC Endpoints Overview:** Study private endpoint benefits in eliminating public Internet data routing and NAT Gateway bandwidth charges. | 13/07/2026 | 13/07/2026 | [AWS VPC Endpoints Guide](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html) |
| Tue | - **Gateway vs Interface Endpoints Comparison:** Compare free Gateway Endpoints (S3/DynamoDB via Route Tables) against paid Interface Endpoints (ENI Private IPs via Private DNS). | 14/07/2026 | 14/07/2026 | [AWS PrivateLink Docs](https://docs.aws.amazon.com/vpc/latest/privatelink/concepts.html) |
| Wed | - **CodExecute VPC Endpoint Policies:** Craft IAM Resource Policies attached to VPC Endpoints restricting access strictly to CodExecute's 3 project S3 buckets. | 15/07/2026 | 15/07/2026 | [VPC Endpoint Policy Docs](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html) |
| Thu | - **Hybrid Access & SQS Failover Testing:** Study On-Premises connectivity via Interface Endpoints and Route 53 Inbound Resolvers. <br> - Test SQS DLQ Failover: Simulate worker crash scenarios to verify failed messages are safely routed to the SQS DLQ. | 16/07/2026 | 16/07/2026 | [AWS Hybrid Connectivity Guide](https://docs.aws.amazon.com/vpc/latest/privatelink/hybrid-commitments.html) |
| Fri | - **Workshop 3.1 Hands-on & Verification:** Deploy Gateway and Interface VPC Endpoints for S3; execute CLI tests confirming successful `aws s3 ls` queries to allowed buckets and `Access Denied` blocks to unauthorized external buckets. | 17/07/2026 | 17/07/2026 | [AWS S3 VPC Endpoints Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/privatelink-interface-endpoints.html) |

### Week 5 Deliverables & Achievements:
* **Completed Workshop 3.1 VPC Endpoints:** Successfully executed the hands-on lab "Securing Hybrid Access to S3 using VPC Endpoints", deploying Gateway & Interface VPC Endpoints.
* **Secured Infrastructure via Private Routing:** Routed all S3 and DynamoDB data traffic entirely through AWS internal backbone networks, eliminating public Internet vulnerability vectors and cutting NAT Gateway costs.
* **Prevented Data Exfiltration via Endpoint Policies:** Configured strict VPC Endpoint Policies restricting API traffic to CodExecute's 3 S3 Buckets, verifying `Access Denied` enforcement on external buckets.
* **Verified SQS DLQ Failover Resilience:** Simulated worker crash failures during active SQS queue processing, confirming messages auto-retry 3 times and safely transition to the Dead-Letter Queue with zero data loss.
* **Enhanced System Fault Tolerance:** Verified network isolation and data integrity compliance across all CodExecute components.
