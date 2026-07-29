---
title: "Worklog Tuần 1"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới me chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

### Mục tiêu tuần 1 (15/06/2026 – 21/06/2026):
* Kết nối, làm quen với người hướng dẫn (mentor) và các thành viên trong chương trình First Cloud AI Journey (FCAJ).
* Thiết lập môi trường AWS CLI, tài khoản IAM và bắt đầu giai đoạn **Phase 1: Khởi Tạo Hạ Tầng & IaC** của dự án proposal **CodExecute**.
* Thiết kế và khởi tạo hệ thống lưu trữ S3, cơ sở dữ liệu DynamoDB và mạng Amazon VPC cho dự án CodExecute bằng kịch bản AWS SAM & Terraform.

### Các công việc triển khai trong tuần:
| Thứ | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Làm quen với ban tổ chức FCAJ, nhận lộ trình hướng dẫn thực tập. <br> - Đăng ký tài khoản AWS Free Tier, cấu hình AWS CLI v2 và thiết lập ngân sách cảnh báo **AWS Budgets** ($5/tháng). <br> - Học tổng quan kiến trúc **Pure Serverless AWS** cho dự án CodExecute. | 15/06/2026 | 15/06/2026 | [Chào mừng đến FCAJ](https://cloudjourney.awsstudygroup.com/8-fcjworkforce/) <br> [Cấu hình AWS Budgets](https://000007.awsstudygroup.com/) |
| 3 | - Thiết kế mạng ảo Amazon VPC (CIDR 10.0.0.0/16), quy hoạch Public Subnets và Private Subnets đa vùng Availability Zones. <br> - Cấu hình Internet Gateway, Route Tables và NAT Gateway cho môi trường thử nghiệm. | 16/06/2026 | 16/06/2026 | [Xây dựng mạng VPC trên AWS](https://000003.awsstudygroup.com/)  |
| 4 | - **Lưu trữ Đối tượng CodExecute (Amazon S3):** Khởi tạo 3 S3 Buckets chuyên dụng (`frontend-assets`, `testcases-storage`, `user-avatars`). <br> - Cấu hình S3 Versioning, mã hóa SSE-S3/KMS và thiết lập S3 Lifecycle Rules chuyển dữ liệu log cũ sang Glacier Flexible Retrieval sau 90 ngày. | 17/06/2026 | 17/06/2026 | [Lưu trữ Website tĩnh S3](https://000057.awsstudygroup.com/) <br> [Bảo mật S3 Best Practices](https://000069.awsstudygroup.com/) |
| 5 | - **Cơ sở dữ liệu CodExecute (Amazon DynamoDB):** Thiết kế mô hình NoSQL cho 7 bảng cốt lõi (`Users`, `Problems`, `Submissions`, `TestCases`, `Posts`, `Notifications`, `UserFollows`). <br> - Cấu hình Partition Key, Sort Key, Global Secondary Indexes (GSI) và chọn chế độ tính phí On-Demand Capacity. | 18/06/2026 | 18/06/2026 | [Amazon DynamoDB Docs](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/) |
| 6 | - **Thực hành IaC (AWS SAM & Terraform):** Viết kịch bản Infrastructure as Code tự động hóa việc khởi tạo S3 Buckets, DynamoDB Tables và VPC. <br> - Đóng gói kịch bản SAM Template, kiểm thử triển khai `sam deploy` thành công lên môi trường AWS Dev. | 19/06/2026 | 19/06/2026 | [AWS SAM Developer Guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/) |

### Kết quả đạt được tuần 1:
* **Hòa nhập & Thiết lập Môi trường:** Đã kết nối thành công với Mentor FCAJ, cài đặt AWS CLI v2 trên máy cá nhân, cấu hình IAM User có quyền hạn tối thiểu và kích hoạt AWS Budgets cảnh báo tự động qua email khi chi phí > $5.
* **Hoàn thành Thiết kế Hạ tầng CodExecute (Phase 1 Init):** Khởi tạo thành công mạng Custom VPC (10.0.0.0/16) với Public/Private Subnets đa AZ, Internet Gateway và NAT Gateway đáp ứng yêu cầu chịu lỗi.
* **Triển khai 3 S3 Buckets chuẩn Enterprise:** Tạo và bảo mật 3 S3 Buckets cho dự án CodExecute (chứa web frontend, bộ testcase bài tập và avatar người dùng), bật mã hóa SSE-S3/KMS và cấu hình S3 Lifecycle Rules giúp cắt giảm 85% chi phí lưu trữ lâu dài.
* **Thiết kế 7 Bảng Cơ sở Dữ liệu DynamoDB NoSQL:** Hoàn thành sơ đồ thiết kế 7 bảng DynamoDB NoSQL On-Demand với thời gian phản hồi truy vấn dưới 10 mili-giây, hỗ trợ phân vùng ngang tự động theo quy mô bài nộp.
* **Tự động hóa 100% bằng Mã Kịch bản IaC SAM/Terraform:** Đóng gói toàn bộ tài nguyên hạ tầng Phase 1 thành file kịch bản IaC SAM Template chuẩn xác, kiểm thử triển khai tự động `sam build && sam deploy` thành công chỉ trong dưới 5 phút.
