---
title: "Tài liệu tham khảo"
date: 2026-07-30
weight: 8
chapter: false
pre: " <b> 8. </b> "
---

Mục này tổng hợp toàn bộ các liên kết chính thức, kho mã nguồn, tài liệu kỹ thuật chuẩn hoá và các nguồn tham khảo bên ngoài liên quan đến hệ thống **CodExecute** và chương trình thực tập **AWS First Cloud AI Journey (FCAJ)**.

---

## 1. Kho Mã Nguồn & Dự Án CodExecute

| Tài nguyên | Liên kết chính thức | Mô tả kỹ thuật |
| :--- | :--- | :--- |
| **Nền tảng trực tiếp (Live App)** | [CodExecute Platform](https://d1hsp5bm4hkjmb.cloudfront.net/) | Phiên bản Production của CodExecute Online Judge System trên AWS CloudFront |
| **Kho mã nguồn (Monorepo)** | [github.com/phuvi301/CodExecute](https://github.com/phuvi301/CodExecute) | Toàn bộ mã nguồn React + Vite Frontend, FastAPI Backend & Lambda Sandbox Worker |
| **Workshop Repository** | [github.com/m1nhtris16/fcaj-workshop](https://github.com/m1nhtris16/fcaj-workshop) | Mã nguồn tài liệu báo cáo thực tập xây dựng trên nền tảng Hugo Learn Theme |

---

## 2. Tài Liệu Kỹ Thuật Chính Thức AWS

| Dịch vụ AWS | Tài liệu chính thức | Nội dung tham chiếu chính |
| :--- | :--- | :--- |
| **AWS Lambda** | [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) | Môi trường thực thi Serverless, Container Image support, Resource limits & Scaling |
| **Amazon API Gateway** | [Amazon API Gateway Guide](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) | Cấu hình REST API, Throttling, CORS, JWT Authorizer & Integration với Lambda |
| **Amazon DynamoDB** | [Amazon DynamoDB Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) | Thiết kế NoSQL Single-Table, On-Demand capacity mode, Secondary Indexes (GSI) |
| **Amazon S3** | [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) | Static Web Hosting, Presigned URLs, S3 Lifecycle policies & Bucket security policies |
| **Amazon SQS** | [Amazon SQS Developer Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) | Asynchronous Queue processing, Long Polling, Visibility Timeout & Dead-Letter Queue (DLQ) |
| **Amazon CloudFront** | [Amazon CloudFront Guide](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) | Edge Caching, Custom Domain SSL/TLS 1.3 certificate, Origin Request Policies |
| **AWS WAF** | [AWS WAF Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html) | Web ACL rules, Rate-based Rules, Managed Rule Groups bảo vệ chống SQLi, XSS & DDoS |
| **AWS ECR** | [AWS ECR User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html) | Quản lý Docker Container Repositories, Image Tagging & tích hợp kéo Image vào Lambda |
| **Amazon CloudWatch** | [Amazon CloudWatch Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) | Centralized Logging (CloudWatch Logs), Custom Performance Metrics & Alarm alerts |
| **Amazon SNS** | [Amazon SNS Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) | Publish/Subscribe messaging, Email Incident notifications từ CloudWatch Alarms |
| **AWS IAM** | [AWS IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) | Principle of Least Privilege, Execution Roles, Policy Statements & Resource-based Policies |
| **AWS SAM** | [AWS SAM Developer Guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) | Khai báo hạ tầng Serverless bằng kịch bản YAML/JSON, Local testing & Deployment CLI |
| **AWS Well-Architected** | [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/) | 5 trụ cột tối ưu kiến trúc: Operational Excellence, Security, Reliability, Performance, Cost |

---

## 3. Tiêu Chuẩn Bảo Mật & Môi Trường Sandbox

| Tài liệu tiêu chuẩn | Liên kết tham chiếu | Mô tả chuyên môn |
| :--- | :--- | :--- |
| **NIST SP 800-207 Zero Trust** | [NIST SP 800-207 Publication](https://csrc.nist.gov/publications/detail/sp/800-207/final) | Tiêu chuẩn kiến trúc an ninh bảo mật Zero-Trust |
| **AWS Serverless Security Lens** | [AWS Serverless Security Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/security.html) | Hướng dẫn thực hành tốt nhất về bảo mật Serverless & chống tấn công RCE |
| **Linux Seccomp & Resource Limit** | [Linux man-pages (seccomp)](https://man7.org/linux/man-pages/man2/seccomp.2.html) | Cơ chế giới hạn System Call và tài nguyên tiến trình (Memory, CPU, Process limit) |

---

## 4. Công Cụ Kiểm Thử Chịu Tải & Tối Ưu Hiệu Năng

| Công cụ | Trang chủ / Repository | Công dụng trong dự án |
| :--- | :--- | :--- |
| **Locust** | [locust.io](https://locust.io/) | Kiểm thử chịu tải bất đồng bộ Python (Load Testing 1,000+ Concurrent Virtual Users) |
| **Grafana k6** | [k6.io](https://k6.io/) | Công cụ kiểm thử hiệu năng API high-performance bằng JavaScript |
| **AWS Lambda Power Tuning** | [github.com/alexcasalboni/aws-lambda-power-tuning](https://github.com/alexcasalboni/aws-lambda-power-tuning) | Công cụ tinh chỉnh tự động dung lượng RAM/vCPU tối ưu nhất cho Lambda |

---

## 5. Ngôn Ngữ Lập Trình, Compilers & Runtimes

| Ngôn ngữ / Engine | Trang chủ chính thức | Phiên bản / Compiler |
| :--- | :--- | :--- |
| **C++ (GCC/G++)** | [gcc.gnu.org](https://gcc.gnu.org/) | Biên dịch mã nguồn C++20 với tối ưu hóa `-O2` |
| **Java (OpenJDK)** | [openjdk.org](https://openjdk.org/) | Trình thực thi Java 17 LTS HotSpot Virtual Machine |
| **Python 3** | [python.org](https://www.python.org/) | Trình thông dịch Python 3.11 CPython Runtime |
| **JavaScript (Node.js)** | [nodejs.org](https://nodejs.org/) | Trình thực thi Node.js v20 LTS V8 JavaScript Engine |
| **FastAPI** | [fastapi.tiangolo.com](https://fastapi.tiangolo.com/) | Web Framework Python tốc độ cao dựa trên Starlette & Pydantic |
| **React** | [react.dev](https://react.dev/) | Thư viện giao diện người dùng xây dựng UI tương tác |
| **Vite** | [vitejs.dev](https://vitejs.dev/) | Frontend Build Tool thế hệ mới cho phản hồi cực nhanh |

---

## 6. Chương Trình AWS First Cloud AI Journey (FCAJ)

| Kênh thông tin | Liên kết truy cập |
| :--- | :--- |
| **FCAJ HCM Portal** | [hcm-portal.awsfcaj.com](https://hcm-portal.awsfcaj.com/) |
| **Quy định & Thể lệ FCAJ HCM** | [hcm-rules.awsfcaj.com](https://hcm-rules.awsfcaj.com/) |
| **Cloud Journey Learning Hub** | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| **Kênh YouTube AWS Study Group** | [youtube.com/@AWSStudyGroup](https://www.youtube.com/@AWSStudyGroup) |
| **Cộng đồng Facebook AWS Study Group** | [facebook.com/groups/awsstudygroupfcj](https://www.facebook.com/groups/awsstudygroupfcj) |
