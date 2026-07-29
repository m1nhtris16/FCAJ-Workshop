---
title: "Worklog Tuần 3"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

### Mục tiêu tuần 3 (29/06/2026 – 05/07/2026):
* Tiến hành **Phase 2: Xây Dựng Frontend & Backend Core** của dự án **CodExecute**.
* Phát triển ứng dụng Single Page App React + Vite + TailwindCSS tích hợp Trình soạn thảo Code tương tác (Code Editor Monaco).
* Phát triển hệ thống RESTful APIs với FastAPI framework, đóng gói chạy trên AWS Lambda và kết nối DynamoDB.

### Các công việc triển khai trong tuần:
| Thứ | Công việc chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - **Phát triển React Frontend UI:** Khởi tạo dự án React + Vite + TailwindCSS. <br> - Xây dựng giao diện danh sách bài tập (Problemset), bộ lọc độ khó (Easy, Medium, Hard) và phân loại theo chủ đề thuật toán (Binary Tree, DP, Graph...). | 29/06/2026 | 29/06/2026 | [Vite Documentation](https://vitejs.dev/) |
| 3 | - **Tích hợp Trình Soạn Thảo Code (Monaco Editor):** Tích hợp Monaco Code Editor hỗ trợ syntax highlighting cho 4 ngôn ngữ (**C++, Java, Python, JavaScript**). <br> - Xây dựng 2 chế độ: **Run Code** (chạy với custom input) và **Submit Code** (chấm bài toàn bộ testcase). | 30/06/2026 | 30/06/2026 | [Monaco Editor React](https://github.com/react-monaco-editor/react-monaco-editor) |
| 4 | - **Phát triển FastAPI RESTful Backend:** Viết ứng dụng FastAPI xử lý các API endpoints: Authentication (JWT login/register), Problems API, Submissions API, User Profile & Social Network (Posts, Comments, Follow). | 01/07/2026 | 01/07/2026 | [FastAPI Documentation](https://fastapi.tiangolo.com/) |
| 5 | - **Đóng gói AWS Lambda API Handler:** Sử dụng Mangum Serverless Adapter để đóng gói ứng dụng FastAPI chạy mượt mà trên AWS Lambda Function (RAM 512MB, Timeout 10s). <br> - Kết nối Lambda API với Amazon DynamoDB để đọc/ghi dữ liệu bài tập và thông tin người dùng. | 02/07/2026 | 02/07/2026 | [AWS Lambda Python Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html) |
| 6 | - **Thực hành & Tích hợp Frontend - Backend (Phase 2 Completion):** Tích hợp thành công ứng dụng React Frontend với API Gateway & Lambda API Handler; triển khai web UI hoàn chỉnh lên CloudFront CDN + S3 Bucket. | 03/07/2026 | 03/07/2026 | [Amazon CloudFront S3 Integration](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-https-alternate-domain-names.html) |

### Kết quả đạt được tuần 3:
* **Giao diện Trình Soạn Thảo Code Chuyên Nghiệp:** Xây dựng thành công web UI React + Vite phản hồi mượt mà, tích hợp Monaco Editor hỗ trợ biên dịch 4 ngôn ngữ (C++, Java, Python, JS) kèm tính năng đổi theme và tự động gợi ý code.
* **Hệ Thống RESTful APIs FastAPI Tốc Độ Cao:** Phát triển thành công bộ RESTful APIs trên FastAPI xử lý toàn bộ logic xác thực JWT, quản lý bài tập, lịch sử nộp bài và tương tác mạng xã hội lập trình viên.
* **Triển khai Serverless Backend với AWS Lambda:** Đóng gói ứng dụng FastAPI chạy thành công trên AWS Lambda API Handler kết hợp với DynamoDB; đạt chỉ số thời gian phản hồi API (API Response Latency) < 200ms.
* **Đồng Bộ Frontend CloudFront & S3:** Đóng gói build dự án React và upload lên Amazon S3, kích hoạt CloudFront CDN giúp người dùng truy cập giao diện ứng dụng với tốc độ tải trang cực nhanh toàn cầu.
* **Hoàn Thành Phase 2 Dự Án CodExecute:** Toàn bộ thành phần Core Frontend và RESTful Backend Serverless đã hoạt động ổn định và sẵn sàng cho luồng chấm bài bất đồng bộ.
