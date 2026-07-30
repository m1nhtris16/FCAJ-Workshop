---
title: "References"
date: 2026-07-30
weight: 8
chapter: false
pre: " <b> 8. </b> "
---

This section consolidates all official links, source code repositories, technical documentation, security standards, and external resources relevant to the **CodExecute** platform and the **AWS First Cloud AI Journey (FCAJ)** internship program.

---

## 1. Repositories & CodExecute Project Resources

| Resource | Official Link | Technical Description |
| :--- | :--- | :--- |
| **Live Application (Production)** | [CodExecute Platform](https://d1hsp5bm4hkjmb.cloudfront.net/) | Live production deployment of CodExecute Online Judge System on AWS CloudFront |
| **Source Code Repository** | [github.com/phuvi301/CodExecute](https://github.com/phuvi301/CodExecute) | Monorepo containing React + Vite Frontend, FastAPI Backend & Lambda Sandbox Worker |
| **Workshop Documentation** | [github.com/m1nhtris16/fcaj-workshop](https://github.com/m1nhtris16/fcaj-workshop) | Hugo Learn Theme internship documentation repository |

---

## 2. AWS Official Technical Documentation

| AWS Service | Official Documentation | Key Reference Topics |
| :--- | :--- | :--- |
| **AWS Lambda** | [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) | Serverless execution, Container Image deployments, Resource limits & Scaling |
| **Amazon API Gateway** | [Amazon API Gateway Guide](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) | REST API configuration, Throttling, CORS, JWT Authorizer & Lambda Integrations |
| **Amazon DynamoDB** | [Amazon DynamoDB Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) | NoSQL Single-Table Design, On-Demand capacity mode, Global Secondary Indexes (GSI) |
| **Amazon S3** | [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) | Static Web Hosting, Presigned URLs, S3 Lifecycle Policies & Security Policies |
| **Amazon SQS** | [Amazon SQS Developer Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) | Asynchronous Queue processing, Long Polling, Visibility Timeout & Dead-Letter Queue (DLQ) |
| **Amazon CloudFront** | [Amazon CloudFront Guide](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) | Global Edge Caching, SSL/TLS 1.3 Custom Domain Certificates, Origin Request Policies |
| **AWS WAF** | [AWS WAF Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html) | Web ACL rules, Rate-based Rules, Managed Rule Groups protecting against SQLi, XSS & DDoS |
| **AWS ECR** | [AWS ECR User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html) | Managing Docker Container Repositories, Image Tagging & native Lambda container deployment |
| **Amazon CloudWatch** | [Amazon CloudWatch Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) | Centralized Logging (CloudWatch Logs), Custom Performance Metrics & Alarms |
| **Amazon SNS** | [Amazon SNS Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) | Publish/Subscribe messaging & Email Incident Notifications triggered by CloudWatch Alarms |
| **AWS IAM** | [AWS IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) | Principle of Least Privilege, Execution Roles, Policy Statements & Resource-based Policies |
| **AWS SAM** | [AWS SAM Developer Guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) | Infrastructure as Code (IaC) YAML templates, Local testing & SAM CLI Deployment |
| **AWS Well-Architected** | [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/) | 5 Architecture Pillars: Operational Excellence, Security, Reliability, Performance, Cost |

---

## 3. Security Standards & Sandbox Isolation Specifications

| Specification | Reference Link | Professional Scope |
| :--- | :--- | :--- |
| **NIST SP 800-207 Zero Trust** | [NIST SP 800-207 Publication](https://csrc.nist.gov/publications/detail/sp/800-207/final) | Zero-Trust Architecture security standards and guidelines |
| **AWS Serverless Security Lens** | [AWS Serverless Security Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/security.html) | Serverless application security best practices and RCE prevention strategies |
| **Linux Seccomp & Resource Limit** | [Linux man-pages (seccomp)](https://man7.org/linux/man-pages/man2/seccomp.2.html) | System Call filtering mechanisms and process resource limits (Memory, CPU, Process limit) |

---

## 4. Performance & Load Testing Tools

| Tool | Homepage / Repository | Usage Rationale |
| :--- | :--- | :--- |
| **Locust** | [locust.io](https://locust.io/) | Asynchronous Python Load Testing Framework (1,000+ Concurrent Virtual Users) |
| **Grafana k6** | [k6.io](https://k6.io/) | High-performance developer-centric API load testing tool |
| **AWS Lambda Power Tuning** | [github.com/alexcasalboni/aws-lambda-power-tuning](https://github.com/alexcasalboni/aws-lambda-power-tuning) | Automated data-driven optimization tool for Lambda memory allocation |

---

## 5. Languages, Compilers & Runtimes

| Language / Engine | Official Website | Version / Compiler Details |
| :--- | :--- | :--- |
| **C++ (GCC/G++)** | [gcc.gnu.org](https://gcc.gnu.org/) | C++20 source code compiler with `-O2` optimization |
| **Java (OpenJDK)** | [openjdk.org](https://openjdk.org/) | Java 17 LTS HotSpot Virtual Machine execution runtime |
| **Python 3** | [python.org](https://www.python.org/) | Python 3.11 CPython interpreter runtime |
| **JavaScript (Node.js)** | [nodejs.org](https://nodejs.org/) | Node.js v20 LTS V8 JavaScript Engine runtime |
| **FastAPI** | [fastapi.tiangolo.com](https://fastapi.tiangolo.com/) | High-performance Python RESTful web framework based on Starlette & Pydantic |
| **React** | [react.dev](https://react.dev/) | Frontend JavaScript library for building interactive user interfaces |
| **Vite** | [vitejs.dev](https://vitejs.dev/) | Next-generation frontend build tool providing fast HMR and optimized builds |

---

## 6. AWS First Cloud AI Journey (FCAJ) Program

| Resource | Link |
| :--- | :--- |
| **FCAJ HCM Portal** | [hcm-portal.awsfcaj.com](https://hcm-portal.awsfcaj.com/) |
| **FCAJ HCM Rules & Guidelines** | [hcm-rules.awsfcaj.com](https://hcm-rules.awsfcaj.com/) |
| **Cloud Journey Learning Hub** | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| **AWS Study Group YouTube** | [youtube.com/@AWSStudyGroup](https://www.youtube.com/@AWSStudyGroup) |
| **AWS Study Group Facebook Group** | [facebook.com/groups/awsstudygroupfcj](https://www.facebook.com/groups/awsstudygroupfcj) |
