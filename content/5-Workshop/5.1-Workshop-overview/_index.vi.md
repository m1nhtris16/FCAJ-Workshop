---
title: "Giới thiệu & Tổng quan"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

## 1. Giới Thiệu Dự Án CodExecute

**CodExecute** là giải pháp nền tảng chấm bài trực tuyến (Online Judge Platform) và mạng xã hội lập trình viên hiện đại, cho phép biên dịch, thực thi và đánh giá kết quả mã nguồn của người dùng (C++, Java, Python, JavaScript) theo thời gian thực.

Dự án được thiết kế chuẩn theo mô hình **Pure Serverless Cloud-Native AWS**, tập trung giải quyết 3 bài toán lớn của các hệ thống chấm bài truyền thống:
1. **Chống RCE (Remote Code Execution) & Đảm bảo an toàn hệ thống:** Cách ly hoàn toàn mã nguồn chưa kiểm duyệt của người dùng trong môi trường **AWS Lambda Worker Sandbox** không có quyền root và bị chặn truy cập mạng ngoài.
2. **Xử lý dồn dập bài nộp (Traffic Spikes):** Sử dụng hàng chờ **Amazon SQS** làm vùng đệm điều tiết lưu lượng nộp bài, giúp hệ thống không bị crash hay nghẽn cổ chai.
3. **Tối ưu chi phí vận hành (Pay-As-You-Go):** Tự động thu phóng tài nguyên từ 0 (Scale-to-Zero) khi không có lượt nộp bài, cắt giảm đến 75% chi phí máy chủ nhàn rỗi.

---

## 2. Chi Tiết Mô Hình Kiến Trúc Hệ Thống (Architecture Layers)

Hệ thống CodExecute được thiết kế phân lớp hoàn chỉnh trên AWS Region `ap-southeast-1`, phối hợp giữa các thành phần theo luồng xử lý từ 1 đến 9:

### 2.1. Lớp CDN & Bảo mật bề mặt (CDN Layer)
* **Amazon CloudFront & AWS WAF:** Tiếp nhận HTTP Request từ người dùng (User Browser), xác thực qua OAuth Providers (Google, GitHub). CloudFront phân phối các tệp tĩnh (Static Files) từ S3 và điều hướng API requests (`/api/*`) tới API Gateway.

### 2.2. Lớp Cổng tiếp nhận & Host tĩnh (Ingress & Static Hosting Layer)
* **AWS S3 Bucket (Static Hosting):** Lưu trữ mã nguồn tĩnh đóng gói của ứng dụng React (HTML/JS/CSS).
* **AWS API Gateway (REST API):** Tiếp nhận yêu cầu API từ CloudFront và gọi đồng bộ (**Synchronous Invoke**) tới hàm Lambda API Handler.

### 2.3. Lớp Tính toán Serverless & Môi trường Sandbox (Serverless Compute & Sandbox Layer)
* **AWS ECR (Container Registry):** Lưu trữ Container Images cho các hàm Lambda.
* **AWS Lambda (API Handler):** 
  * Xử lý dữ liệu người dùng/bài tập trên DynamoDB (*Step 5a*).
  * Đọc testcase mẫu và avatar người dùng từ S3 (*Step 5b, 5c*).
  * Đẩy job chấm bài (**Push Execution Job**) vào hàng chờ SQS (*Step 6*).
* **AWS Lambda (Code Executor Sandbox):**
  * Nhận sự kiện kích hoạt (**Event Trigger**) từ SQS (*Step 7*).
  * Tải bộ testcase đầy đủ từ S3 (*Step 8*).
  * Thực thi và chấm điểm mã nguồn trong môi trường Sandbox cách ly.
  * Ghi kết quả bài nộp (**Save Result**) vào DynamoDB (*Step 9*).

### 2.4. Lớp Xử lý hàng chờ (Queue Processing Layer)
* **AWS SQS (Submission Queue):** Hàng chờ đệm lưu trữ các job nộp bài bất đồng bộ, điều tiết thông lượng và phát sự kiện kích hoạt Lambda Code Executor.

### 2.5. Lớp Cơ sở dữ liệu & Lưu trữ (Database & Storage Layer)
* **AWS DynamoDB (Submission & Problem):** Cơ sở dữ liệu NoSQL lưu trữ thông tin người dùng, bài tập và kết quả chấm bài.
* **AWS S3 Bucket (Testcases & User Avatar):** Hai S3 Bucket độc lập lưu trữ bộ dữ liệu Testcases bài tập và ảnh đại diện người dùng.

### 2.6. Lớp Bảo mật & Giám sát (Security & Monitoring Layer)
* **IAM Roles (Execution Role):** Phân quyền tối thiểu (Least Privilege Access) cho các dịch vụ thực thi.
* **AWS CloudWatch (Logs & Metrics) & AWS SNS:** Thu thập nhật ký log thực thi, theo dõi chỉ số hiệu năng và gửi thông báo cảnh báo sự cố.

---

<div align="center">

<img src="/images/architect-codexecute.png" alt="Sơ đồ kiến trúc CodExecute" style="width: 90%; max-width: 1200px; border-radius: 6px;">

<p style="font-size: 1.15rem; font-weight: 600; margin-top: 8px;">
<i>Hình 1: Sơ đồ chi tiết kiến trúc hệ thống CodExecute trên AWS Serverless</i>
</p>

<img src="/images/5-Workshop/5.1-Workshop-overview/project_overview.png" alt="Trang giao diện Web của CodExecute" style="width: 80%; max-width: 1100px; border-radius: 6px;">
<p style="font-size: 1.15rem; font-weight: 600; margin-top: 8px;">
<i>Hình 2: Trang giao diện Web của CodExecute</i></br>
<i>Link web: </i><a href="https://d1hsp5bm4hkjmb.cloudfront.net">https://d1hsp5bm4hkjmb.cloudfront.net</a>
</p>

</div>
