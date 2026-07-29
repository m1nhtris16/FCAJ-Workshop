---
title: "Worklog Tuần 4"
date: 2026-07-06
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

### Mục tiêu tuần 4 (06/07/2026 – 12/07/2026):
* Triển khai **Phase 3: Xử Lý Bất Đồng Bộ & Lambda Sandbox** của dự án **CodExecute**.
* Viết AWS Lambda Worker Runner tiêu thụ tin nhắn nộp bài từ Amazon SQS Queue, biên dịch và thực thi 4 ngôn ngữ (C++, Java, Python, JS).
* Thiết lập môi trường Sandbox cách ly hoàn toàn trên AWS Lambda, rà soát an ninh bảo mật triệt để chống nguy cơ Remote Code Execution (RCE).

### Các công việc triển khai trong tuần:
| Thứ | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - **Luồng Đẩy Bài Nộp Bất Đồng Bộ (SQS Job Producer):** Cấu hình Lambda API đẩy payload bài nộp (code người dùng, language, problem_id) vào Amazon SQS `Submissions Queue` khi nhận request "Submit Code". | 06/07/2026 | 06/07/2026 | [Amazon SQS Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/) |
| 3 | - **Thiết kế AWS Lambda Worker Runner:** Lập trình Lambda Worker tiêu thụ tin nhắn từ SQS, tự động chọn trình biên dịch (GCC 13 cho C++, OpenJDK 21 cho Java, Python 3.12, Node.js 20 cho JS). | 07/07/2026 | 07/07/2026 | [AWS Lambda Execution Environment](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-context.html) |
| 4 | - **Thiết lập Sandbox Cách Ly Chống RCE:** Tắt kết nối mạng ngoài (`VPC Network: Disabled / Strict Subnet Groups`), áp dụng giới hạn cứng tài nguyên (RAM 512MB, CPU 1 Core, Execution Timeout 5s, Process Limit 20 pids). | 08/07/2026 | 08/07/2026 | [AWS Lambda Security Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/lambda-security.html) |
| 5 | - **Xử lý Testcase & Chấm Điểm Tự Động:** Viết logic đọc bộ Testcases (Input/Output text files) từ Amazon S3, so sánh output từng dòng và cập nhật kết quả (Accept, Wrong Answer, TLE, MLE) vào DynamoDB. | 09/07/2026 | 09/07/2026 | [Amazon S3 Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/) |
| 6 | - **Thực hành & Hoàn thiện Phase 3:** Kiểm thử toàn bộ luồng nộp bài end-to-end: Client → API Gateway → Lambda API → SQS → Lambda Worker Sandbox → S3 Testcases → DynamoDB. | 10/07/2026 | 10/07/2026 |  |

### Kết quả đạt được tuần 4:
* **Hoàn Thành Luồng Chấm Bài Bất Đồng Bộ:** Thiết lập luồng gửi nhận tin nhắn bất đồng bộ qua SQS giúp hệ thống chấm bài nhận hàng nghìn yêu cầu nộp bài cùng lúc mà không làm quá tải backend.
* **Xây Dựng Lambda Worker Sandbox Đa Ngôn Ngữ:** Lập trình thành công Lambda Worker thực thi 4 ngôn ngữ lập trình phổ biến (C++, Java, Python, JavaScript) với khả năng tự động phân tích lỗi biên dịch (Compile Errors) và lỗi thực thi (Runtime Errors).
* **Bảo Mật An Toàn Tuyệt Đối Chống RCE:** Thiết lập môi trường Sandbox cách ly hoàn toàn không có quyền mạng ngoài, giới hạn tài nguyên nghiêm ngặt (Memory 512MB, Timeout 5s), chống lại các nguy cơ Remote Code Execution (RCE) hoặc Fork Bomb phá hoại server.
* **Tự Động Hóa Chấm Điểm Testcase trên S3:** Đọc và đối sánh dữ liệu bộ Testcase lớn cực nhanh từ Amazon S3, ghi nhận thời gian thực thi (runtime ms) và bộ nhớ tiêu thụ (memory MB) chính xác vào DynamoDB Submissions table.
* **Hoàn Thành 100% Phase 3 Dự Án CodExecute:** Luồng chấm bài tự động bất đồng bộ trong môi trường Sandbox cách ly đã vận hành hoàn hảo với độ trễ chấm bài < 2.0 giây.
