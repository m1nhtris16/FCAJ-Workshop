---
title: "Worklog Week 1"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

### Week 1 Objectives (15/06/2026 – 21/06/2026):
* Onboard with mentors and team members of the First Cloud AI Journey (FCAJ) program.
* Configure AWS CLI, IAM user accounts, and launch **Phase 1: Infrastructure & IaC Setup** for the proposed **CodExecute** project.
* Design and provision S3 storage buckets, DynamoDB NoSQL tables, and Amazon VPC network infrastructure for CodExecute using AWS SAM & Terraform IaC templates.

### Weekly Tasks Breakdown:
| Day | Task Description | Start Date | End Date | Resource Links |
| --- | --- | --- | --- | --- |
| Mon | - Meet FCAJ organizers, receive internship roadmap. <br> - Create AWS Free Tier account, install AWS CLI v2, and set **AWS Budgets** alerts ($5/month). <br> - Study **Pure Serverless AWS** architecture for the CodExecute proposal. | 15/06/2026 | 15/06/2026 | [Welcome to FCAJ](https://cloudjourney.awsstudygroup.com/8-fcjworkforce/) <br> [Configure AWS Budgets](https://000007.awsstudygroup.com/) |
| Tue | - Design Amazon VPC (CIDR 10.0.0.0/16), multi-AZ Public and Private Subnet topology. <br> - Configure Internet Gateway, Route Tables, and NAT Gateway for dev environments. | 16/06/2026 | 16/06/2026 | [AWS VPC Setup](https://000003.awsstudygroup.com/) |
| Wed | - **CodExecute Object Storage (Amazon S3):** Provision 3 dedicated S3 Buckets (`frontend-assets`, `testcases-storage`, `user-avatars`). <br> - Enable S3 Versioning, SSE-S3/KMS encryption, and S3 Lifecycle Rules transitioning log archives to Glacier after 90 days. | 17/06/2026 | 17/06/2026 | [Static Website Hosting on S3](https://000057.awsstudygroup.com/) <br> [S3 Security Best Practices](https://000069.awsstudygroup.com/) |
| Thu | - **CodExecute Database Layer (Amazon DynamoDB):** Design NoSQL schemas for 7 core tables (`Users`, `Problems`, `Submissions`, `TestCases`, `Posts`, `Notifications`, `UserFollows`). <br> - Define Partition Keys, Sort Keys, Global Secondary Indexes (GSIs), and select On-Demand capacity mode. | 18/06/2026 | 18/06/2026 | [Amazon DynamoDB Docs](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/) |
| Fri | - **IaC Automation (AWS SAM & Terraform):** Author Infrastructure as Code scripts automating S3, DynamoDB, and VPC provisioning. <br> - Package SAM Template and test `sam deploy` successfully to the AWS Dev environment. | 19/06/2026 | 19/06/2026 | [AWS SAM Developer Guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/) |

### Week 1 Deliverables & Achievements:
* **Environment Setup & Onboarding:** Connected with FCAJ Mentors, installed AWS CLI v2, configured Least-Privilege IAM Users, and configured automated AWS Budgets email alerts for cost safety.
* **CodExecute Infrastructure Design (Phase 1 Init):** Successfully provisioned Custom VPC (10.0.0.0/16) featuring multi-AZ Public/Private Subnets, Internet Gateways, and NAT Gateways for fault-tolerant dev operations.
* **Deployed 3 Enterprise S3 Buckets:** Created and secured 3 S3 Buckets for CodExecute (frontend hosting, testcases repository, user avatars), enforcing SSE-S3/KMS encryption and automated S3 Glacier lifecycle rules.
* **Designed 7 DynamoDB NoSQL Tables:** Finalized NoSQL schema design for 7 core On-Demand DynamoDB tables delivering single-digit millisecond query performance.
* **100% Infrastructure as Code Automation:** Packaged Phase 1 infrastructure into AWS SAM Templates, verifying automated `sam build && sam deploy` executions in under 5 minutes.
