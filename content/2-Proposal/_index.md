---
title: "Project Proposal"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

# CODEXECUTE - ONLINE JUDGE SYSTEM & AUTOMATED ALGORITHM EVALUATION PLATFORM


## 1. PROJECT OVERVIEW

**CodExecute** is a modern Online Judge Platform and developer social network designed to solve code testing, algorithm practice, and real-time developer skill evaluation challenges.

Built 100% on **Pure Serverless Cloud-Native AWS** infrastructure, CodExecute leverages **AWS Lambda** as both its RESTful backend engine and its isolated code execution sandbox, delivering High Availability, Scale-to-Zero capability, and a Secure Isolated Sandbox Execution environment for user-submitted source code.

![CodExecute Platform Architecture](/images/2-Proposal/codexecute_platform_structure.png)

---

## 2. PROJECT OBJECTIVES

* **Elastic Scalability:** Auto-scaling Serverless architecture capable of handling hundreds to tens of thousands of concurrent code submissions during contests without congestion or system failure.
* **Absolute Isolation & Security:** Execute unverified user code within resource-constrained Lambda sandbox environments (CPU, RAM, Execution Time, Network isolation) to mitigate Remote Code Execution (RCE) and infrastructure compromise risks.
* **Low-Latency Real-Time Response:** Optimize the submission pipeline to deliver evaluation results (Accepted/Wrong Answer/Time Limit Exceeded/Memory Limit Exceeded) within under 2 seconds for standard problems.
* **Cost Optimization:** Rigorously adopt a *Pay-As-You-Go* serverless model, paying strictly for consumed compute/storage resources and eliminating 24/7 idle server overhead.
* **Seamless User Experience (UX/UI):** Deliver an intuitive interface featuring a professional interactive Code Editor supporting multiple programming languages (Python, C++, Java, JavaScript), categorized problem sets, and social network features (Posts, Comments, Follow System, Achievement Badges).

---

## 3. PROBLEM STATEMENT & SOLUTIONS

| Traditional System Bottlenecks | CodExecute AWS Cloud Solution |
| :--- | :--- |
| **Security Risks (RCE):** Unvetted user code can execute system file deletion commands, mine cryptocurrency, or attack internal networks. | **RCE Prevention with Lambda Sandbox:** Every submission runs inside a completely isolated Lambda Worker execution environment with disabled external network access and strict resource boundaries (RAM, Timeout, Process Limits). |
| **Peak Traffic Bottlenecks:** Sudden submission spikes overload traditional servers, leading to system crashes. | **Asynchronous Queueing with Amazon SQS:** Submissions are queued to buffer traffic spikes, allowing Lambda Workers to auto-scale smoothly based on queue depth. |
| **High Idle Server Costs:** Provisioning high-spec 24/7 servers incurs significant cost even during off-peak hours with zero traffic. | **Serverless Pay-As-You-Go (Lambda + API Gateway + DynamoDB):** Infrastructure automatically scales to zero when idle, reducing infrastructure costs by up to 75%. |
| **Massive Testcase Dataset Management:** Multi-megabyte or gigabyte testcase files strain relational database engines (RDBMS). | **Tiered Storage (S3 & DynamoDB):** Problem metadata is stored in high-speed DynamoDB NoSQL tables, while large input/output testcases reside in Amazon S3 for fast retrieval. |

---

## 4. KEY PLATFORM FEATURES

1. **Problemset Management & Tagging System:**
   - Search and filter problems by title, difficulty (Easy, Medium, Hard), and algorithmic topics (Binary Tree, Dynamic Programming, Graph, Arrays, etc.).
   - Track problem acceptance rates and total submission counts.

2. **Interactive Code Editor & Runner:**
   - Compile and execute code across 4 popular languages: **C++, Java, Python, JavaScript**.
   - Dual execution modes: **Run Code** (custom user inputs) and **Submit Code** (full testcase suite grading).
   - Automated detection of Compile Errors, Runtime Errors, Time Limit Exceeded (TLE), and Memory Limit Exceeded (MLE).

3. **Automated Isolated Sandbox Execution:**
   - Strict separation between API ingestion handlers and execution sandbox runners.
   - Character-by-character and line-by-line automated output verification against ground truth solutions.

4. **Developer Social Network:**
   - Knowledge sharing posts, solution discussion threads, user follow system.
   - Gamified user achievement badges and level progression.

5. **User Dashboard & Performance Analytics:**
   - Interactive submission heatmaps (GitHub-style activity contributions).
   - Comprehensive submission history logs detailing execution runtimes and memory consumption metrics.

---

## 5. SYSTEM ARCHITECTURE 

### 5 Pillars of AWS Well-Architected Framework
CodExecute's architecture adheres strictly to the 5 pillars of the **AWS Well-Architected Framework**:

1. **Operational Excellence:** Infrastructure as Code (IaC SAM/CloudFormation) deployment pipelines, centralized logging and telemetry using Amazon CloudWatch.
2. **Security:** Least-Privilege IAM Roles, Data Encryption At-Rest (KMS) & In-Transit (TLS 1.3/HTTPS via CloudFront), isolated Lambda execution sandbox.
3. **Reliability:** Multi-AZ High Availability fault tolerance, automated retry routines, and SQS Dead-Letter Queue (DLQ) safeguards.
4. **Performance Efficiency:** Global edge distribution via CloudFront CDN, single-digit millisecond latency NoSQL queries with Amazon DynamoDB.
5. **Cost Optimization:** Strict Event-Driven Pure Serverless architecture (Lambda Pay-As-You-Go).

### Project Architecture Diagram
![CodExecute Architecture Diagram](/images/2-Proposal/architect-codexecute.drawio.png)

Below is the inventory of all **8 core AWS services** implemented in CodExecute, detailing their specific role and technical selection rationale:

| No. | AWS Service | Role & Responsibilities in CodExecute | Selection Rationale & Technical Benefits |
| :---: | :--- | :--- | :--- |
| **1** | **Amazon CloudFront** | Distributes React Frontend static assets globally from S3 with ultra-low latency. Acts as reverse proxy routing `/api/*` requests to API Gateway. | Accelerates global page load times using edge caching. Enables automated HTTPS/TLS 1.3 encryption and built-in AWS Shield Layer 3/4 DDoS protection. |
| **2** | **Amazon S3** | **Bucket 1:** Frontend static hosting (HTML/JS/CSS).<br>**Bucket 2:** Testcase input/output file repository.<br>**Bucket 3:** User profile media storage. | Ultra-cost-effective storage ($0.023/GB), 99.999999999% (11 9's) durability. Infinite capacity scaling and secure Presigned URL upload capabilities. |
| **3** | **Amazon DynamoDB** | Stores all structured platform data: `Users`, `Problems`, `Submissions`, `TestCases`, `Posts`, `Notifications`, and `UserFollows` tables. | Single-digit millisecond query performance. On-Demand auto-scaling handles millions of queries without complex cluster management or sharding. |
| **4** | **AWS Lambda** | **Lambda API:** Runs FastAPI RESTful backend logic.<br>**Lambda Worker:** Consumes messages from SQS, compiles and executes untrusted code (C++, Java, Python, JS) in an isolated Sandbox runtime. | Millisecond-level billing precision (pay only when code executes). Automatic scaling from 0 to thousands of concurrent executions without server or container cluster management. |
| **5** | **Amazon SQS** | Asynchronous submission queue (`Submissions Queue`) decoupling API handlers from background code evaluation workers. | Buffers traffic spikes during competitive programming contests. Prevents submission data loss using message retention and Dead-Letter Queue (DLQ) patterns. |
| **6** | **AWS IAM** | Manages identity, enforcing Least-Privilege Access control for Lambda, S3, DynamoDB, and SQS cross-service communications. | Implements Zero-Trust security model. Prevents unauthorized privilege escalation across platform components via IAM Roles and Resource Policies. |
| **7** | **Amazon CloudWatch** | Centralizes logging (CloudWatch Logs) from Lambda API & Worker, and API Gateway. Tracks performance metrics and sets up proactive Alarms. | Real-time system observability. Triggers automated alarms when error rates exceed 1% or compute resources saturate, accelerating root-cause debugging. |
| **8** | **Amazon API Gateway** | Unified entry point (API Gateway) managing RESTful API endpoints and routing incoming HTTP requests to Lambda API handlers. | Built-in Rate Limiting / Throttling preventing API abuse, JWT authentication integration, seamless CORS and CloudFront custom domain binding. |

---

## 8. IMPLEMENTATION ROADMAP & TIMELINE 

The project is executed over a **7-week duration** (from **June 15, 2026** to **July 31, 2026**) structured into **5 distinct phases**:

* **Phase 1: Infrastructure & IaC Setup** *(15/06/2026 – 24/06/2026)*
  > Provision S3 Buckets, DynamoDB Tables, SQS Queues, and IAM Roles using AWS SAM & Terraform scripts.

* **Phase 2: Frontend & Backend Core Development** *(25/06/2026 – 05/07/2026)*
  > Develop React + Vite UI (Interactive Code Editor, Problemset) & FastAPI RESTful APIs on AWS Lambda.

* **Phase 3: Asynchronous Pipeline & Lambda Sandbox** *(06/07/2026 – 16/07/2026)*
  > Implement Amazon SQS queues and deploy isolated AWS Lambda Workers for automated code evaluation.

* **Phase 4: Load Testing & Security Hardening** *(17/07/2026 – 25/07/2026)*
  > Conduct load testing (1,000 VUs via Locust/k6), security penetration testing against RCE, and verify DLQ failover behavior.

* **Phase 5: Cost Optimization & Production Go-Live** *(26/07/2026 – 31/07/2026)*
  > Fine-tune Lambda RAM/Timeouts, set CloudWatch Budget Alarms, and officially **GO-LIVE on July 31, 2026**.

### Phase Details

| Phase | Timeline | Key Tasks | Deliverables |
| :--- | :--- | :--- | :--- |
| **Phase 1: Infrastructure & IaC Setup** | **15/06/2026 – 24/06/2026** *(10 days)* | - Design AWS Well-Architected system architecture.<br>- Author AWS SAM / Terraform scripts provisioning S3 Buckets, DynamoDB Tables, SQS Queues, IAM Roles.<br>- Setup repository structure and GitHub Actions CI/CD pipelines. | - Operational AWS Dev environment.<br>- Complete SAM Template codebase. |
| **Phase 2: Frontend & Backend Core Development** | **25/06/2026 – 05/07/2026** *(11 days)* | - Develop React + Vite Frontend UI (Code Editor, Problem Browser, User Profiles).<br>- Develop FastAPI Backend endpoints (JWT Auth, Problems API, Submissions API).<br>- Package Lambda API Handler and integrate with API Gateway & CloudFront. | - Web UI deployed on CloudFront + S3.<br>- Functional RESTful APIs powered by Lambda & DynamoDB. |
| **Phase 3: Asynchronous Pipeline & Lambda Sandbox** | **06/07/2026 – 16/07/2026** *(11 days)* | - Implement SQS job enqueueing pipeline.<br>- Build multi-language Lambda Sandbox runner environment (C++, Java, Python, JS).<br>- Deploy AWS Lambda Workers to poll jobs from SQS, parse testcases, and evaluate submissions. | - Automated async code evaluation workflow complete.<br>- Verified RCE protection in Lambda Sandbox. |
| **Phase 4: Testing & Security Hardening** | **17/07/2026 – 25/07/2026** *(9 days)* | - Load testing using Locust/k6 simulating up to 1,000 concurrent Virtual Users.<br>- Security penetration testing (RCE, SQLi, XSS, Resource Exhaustion).<br>- Resilience testing (Failover mechanism and DLQ verification). | - Load test report confirming SLA metrics.<br>- 100% resolution of High/Critical security vulnerabilities. |
| **Phase 5: Optimization & Production Go-Live** | **26/07/2026 – 31/07/2026** *(6 days)* | - Configure CloudWatch Alarms & AWS Cost Budgets Alerts.<br>- Fine-tune Lambda RAM sizing and timeout limits.<br>- Switch over to Production environment and deliver full documentation. | - **OFFICIAL PRODUCTION GO-LIVE on July 31, 2026.**<br>- Finalized Architecture & Operations documentation. |

---

## 9. BUDGET ESTIMATION & COST OPTIMIZATION ASSESSMENT

### 1. Monthly Cost Estimate

Budget calculations are based on an average monthly workload of **100,000 code submissions** and **500,000 API requests**.

| AWS Service | Anticipated Monthly Usage | Unit Price Reference (ap-southeast-1) | Monthly Cost (USD) |
| :--- | :--- | :--- | :---: |
| **AWS Lambda** | 500k API Requests + 100k Worker Sandbox executions (Memory: 512MB, Avg duration: 800ms) | $0.20 / 1M Requests + Compute time | **$3.80** |
| **Amazon API Gateway** | 500,000 HTTP API calls | $1.00 / 1M Requests | **$0.50** |
| **Amazon SQS** | 200,000 SQS Requests (SendMessage + ReceiveMessage) | $0.40 / 1M Requests | **$0.08** |
| **Amazon DynamoDB** | 1,000,000 Read/Write Units (On-Demand Mode) + 5GB Data Storage | $0.25 / 1M WCU, $0.05 / 1M RCU | **$3.20** |
| **Amazon S3** | 15GB Storage (Testcases + Media + Assets) + 100k GET/PUT | $0.023 / GB | **$0.65** |
| **Amazon CloudFront** | 50GB Data Transfer Out + 500k HTTPS Requests | $0.09 / GB | **$4.50** |
| **Amazon CloudWatch** | 3GB Ingestion Logs + 5 Custom Metrics + 3 Alarms | $0.57 / GB Logs | **$2.70** |
| **AWS IAM** | All IAM Users, Roles, Policies | Free | **$0.00** |
| **TOTAL ESTIMATED MONTHLY COST:** | | | **~$15.43 USD / month** |

> 💡 *Note:* During the first 12 months, the majority of costs will fall under the **AWS Free Tier** (1M Lambda requests, 1M API Gateway requests, 25GB DynamoDB storage, 5GB S3 storage), bringing actual expenses down to **< $3.00 USD/month**.

---

### 2. Cost Optimization Assessment

1. **Pure Serverless Pay-As-You-Go Architecture:**
   - Leveraging **AWS Lambda** and **API Gateway HTTP APIs** (70% cheaper than standard REST APIs) eliminates idle server maintenance costs during non-traffic periods.

2. **DynamoDB On-Demand vs. Provisioned Capacity:**
   - Initial deployment uses **On-Demand Capacity** for strict pay-per-request pricing. Once traffic patterns stabilize, transitioning high-traffic tables to **Provisioned Capacity + Auto Scaling** yields an additional 40% cost reduction.

3. **S3 Lifecycle Rules & Compression:**
   - Automated **S3 Lifecycle Rules** transition testcase execution logs older than 90 days to **S3 Glacier Flexible Retrieval** (saving 85% on storage costs).
   - Testcases are compressed using Gzip prior to S3 upload, minimizing bandwidth consumption.

4. **SQS Long Polling Implementation:**
   - Setting SQS `ReceiveMessageWaitTimeSeconds = 20` (Long Polling) reduces empty receive requests, cutting SQS API call fees by up to 90%.

5. **AWS Lambda Power Tuning:**
   - Utilizing *AWS Lambda Power Tuning* optimizes memory and vCPU allocation against execution latency, reducing overall compute costs for both API and Worker functions.

---

## 10. RISK ASSESSMENT & MITIGATION MATRIX

| No. | Risk Category | Detailed Risk Analysis | Severity | Mitigation Strategy |
| :---: | :--- | :--- | :---: | :--- |
| **1** | **Security** | **RCE in Execution Sandbox:** Malicious user code attempts file system manipulation, fork bomb CPU exhaustion, or internal AWS network access. | **CRITICAL** | - Execute code inside non-root **Lambda Isolated Sandbox**.<br>- Disable external network access (`VPC Network: Disabled / Strict Security Group Rules`).<br>- Enforce hard resource limits: RAM (max 512MB), CPU (max 1 Core), Execution Timeout (max 5s), Process limit (max 20 pids). |
| **2** | **Performance** | **SQS Queue Congestion / Lambda Cold Start:** High submission traffic bursts (thousands/minute) or cold starts increase response latency. | **HIGH** | - Enable **Provisioned Concurrency** for critical Lambda functions during contest peak hours.<br>- Auto-scale worker execution instances based on SQS `ApproximateNumberOfMessagesVisible` metrics. |
| **3** | **Security** | **DDoS / API Abuse Attacks:** Malicious traffic floods target platform endpoints to cause financial exhaustion. | **HIGH** | - Enable **AWS WAF** with strict **API Gateway Throttling / Rate Limiting** (max 20 requests/minute per IP/User).<br>- Configure CloudFront Geo Blocking and AWS Shield Standard. |
| **4** | **Operations** | **Data Loss During Worker Crashes:** Worker instances crash mid-evaluation when processing SQS messages. | **MEDIUM** | - Configure appropriate SQS `VisibilityTimeout`.<br>- Enable **Dead-Letter Queue (DLQ)**: Failed messages retried > 3 times are safely redirected to DLQ for developer inspection without data loss. |
| **5** | **Cost** | **Unexpected Cost Spikes:** Infinite loops in Lambda handlers or excessive log writes to CloudWatch. | **MEDIUM** | - Set **AWS Budgets Alerts** triggering automatic Slack/Email warnings if monthly costs exceed $20 USD.<br>- Set CloudWatch Log Retention policy to 14 days maximum. |

---

## 11. EXPECTED OUTCOMES & KPIS

Following final deployment on **July 31, 2026**, the **CodExecute** platform is expected to achieve the following technical performance metrics and business objectives:

### Technical KPIs
* **System Availability SLA:** **99.9%** uptime guaranteed by Multi-AZ Serverless infrastructure.
* **API Response Latency:** < 200ms for standard data read/write transactions via API Gateway & Lambda.
* **Submission Evaluation Speed:** < 2.0 seconds from the moment a user clicks Submit to receiving grading results (for testcases under 50MB).
* **Concurrency Handling Capacity:** Smoothly handles at least **1,000 concurrent submissions** without request drops or system degradation.
* **Zero Security Vulnerabilities:** 100% of user code executed in strict container isolation with zero RCE incidents.

### Business Outcomes
* **Infrastructure Cost Savings:** Over **75%** operational cost reduction compared to legacy dedicated EC2 server hosting.
* **High Maintainability:** 100% Infrastructure as Code management (SAM/CloudFormation) allows provisioning new staging environments in under 15 minutes.
* **Superior User Experience:** Provides competitive programmers with a reliable, standardized execution engine, elevating coding skills and interview preparation capabilities.

---