---
title: "Introduction & Project Deployment Overview"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---



### 1. CodExecute Project Executive Overview
**CodExecute** is an enterprise-grade Online Judge System and Automated Algorithm Evaluation Platform engineered natively on **Pure Serverless AWS** (Cloud-Native & Event-Driven). The system is built to handle tens of thousands of concurrent programming submission evaluation jobs submitted by competitive programmers and students during contest windows with high reliability, sub-2.0 second evaluation speeds ($< 2.0$s), and ultra-low monthly operational overhead.

This **Workshop section serves as the comprehensive Step-by-Step AWS Deployment Guide**, documenting the complete end-to-end technical procedures for environment setup, VPC networking design, database schema provisioning, serverless application packaging, and official deployment of CodExecute onto the AWS Cloud from scratch.

---

### 2. AWS System Deployment Architecture Overview
The CodExecute platform is deployed across a multi-tier Serverless architecture adhering strictly to the 6 pillars of the **AWS Well-Architected Framework**:

![CodExecute Architecture Diagram](/images/2-Proposal/architect-codexecute.drawio.png)

#### Core AWS Infrastructure Components:
* **Frontend & Edge Distribution Layer (Amazon CloudFront + S3):** Single Page Application UI (React + Vite + TailwindCSS + Monaco Code Editor) compiled and hosted on **Amazon S3** (`frontend-assets`), paired with **Amazon CloudFront CDN** for global edge asset delivery and automated AWS Shield Layer 3/4 DDoS protection.
* **API Entry Gateway & Serverless Backend Layer (API Gateway + AWS Lambda API):** **Amazon API Gateway** (HTTP API) serves as the unified entry point with JWT Token authorization, proxying RESTful requests to an **AWS Lambda API Handler** (built using FastAPI + Mangum Serverless Adapter) for business logic, problem management, and social user interactions.
* **Asynchronous Buffer Queue Layer (Amazon SQS Buffer Queue):** When users submit code, submission payloads are ingested immediately into an **Amazon SQS** queue (`Submissions Queue`). SQS acts as a high-concurrency buffer smoothing traffic spikes, backed by a **Dead-Letter Queue (DLQ)** capturing failed messages to guarantee zero data loss.
* **Isolated Execution Sandbox Worker Layer (AWS Lambda Worker Sandbox):** **AWS Lambda Worker** handlers consume SQS messages, invoking language compiler toolchains to execute 4 supported languages (**C++, Java, Python, JavaScript**). The runtime environment is secured with disabled external network access (`VPC Network: Disabled / Strict Subnet Security Groups`) and hard resource caps (512MB RAM, 5s Timeout), mitigating Remote Code Execution (RCE) attacks.
* **Database & Testcase Storage Layer (DynamoDB + S3):** **Amazon DynamoDB** (7 On-Demand NoSQL tables) stores user records, problem sets, and submission histories with single-digit millisecond query latency. Large testcase files are stored in a dedicated **Amazon S3** bucket (`testcases-storage`).
* **Security & Private Networking Layer (Custom VPC + VPC Endpoints + IAM):** Service-to-service data traffic routes privately through a **Custom VPC** using **VPC Endpoints** (Gateway & Interface Endpoints), avoiding public Internet exposure. Service access permissions are enforced via **AWS IAM Roles** following the Principle of Least Privilege Access.

---

### 3. Step-by-Step AWS Project Deployment Roadmap
The deployment guide for CodExecute on AWS is structured into 5 practical implementation phases matching the Workshop modules:

| Deployment Step | Workshop Module | Infrastructure Tasks Executed |
| --- | --- | --- |
| **Step 1** | **5.2 - Preparation & Network VPC** | Provision AWS Free Tier account, create IAM Users, install AWS CLI v2 / SAM CLI, and design Amazon Custom VPC topology (multi-AZ Public/Private Subnets, IGW, Route Tables). |
| **Step 2** | **5.3 - Storage & Database Layer** | Create 3 dedicated S3 Buckets, configure S3 Lifecycle Rules, enable SSE-S3/KMS encryption, design 7 DynamoDB NoSQL tables, and attach VPC Gateway/Interface Endpoints for S3 & DynamoDB. |
| **Step 3** | **5.4 - Serverless API & SQS Buffer** | Develop FastAPI Backend, wrap Mangum Serverless Adapter on AWS Lambda API Handler, configure API Gateway HTTP API routes, and deploy Amazon SQS `Submissions Queue` with Dead-Letter Queue (DLQ) failover. |
| **Step 4** | **5.5 - Execution Worker Sandbox & Security** | Program AWS Lambda Worker Runners for 4 language toolchains, enforce container sandbox boundaries against RCE, attach Least-Privilege IAM Roles, and deploy CloudFront CDN for React Frontend. |
| **Step 5** | **5.6 - Optimization, Testing & Cleanup** | Conduct load testing (1,000 VUs via Locust/k6), optimize compute RAM/Timeout with AWS Lambda Power Tuning, configure CloudWatch Slack Alarms, and script automated resource cleanup procedures. |

---

### 4. Key Expected Deployment Outcomes
* **100% Serverless Cloud-Native Infrastructure:** Auto-scales effortlessly from 0 to thousands of concurrent submissions without managing EC2 servers.
* **Ultra-Low Response Latencies:** API Latency $< 200ms$, submission evaluation duration $< 2.0$ seconds.
* **Hardened Zero-Trust Security:** Zero static access keys stored, 100% user code execution isolated in Lambda Sandboxes, eliminating RCE risks.
* **Outstanding Cost Efficiency:** Estimated ongoing operational budget of **~$15.43 USD/month** (maintained under **< $3.00 USD/month** during Year 1 under AWS Free Tier).