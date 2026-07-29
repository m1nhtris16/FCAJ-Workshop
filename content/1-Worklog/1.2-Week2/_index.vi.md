---
title: "Worklog Tuần 2"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

### Mục tiêu tuần 2 (22/06/2026 – 28/06/2026):
* Nắm vững các thành phần bảo mật AWS IAM Roles và hoàn thiện nguyên tắc phân quyền tối thiểu (Least Privilege Access) cho hệ thống **CodExecute**.
* Nghiên cứu và triển khai hàng chờ bất đồng bộ **Amazon SQS** (`Submissions Queue`) làm Buffer chống nghẽn hệ thống khi có đợt nộp bài đột biến.
* Tìm hiểu và cấu hình **Amazon API Gateway** (HTTP API) và **Amazon CloudFront CDN Distribution** cho ứng dụng React Frontend.

### Các công việc triển khai trong tuần:
| Thứ | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - **Bảo mật IAM Roles CodExecute:** Phân tích các quyền truy cập tối thiểu giữa các dịch vụ. <br> - Viết IAM Policies dạng JSON phân quyền cho Lambda API, Lambda Worker, S3 và DynamoDB mà không dùng cặp chìa khóa Access Keys tĩnh. | 22/06/2026 | 22/06/2026 | [Quản lý truy cập IAM](https://000002.awsstudygroup.com/) <br> [IAM Policies Developer Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/) |
| 3 | - **Hàng chờ Bất đồng bộ Amazon SQS:** Học nguyên lý Buffer chống nghẽn cho bài toán nộp bài tập trung trong các kỳ thi. <br> - Khởi tạo `Submissions Queue` trên Amazon SQS, cấu hình Long Polling (`ReceiveMessageWaitTimeSeconds = 20`) giúp tiết kiệm 90% chi phí API. | 23/06/2026 | 23/06/2026 | [Amazon SQS Developer Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) |
| 4 | - **Cơ chế Failover Dead-Letter Queue (DLQ):** Cấu hình Amazon SQS Dead-Letter Queue để lưu giữ các bài nộp chấm lỗi quá 3 lần, đảm bảo không mất mát bài nộp của người dùng khi worker gặp sự cố. | 24/06/2026 | 24/06/2026 | [SQS Dead-Letter Queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) |
| 5 | - **Cổng vào Amazon API Gateway & CDN CloudFront:** Cấu hình API Gateway HTTP API (tiết kiệm 70% chi phí so với REST API), tạo routes xác thực JWT. <br> - Cấu hình CloudFront CDN phân phối ứng dụng React từ S3, điều hướng `/api/*` tới API Gateway. | 25/06/2026 | 25/06/2026 | [Amazon API Gateway Docs](https://docs.aws.amazon.com/apigateway/) <br> [Amazon CloudFront Docs](https://docs.aws.amazon.com/cloudfront/) |
| 6 | - **Thực hành & Hoàn thiện Phase 1:** Triển khai thành công cụm SQS + DLQ + IAM Roles + CloudFront CDN lên môi trường AWS bằng kịch bản SAM Template. | 26/06/2026 | 26/06/2026 |  |

### Kết quả đạt được tuần 2:
* **Bảo mật Chuẩn Zero-Trust với IAM Roles:** Đã gán thành công các IAM Roles chuẩn phân quyền tối thiểu cho Lambda API và Lambda Worker; loại bỏ hoàn toàn nguy cơ rò rỉ secret key trên nguồn code.
* **Xây dựng Hàng chờ SQS Buffer Chống Nghẽn:** Khởi tạo thành công Amazon SQS `Submissions Queue` có khả năng điều tiết lưu lượng nộp bài hàng nghìn bài nộp/phút, áp dụng Long Polling giúp giảm 90% chi phí yêu cầu SQS API rỗng.
* **Đảm bảo Không Mất Mát Bài Nộp (Zero Data Loss):** Thiết lập cơ chế Dead-Letter Queue (DLQ) tự động bắt các message chấm bài bị lỗi 3 lần liên tiếp để kỹ sư xử lý lại, đảm bảo 100% độ tin cậy dữ liệu.
* **Cấu hình Cổng vào API Gateway & CloudFront CDN Global:** Triển khai thành công API Gateway HTTP API làm cổng duy nhất xác thực JWT Token, tích hợp với CloudFront CDN phân phối nội dung tĩnh ở vùng biên với độ trễ siêu thấp và tự động bật bảo vệ DDoS với AWS Shield.
* **Hoàn Thành 100% Phase 1 Dự Án CodExecute:** Toàn bộ hạ tầng cốt lõi (VPC, S3, DynamoDB, SQS, IAM, CloudFront, API Gateway) đã sẵn sàng hoạt động trên AWS Dev environment theo chuẩn AWS Well-Architected.
