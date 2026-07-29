---
title: "Giới thiệu & Tổng quan Triển khai Dự án"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---



### 1. Giới thiệu Tổng quan Dự án CodExecute
**CodExecute** là hệ thống chấm bài trực tuyến và nền tảng đánh giá thuật toán tự động được thiết kế và triển khai hoàn toàn trên hạ tầng đám mây **Pure Serverless AWS** (Cloud-Native & Event-Driven). Hệ thống được xây dựng nhằm phục vụ hàng chục nghìn lượt nộp bài lập trình đồng thời của sinh viên và lập trình viên trong các kỳ thi thuật toán với độ ổn định cao, thời gian chấm bài siêu nhanh ($< 2.0$ giây) và chi phí vận hành siêu tiết kiệm.

Phần **Workshop này là tài liệu hướng dẫn từng bước (Step-by-step AWS Deployment Guide)** ghi lại chi tiết toàn bộ quy trình thiết lập môi trường, thiết kế hạ tầng mạng, xây dựng cơ sở dữ liệu, đóng gói ứng dụng Serverless và triển khai chính thức dự án CodExecute lên đám mây AWS từ con số 0.

---

### 2. Kiến trúc Hạ tầng Triển khai (System Architecture Overview)
Hệ thống CodExecute được triển khai theo mô hình kiến trúc Serverless đa tầng khép kín tuân thủ 6 trụ cột của **AWS Well-Architected Framework**:

![Sơ đồ kiến trúc triển khai CodExecute](/images/2-Proposal/architect-codexecute.drawio.png)

#### Các thành phần hạ tầng AWS cốt lõi của dự án:
* **Tầng Frontend & Phân phối Edge (Amazon CloudFront + S3):** Giao diện Single Page Application (React + Vite + TailwindCSS + Monaco Code Editor) được đóng gói tĩnh và lưu trữ trên **Amazon S3** (`frontend-assets`), kết hợp với **Amazon CloudFront CDN** phân phối dữ liệu ở vùng biên giúp tối ưu tốc độ tải trang toàn cầu và tự động chống tấn công DDoS với AWS Shield.
* **Tầng Cổng vào API Gateway & Backend Serverless (API Gateway + AWS Lambda API):** **Amazon API Gateway** (HTTP API) đóng vai trò cổng duy nhất nhận request, tích hợp xác thực JWT Token và chuyển hướng API requests đến **AWS Lambda API Handler** (viết bằng FastAPI + Mangum adapter) để xử lý logic quản lý bài tập, tài khoản và tương tác mạng xã hội.
* **Tầng Hàng chờ Bất đồng bộ (Amazon SQS Buffer Queue):** Khi người dùng nhấn "Submit Code", bài nộp được đẩy ngay vào **Amazon SQS** (`Submissions Queue`). SQS đóng vai trò làm bộ đệm điều tiết lượng lưu lượng nộp bài đột biến, đi kèm cơ chế **Dead-Letter Queue (DLQ)** tự động lưu trữ các tin nhắn bị lỗi quá 3 lần để đảm bảo không mất mát dữ liệu bài nộp.
* **Tầng Chấm bài Sandbox Cách ly (AWS Lambda Worker Sandbox):** **AWS Lambda Worker** tiêu thụ tin nhắn từ SQS, tự động gọi trình biên dịch phù hợp để thực thi 4 ngôn ngữ (**C++, Java, Python, JavaScript**). Môi trường chạy được rào chắn an ninh nghiêm ngặt không có kết nối mạng ngoài (`VPC Network: Disabled`), giới hạn cứng tài nguyên (RAM 512MB, Execution Timeout 5s), triệt tiêu 100% nguy cơ bị tấn công thực thi mã độc từ xa (Remote Code Execution - RCE).
* **Tầng Cơ sở Dữ liệu & Lưu trữ Bộ Testcase (DynamoDB + S3):** **Amazon DynamoDB** (7 bảng NoSQL On-Demand) lưu trữ dữ liệu người dùng, danh sách bài tập và lịch sử nộp bài với độ trễ phản hồi truy vấn dưới 10 mili-giây. Bộ Testcase lớn được lưu trữ riêng biệt trên **Amazon S3** (`testcases-storage`).
* **Tầng An ninh & Mạng Riêng tư (Custom VPC + VPC Endpoints + IAM):** Toàn bộ lưu lượng kết nối dữ liệu giữa các dịch vụ AWS đi qua **Custom VPC** riêng tư và các điểm cuối **VPC Endpoints** (Gateway & Interface Endpoints), loại bỏ hoàn toàn việc truyền dữ liệu qua Public Internet. Quyền truy cập giữa các thành phần được kiểm soát nghiêm ngặt bằng **AWS IAM Roles** theo nguyên tắc phân quyền tối thiểu (Least Privilege Access).

