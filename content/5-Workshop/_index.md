---
title: "Workshop"
date: 2026-06-15
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->


# CODEXECUTE PROJECT AWS DEPLOYMENT GUIDE

#### Overview

The **Workshop** section serves as a step-by-step technical deployment manual for executing and deploying the **CodExecute project (Online Judge System & Automated Algorithm Evaluation Platform)** onto **Pure Serverless AWS** cloud infrastructure from scratch.

The entire deployment workflow is standardized using Infrastructure as Code (AWS SAM / Terraform Templates), strictly adhering to the 6 pillars of the **AWS Well-Architected Framework**:
* **Operational Excellence:** 100% automated infrastructure provisioning and updates via AWS SAM templates.
* **Security:** Zero-Trust model with Least Privilege IAM Roles, private VPC Endpoints, and hardened Lambda Execution Sandboxes protecting against RCE.
* **Reliability:** Multi-AZ fault tolerance, Amazon SQS buffering, and Dead-Letter Queue (DLQ) failover guaranteeing zero user data loss.
* **Performance Efficiency:** API latency $< 200ms$ and submission processing speeds $< 2.0$s powered by CloudFront CDN Edge locations and DynamoDB NoSQL.
* **Cost Optimization:** Event-Driven Pure Serverless architecture reducing monthly operating budget down to **~$15.43 USD/month**.
* **Sustainability:** Optimized compute cycles minimizing hardware power consumption.

---

#### Deployment Roadmap & Modules

1. [Introduction & Project Deployment Overview](5.1-Workshop-overview/)
2. [Step 1: Environment Setup & Custom VPC Network Provisioning](5.2-Prerequiste/)
3. [Step 2: S3 Storage, DynamoDB NoSQL & VPC Endpoints Deployment](5.3-S3-vpc/)
4. [Step 3: Serverless API & SQS Buffer Queue Deployment](5.4-S3-onprem/)
5. [Step 4: Lambda Worker Sandbox & IAM Security Hardening](5.5-Policy/)
6. [Step 5: Cost Optimization, Load Testing & Resource Cleanup](5.6-Cleanup/)