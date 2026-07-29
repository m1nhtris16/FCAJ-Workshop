---
title: "Workshop"
date: 2026-06-15
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


# HƯỚNG DẪN TRIỂN KHAI DỰ ÁN CODEXECUTE TRÊN AWS

#### Tổng quan

Phần **Workshop** đóng vai trò là cuốn cẩm nang kỹ thuật hướng dẫn chi tiết quy trình **triển khai dự án CodExecute (Hệ thống chấm bài thuật toán tự động)** lên hạ tầng điện toán đám mây **Pure Serverless AWS** từ con số 0.

Toàn bộ quy trình triển khai được chuẩn hóa thành các bước thực hành tự động hóa qua kịch bản Infrastructure as Code (AWS SAM / Terraform Templates), tuân thủ nghiêm ngặt 6 trụ cột thiết kế của **AWS Well-Architected Framework**:
* **Operational Excellence:** Tự động hóa 100% việc khởi tạo và cập nhật hạ tầng qua kịch bản SAM Templates.
* **Security:** Mô hình Zero-Trust với IAM Roles phân quyền tối thiểu, VPC Endpoints riêng tư và môi trường Lambda Sandbox rào chắn chống RCE.
* **Reliability:** Hạ tầng Multi-AZ chịu lỗi, cơ chế đệm SQS Queue và Dead-Letter Queue (DLQ) đảm bảo không mất mát dữ liệu bài nộp.
* **Performance Efficiency:** Tốc độ phản hồi API $< 200ms$, thời gian chấm bài $< 2.0$ giây nhờ phân phối CDN CloudFront Edge và NoSQL DynamoDB.
* **Cost Optimization:** Kiến trúc Pure Serverless Event-Driven giúp tối ưu chi phí vận hành chỉ còn **~$15.43 USD/tháng**.
* **Sustainability:** Tối ưu hóa chu kỳ tính toán của Lambda giúp giảm thiểu tiêu thụ tài nguyên điện năng phần cứng.

---

#### Nội dung các bước triển khai (Deployment Roadmap)

1. [Giới thiệu & Tổng quan Triển khai Dự án](5.1-Workshop-overview/)
2. [Bước 1: Chuẩn bị Môi trường & Khởi tạo Mạng Custom VPC](5.2-Prerequiste/)
3. [Bước 2: Triển khai Lưu trữ S3, DynamoDB NoSQL & VPC Endpoints](5.3-S3-vpc/)
4. [Bước 3: Triển khai Serverless API & Hàng chờ Bất đồng bộ SQS Buffer](5.4-S3-onprem/)
5. [Bước 4: Triển khai Lambda Worker Sandbox & Bảo mật IAM Policies](5.5-Policy/)
6. [Bước 5: Tối ưu Chi phí, Kiểm thử Load Test & Dọn dẹp Tài nguyên](5.6-Cleanup/)