---

### 3. Lộ trình Các Bước Triển Khai Dự Án Trên AWS
Toàn bộ quy trình triển khai dự án CodExecute trên AWS trong phần Workshop được chia làm 5 giai đoạn thực hành chi tiết:

| Bước triển khai | Mục nội dung Workshop | Công việc hạ tầng thực hiện |
| --- | --- | --- |
| **Bước 1** | **5.2 - Preparation & Network VPC** | Cấu hình tài khoản AWS, tạo IAM User, cài đặt AWS CLI v2, AWS SAM CLI và thiết kế mạng ảo Amazon Custom VPC (Public/Private Subnets đa AZ, IGW, Route Tables). |
| **Bước 2** | **5.3 - Storage & Database Layer** | Khởi tạo 3 S3 Buckets chuyên dụng, cấu hình S3 Lifecycle Rules, mã hóa SSE-S3/KMS, thiết kế 7 bảng DynamoDB NoSQL và cấu hình VPC Gateway/Interface Endpoints cho S3 & DynamoDB. |
| **Bước 3** | **5.4 - Serverless API & SQS Buffer** | Viết ứng dụng FastAPI Backend, đóng gói Mangum Serverless Adapter trên AWS Lambda API Handler, cấu hình Amazon API Gateway HTTP API routes và khởi tạo Amazon SQS `Submissions Queue` kèm Dead-Letter Queue (DLQ). |
| **Bước 4** | **5.5 - Execution Worker Sandbox & Security** | Lập trình AWS Lambda Worker Runner chấm 4 ngôn ngữ lập trình, thiết lập môi trường Sandbox rào chắn chống RCE, gán IAM Security Roles chuẩn phân quyền tối thiểu và cấu hình CloudFront CDN cho React Frontend. |
| **Bước 5** | **5.6 - Optimization, Testing & Cleanup** | Chạy kịch bản kiểm thử chịu tải (Load Test 1,000 VUs bằng Locust/k6), tối ưu hóa RAM/Timeout với AWS Lambda Power Tuning, cấu hình CloudWatch Alarms gửi cảnh báo Slack và xây dựng kịch bản dọn dẹp tài nguyên. |

---

### 4. Kết quả Kỳ vọng của Quy trình Triển khai
* **Hạ tầng 100% Serverless Cloud-Native:** Tự động mở rộng từ 0 lên hàng nghìn yêu cầu nộp bài đồng thời mà không cần quản lý máy chủ EC2.
* **Thời gian Phản hồi Rất Thấp:** API Latency $< 200ms$, thời gian chấm bài tự động $< 2.0$ giây.
* **Bảo mật An toàn Tuyệt đối:** Không lưu secret key tĩnh, cách ly 100% mã người dùng trong Lambda Sandbox, chặn hoàn toàn lỗ hổng RCE.
* **Tối ưu Chi phí Xuất sắc:** Tổng chi phí vận hành hạ tầng dự toán chỉ khoảng **~$15.43 USD/tháng** (miễn phí $< $3 USD/tháng trong năm đầu nhờ AWS Free Tier).
