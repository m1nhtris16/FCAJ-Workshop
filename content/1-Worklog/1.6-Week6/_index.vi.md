---
title: "Worklog Tuần 6"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

### Mục tiêu tuần 6 (20/07/2026 – 26/07/2026):
* Triển khai **Phase 4: Kiểm Thử Chịu Tải & Bảo Mật Hardening** của dự án **CodExecute**.
* Thực hành bài Workshop 3.2 "SLA & Monitoring", nghiên cứu mô hình 5 tầng Kim tự tháp Giám sát (Monitoring Pyramid).
* Thực hiện kiểm thử chịu tải (Load Testing với Locust/k6 lên đến 1,000 VUs), cấu hình CloudWatch Alarms gửi thông báo tự động tới Slack/Email và tối ưu hóa chi phí Lambda Compute.

### Các công việc triển khai trong tuần:
| Thứ | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - **Nghiên cứu Workshop 3.2 SLA & Monitoring:** Học khái niệm SLA, phân tích khoảng cách giữa *Healthy Infrastructure* (CPU/RAM xanh) và *Healthy User Experience* (người dùng đăng nhập/nộp bài thành công). | 20/07/2026 | 20/07/2026 | [AWS Observability & CloudWatch Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) |
| 3 | - **Thiết lập Luồng Cảnh Báo CloudWatch (Alerting Flow):** Tạo Custom Metrics (`SubmissionLatency`, `SubmissionErrorRate`, `LoginFailure`), cấu hình CloudWatch Alarms gửi thông báo qua Amazon SNS đến kênh Slack khi tỷ lệ lỗi > 1%. | 21/07/2026 | 21/07/2026 | [CloudWatch Alarms & SNS Alerting Docs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) |
| 4 | - **Kiểm Thử Chịu Tải CodExecute (Load Testing 1,000 VUs):** Viết kịch bản kiểm thử chịu tải bằng Locust/k6 giả lập 1,000 người dùng ảo (Virtual Users) đồng thời gửi yêu cầu nộp bài trong các đợt thi cao điểm. | 22/07/2026 | 22/07/2026 | [Locust Load Testing Docs](https://locust.io/) |
| 5 | - **Tối Ưu Chi Phí AWS Lambda Power Tuning:** Sử dụng công cụ AWS Lambda Power Tuning để tìm mức cấu hình RAM/vCPU tối ưu nhất cho Lambda API & Worker, vừa tăng tốc xử lý vừa giảm chi phí compute. | 23/07/2026 | 23/07/2026 | [AWS Lambda Power Tuning](https://github.com/alexcasalboni/aws-lambda-power-tuning) |
| 6 | - **Thực hành & Hoàn thiện Phase 4:** Đánh giá chỉ số Load Test đạt SLA cam kết (API Response Latency < 200ms, Submission Evaluation < 2.0s), rà soát bảo mật và khắc phục 100% lỗ hổng High/Critical. | 24/07/2026 | 24/07/2026 | [AWS Performance Efficiency Pillar](https://docs.aws.amazon.com/wellarchitected/latest/performance-efficiency-pillar/welcome.html) |

### Kết quả đạt được tuần 6:
* **Hoàn Thành Bài Workshop 3.2 SLA & Monitoring:** Nắm vững mô hình Tháp Giám Sát 5 tầng, chuyển đổi tư duy từ giám sát hạ tầng sang giám sát trải nghiệm người dùng thực tế.
* **Tự Động Hóa Luồng Cảnh Báo Tức Thì (CloudWatch → SNS → Slack):** Khởi tạo thành công Custom Metrics và CloudWatch Alarms tự động gửi thông báo đến Slack/Email khi phát hiện số lượng đăng nhập thất bại hoặc lỗi bài nộp tăng bất thường.
* **Kiểm Thử Chịu Tải Thành Công 1,000 VUs:** Thực hiện Load Test giả lập 1,000 người dùng nộp bài đồng thời; hệ thống Serverless Lambda + SQS tự động mở rộng mượt mà không xảy ra hiện tượng rớt request hay sập hệ thống.
* **Tối Ưu Hiệu Năng & Chi Phí Tính Toán:** Áp dụng công cụ AWS Lambda Power Tuning tìm được điểm RAM 512MB tối ưu cho Worker, rút ngắn thời gian xử lý bài nộp xuống 800ms và tiết kiệm thêm 25% chi phí compute.
* **Hoàn Thành 100% Phase 4 Dự Án CodExecute:** Hệ thống CodExecute đạt đầy đủ các chỉ số SLA cam kết và sẵn sàng cho giai đoạn GO-LIVE chính thức.
