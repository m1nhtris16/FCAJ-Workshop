---
title: "Worklog Tuần 5"
date: 2026-07-13
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

### Mục tiêu tuần 5 (13/07/2026 – 19/07/2026):
* Tìm hiểu giải pháp kết nối riêng tư AWS PrivateLink và các loại VPC Endpoints (Gateway Endpoints & Interface Endpoints).
* Thực hành bài Workshop "Đảm bảo truy cập Hybrid an toàn đến S3 bằng cách sử dụng VPC endpoint" và cấu hình VPC Endpoint Policies.
* Đánh giá bảo mật hạ tầng CodExecute, kiểm thử cơ chế SQS Dead-Letter Queue (DLQ) failover để đảm bảo tính an toàn dữ liệu bài nộp.

### Các công việc triển khai trong tuần:
| Thứ | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - **Tổng quan AWS PrivateLink & VPC Endpoints:** Nghiên cứu lý do sử dụng điểm cuối riêng tư để loại bỏ hoàn toàn việc truyền dữ liệu qua Public Internet hay NAT Gateway. | 13/07/2026 | 13/07/2026 | [AWS VPC Endpoints Guide](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html) |
| 3 | - **So sánh Gateway vs Interface Endpoints:** So sánh Gateway Endpoints (miễn phí, dành cho S3/DynamoDB qua Route Tables) vs Interface Endpoints (dùng ENI có IP riêng qua Private DNS). | 14/07/2026 | 14/07/2026 | [AWS PrivateLink Docs](https://docs.aws.amazon.com/vpc/latest/privatelink/concepts.html) |
| 4 | - **Kiểm soát VPC Endpoint Policies cho CodExecute:** Tạo IAM Resource Policy đính kèm vào VPC Endpoint, chỉ cho phép các máy chủ Lambda Worker truy cập đúng 3 S3 Buckets của dự án CodExecute. | 15/07/2026 | 15/07/2026 | [VPC Endpoint Policy Docs](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html) |
| 5 | - **Kịch bản Hybrid Access & SQS Failover Testing:** Học giải pháp kết nối On-Premises qua Interface Endpoint và Route 53 Inbound Resolver. <br> - Thử nghiệm mô hình DLQ Failover: Giả lập lỗi Worker crash để xác minh tin nhắn được đẩy an toàn vào SQS DLQ. | 16/07/2026 | 16/07/2026 | [AWS Hybrid Connectivity Guide](https://docs.aws.amazon.com/vpc/latest/privatelink/hybrid-commitments.html) |
| 6 | - **Thực hành & Verification:** Triển khai Gateway VPC Endpoint và Interface VPC Endpoint cho S3, kiểm thử `aws s3 ls` xác nhận truy cập thành công tới bucket cho phép và bị chặn (`Access Denied`) khi gọi bucket khác. | 17/07/2026 | 17/07/2026 | [AWS S3 VPC Endpoints Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/privatelink-interface-endpoints.html) |

### Kết quả đạt được tuần 5:
* **Hoàn Thành Bài Workshop VPC Endpoint:** Thực hiện thành công bài thực hành "Đảm bảo truy cập Hybrid an toàn đến S3 bằng VPC endpoint", khởi tạo thành công Gateway & Interface Endpoints.
* **Tối Ưu Đường Truyền Riêng Tư (Private Infrastructure):** Điều hướng toàn bộ lưu lượng dữ liệu S3 và DynamoDB đi qua đường mạng riêng tư của AWS, loại bỏ rủi ro bảo mật từ Public Internet và cắt giảm chi phí băng thông NAT Gateway.
* **Kiểm Soát Rò Rỉ Dữ Liệu Với Endpoint Policies:** Cấu hình chính xác VPC Endpoint Policy chỉ cho phép thao tác trên đúng 3 S3 Buckets của dự án CodExecute; thử nghiệm thành công cơ chế chặn truy cập đối với tất cả các S3 Buckets bên ngoài.
* **Kiểm Thử Thành Công SQS DLQ Failover:** Giả lập sự cố ngắt đứt Worker trong khi đang xử lý tin nhắn từ SQS; xác minh tin nhắn được tự động retry 3 lần và chuyển vào Dead-Letter Queue an toàn mà không mất mát bất kỳ bài nộp nào.
* **Nâng Cao Độ Tin Cậy & An Toàn Hạ Tầng:** Hệ thống CodExecute đạt chuẩn bảo mật hạ tầng khép kín (Network Isolation) và duy trì độ tin cậy dữ liệu 100%.
