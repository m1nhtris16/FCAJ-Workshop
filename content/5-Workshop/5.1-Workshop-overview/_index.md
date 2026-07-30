---
title: "Introduction & Overview"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

### Introduction to CodExecute

**CodExecute** is a modern Online Judge platform and developer social network designed to compile, execute, and evaluate user-submitted code (C++, Java, Python, JavaScript) in real time.

Built on a **Pure Serverless Cloud-Native AWS** architecture, CodExecute addresses 3 primary operational challenges of traditional online judge systems:
1. **RCE Prevention & Sandbox Security:** Fully isolating unverified user code within an **AWS Lambda Worker Sandbox** runtime with restricted permissions and disabled outbound network access.
2. **Handling Submission Spikes:** Utilizing **Amazon SQS** as an asynchronous buffer queue to absorb traffic spikes without system degradation or downtime.
3. **Cost Optimization (Pay-As-You-Go):** Auto-scaling infrastructure from 0 (Scale-to-Zero) during idle hours, reducing server costs by up to 75%.

---

## 2. Detailed System Architecture Layers & Execution Flow

The CodExecute system is designed as a fully layered architecture in the AWS `ap-southeast-1` Region, coordinating components through steps 1 to 9:

### 2.1. CDN & Edge Security Layer
* **Amazon CloudFront & AWS WAF:** Receives HTTP requests from user browsers, authenticates via OAuth Providers (Google, GitHub). CloudFront distributes static assets from S3 and routes API requests (`/api/*`) to API Gateway.

### 2.2. Ingress & Static Hosting Layer
* **AWS S3 Bucket (Static Hosting):** Stores compiled static web assets of the React application (HTML/JS/CSS).
* **AWS API Gateway (REST API):** Accepts API requests from CloudFront and synchronously invokes (**Synchronous Invoke**) the Lambda API Handler.

### 2.3. Serverless Compute & Sandbox Layer
* **AWS ECR (Container Registry):** Stores Docker container images for Lambda functions.
* **AWS Lambda (API Handler):**
  * Manages user and problem data in DynamoDB (*Step 5a*).
  * Reads sample testcases and user avatars from S3 (*Step 5b, 5c*).
  * Pushes submission jobs (**Push Execution Job**) to SQS (*Step 6*).
* **AWS Lambda (Code Executor Sandbox):**
  * Triggered by event notifications (**Event Trigger**) from SQS (*Step 7*).
  * Fetches full testcase suites from S3 (*Step 8*).
  * Compiles and evaluates code inside an isolated sandbox environment.
  * Writes submission evaluation results (**Save Result**) to DynamoDB (*Step 9*).

### 2.4. Queue Processing Layer
* **AWS SQS (Submission Queue):** Asynchronous buffer queue holding submission jobs, regulating throughput and triggering the Lambda Code Executor.

### 2.5. Database & Storage Layer
* **AWS DynamoDB (Submission & Problem):** High-performance NoSQL database storing user profiles, problemsets, and evaluation results.
* **AWS S3 Bucket (Testcases & User Avatar):** Two independent S3 Buckets storing problem testcase files and user profile avatars.

### 2.6. Security & Monitoring Layer
* **IAM Roles (Execution Role):** Enforces Least Privilege Access across execution services.
* **AWS CloudWatch (Logs & Metrics) & AWS SNS:** Collects execution logs, monitors performance metrics, and publishes incident notification alerts.

---

<div align="center">

<img src="/images/architect-codexecute.png" alt="CodExecute Architecture Diagram" style="width: 90%; max-width: 1200px; border-radius: 6px;">

<p style="font-size: 1.15rem; font-weight: 600; margin-top: 8px;">
<i>Figure 1: Detailed CodExecute System Architecture on AWS Serverless</i>
</p>

<img src="/images/5-Workshop/5.1-Workshop-overview/project_overview.png" alt="CodExecute Web Application Interface" style="width: 80%; max-width: 1100px; border-radius: 6px;">
<p style="font-size: 1.15rem; font-weight: 600; margin-top: 8px;">
<i>Figure 2: CodExecute Web Application Interface</i></br>
<i>Live Web: </i><a href="https://d1hsp5bm4hkjmb.cloudfront.net">https://d1hsp5bm4hkjmb.cloudfront.net</a>
</p>

</div>
