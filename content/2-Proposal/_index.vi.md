---
title: "Bản đề xuất dự án"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

# CODEXECUTE - HỆ THỐNG CHẤM BÀI TRỰC TUYẾN & NỀN TẢNG THUẬT TOÁN TỰ ĐỘNG


## 1. TỔNG QUAN DỰ ÁN

**CodExecute** là một nền tảng chấm bài tự động trực tuyến (Online Judge Platform) và mạng xã hội lập trình viên hiện đại, được thiết kế để giải quyết bài toán kiểm thử code, luyện tập thuật toán và đánh giá năng lực lập trình theo thời gian thực.

Dự án được xây dựng 100% theo mô hình **Pure Serverless Cloud-Native AWS**, sử dụng **AWS Lambda** làm môi trường thực thi backend lẫn sandbox chấm bài cách ly hoàn toàn, nhằm đảm bảo tính sẵn sàng cao (High Availability), khả năng tự động mở rộng từ 0 (Scale-to-Zero), và môi trường thực thi mã nguồn bảo mật (Secure Sandbox Execution).

![CodExecute Platform Architecture](/images/2-Proposal/codexecute_platform_structure.png)

---

## 2. MỤC TIÊU DỰ ÁN

* **Khả năng mở rộng vô hạn (Elastic Scalability):** Hệ thống Serverless tự động thu phóng theo nhu cầu, có khả năng xử lý từ hàng trăm đến hàng chục nghìn lượt nộp bài (submissions) đồng thời trong các kỳ thi mà không gây nghẽn hoặc sập hệ thống.
* **Môi trường chấm bài an toàn tuyệt đối (Isolation & Security):** Thực thi mã nguồn nguy hiểm do người dùng gửi lên trong môi trường sandbox Lambda cách ly được giới hạn tài nguyên (CPU, RAM, Time, Network), chống lại các nguy cơ Remote Code Execution (RCE) hoặc chiếm quyền hạ tầng.
* **Phản hồi thời gian thực với độ trễ tối thiểu (Low-Latency Response):** Tối ưu luồng nhận bài và trả kết quả chấm bài (Accept/Wrong Answer/Time Limit Exceeded/Memory Limit Exceeded) trong dưới 2 giây đối với bài tập thông thường.
* **Tối ưu hóa chi phí vận hành (Cost Optimization):** Áp dụng triệt để mô hình *Pay-As-You-Go*, chỉ chi trả cho lượng tài nguyên compute/storage thực tế tiêu thụ, hoàn toàn không có chi phí duy trì máy chủ nhàn rỗi 24/7.
* **Trải nghiệm người dùng cao cấp (Seamless UX/UI):** Cung cấp giao diện trực quan với Trình soạn thảo Code chuyên nghiệp, hỗ trợ đa ngôn ngữ (Python, C++, Java, JavaScript), hệ thống bài tập phân loại theo chủ đề, cùng tính năng tương tác mạng xã hội (Post, Follow, Achievement Badges).

---

## 3. VẤN ĐỀ DỰ ÁN GIẢI QUYẾT

| Vấn đề của hệ thống truyền thống | Giải pháp của CodExecute trên AWS |
| :--- | :--- |
| **Rủi ro rò rỉ bảo mật (RCE):** Mã nguồn chưa kiểm duyệt của người dùng có thể thực thi lệnh xóa file hệ thống, đào coin, hoặc tấn công nội bộ server. | **Chống RCE với Lambda Sandbox:** Mỗi submission được chạy trong một Lambda Worker cách ly hoàn toàn, không có quyền truy cập mạng ngoài và bị giới hạn tài nguyên nghiêm ngặt (Memory, Execution Timeout, Process Limits). |
| **Nghẽn cổ chai khi cao điểm (Traffic Spikes):** Các đợt nộp bài dồn dập khiến server bị quá tải (Overload/Crash). | **Hàng chờ bất đồng bộ với Amazon SQS:** Đẩy các bài nộp vào queue để điều tiết lưu lượng (Buffer), giúp Lambda Worker tự động scale xử lý ổn định theo cơ chế nạp xả tự động. |
| **Chi phí máy chủ nhàn rỗi cao:** Phải duy trì cụm máy chủ EC2/Container cấu hình cao 24/7 dù giờ thấp điểm không có người dùng. | **Serverless Pay-As-You-Go (Lambda + API Gateway + DynamoDB):** Hạ tầng tự động thu phóng về 0 (Scale to Zero) khi không có request, giảm đến 75% chi phí hạ tầng. |
| **Quản lý bộ dữ liệu Testcase khổng lồ:** Testcase lớn (vài chục MB đến vài GB) làm quá tải cơ sở dữ liệu quan hệ (RDBMS). | **Lưu trữ phân tầng S3 & DynamoDB:** Metadata bài tập lưu ở DynamoDB (NoSQL tốc độ cao), bộ Testcase input/output lớn lưu trên Amazon S3 với tốc độ đọc cực nhanh. |

---

## 4. CÁC TÍNH NĂNG CHÍNH

1. **Hệ Thống Quản Lý & Phân Loại Bài Tập (Problemset & Tagging System):**
   - Tìm kiếm bài tập theo tiêu đề, độ khó (Easy, Medium, Hard), chủ đề thuật toán (Binary Tree, DP, Graph, Dynamic Array...).
   - Thống kê tỷ lệ giải thành công (Acceptance Rate) và số lượng lượt nộp bài.

2. **Trình Soạn Thảo & Chạy Bài Nộp (Interactive Code Editor & Runner):**
   - Hỗ trợ biên dịch và thực thi 4 ngôn ngữ phổ biến: **C++, Java, Python, JavaScript**.
   - Chế độ **Run Code** (Chạy thử với custom input) và **Submit Code** (Chấm điểm toàn bộ Testcase).
   - Tự động bắt lỗi biên dịch (Compile Error), Runtime Error, Time Limit Exceeded (TLE), Memory Limit Exceeded (MLE).

3. **Môi Trường Sandbox Chấm Bài Tự Động (Automated Isolated Sandbox):**
   - Tách biệt hoàn toàn môi trường nhận API và môi trường thực thi code.
   - So sánh output với chuẩn chính xác từng ký tự/dòng.

4. **Mạng Xã Hội Lập Trình Viên (Developer Social Network):**
   - Tạo bài viết chia sẻ kiến thức (Posts), thảo luận lời giải (Comments), theo dõi bạn bè (Follow system).
   - Hệ thống danh hiệu & huy hiệu thành tựu (Achievement Badges).

5. **Trang Cá Nhân & Thống Kê Tiến Độ (User Dashboard & Analytics):**
   - Biểu đồ nhiệt lượt giải bài (Submission Heatmap).
   - Lịch sử nộp bài chi tiết kèm mã nguồn và thời gian chạy/bộ nhớ tiêu dùng.

---

## 5. KIẾN TRÚC HOẠT ĐỘNG CỦA DỰ ÁN

### 5 trụ cột của AWS Well-Architected Framework
Kiến trúc của **CodExecute** tuân thủ 5 trụ cột của **AWS Well-Architected Framework**:

1. **Operational Excellence (Vận hành xuất sắc):** Quản lý cấu hình bằng Infrastructure as Code (IaC SAM/CloudFormation), giám sát tập trung với Amazon CloudWatch Logs & Metrics.
2. **Security (Bảo mật):** Phân quyền tối thiểu (Least Privilege) với IAM Roles, mã hóa dữ liệu At-Rest (KMS) & In-Transit (TLS 1.3/HTTPS via CloudFront), sandbox cách ly hoàn toàn.
3. **Reliability (Độ tin cậy):** Đảm bảo tính khả dụng cao qua đa Availability Zone (Multi-AZ), cơ chế retry và Dead-Letter Queue (DLQ) trên SQS.
4. **Performance Efficiency (Hiệu năng):** Phân phối tĩnh với CloudFront CDN Edge locations, đọc dữ liệu cực nhanh với DynamoDB Single-digit millisecond latency.
5. **Cost Optimization (Tối ưu chi phí):** Áp dụng triệt để kiến trúc Pure Serverless Event-Driven (Lambda Pay-As-You-Go).

### Sơ đồ kiến trúc của dự án
![Sơ đồ kiến trúc CodExecute](/images/2-Proposal/architect-codexecute.drawio.png)

Dưới đây là bảng liệt kê toàn bộ **8 dịch vụ AWS cốt lõi** được ứng dụng trong dự án CodExecute, làm rõ vai trò, nhiệm vụ và lý do kỹ thuật lựa chọn:

| STT | Dịch vụ AWS | Vai trò & Nhiệm vụ trong CodExecute | Lý do lựa chọn & Lợi ích kỹ thuật |
| :---: | :--- | :--- | :--- |
| **1** | **Amazon CloudFront** | Phân phối ứng dụng React Frontend từ S3 Bucket đến người dùng cuối với độ trễ thấp nhất. Đóng vai trò Reverse Proxy điều hướng `/api/*` tới API Gateway. | Tăng tốc tải trang toàn cầu bằng caching ở các Edge Locations. Hỗ trợ HTTPS/TLS 1.3 tự động, tích hợp sẵn AWS Shield bảo vệ chống DDoS lớp 3/4. |
| **2** | **Amazon S3** | **Bucket 1:** Lưu trữ static assets (HTML/JS/CSS) của Frontend.<br>**Bucket 2:** Lưu trữ bộ Testcase (Input/Output text files) bài tập.<br>**Bucket 3:** Lưu trữ file avatar người dùng. | Chi phí lưu trữ cực rẻ ($0.023/GB), độ tin cậy 99.999999999% (11 số 9 durability). Khả năng mở rộng dung lượng vô hạn, tích hợp S3 Presigned URL để upload file an toàn. |
| **3** | **Amazon DynamoDB** | Lưu trữ toàn bộ dữ liệu cấu trúc của hệ thống bao gồm: Bảng `Users`, `Problems`, `Submissions`, `TestCases`, `Posts`, `Notifications`, `UserFollows`. | Tốc độ truy vấn ổn định ở mức single-digit millisecond. Khả năng tự động mở rộng (On-Demand capacity) xử lý hàng triệu request mà không cần quản lý cluster hay sharding phức tạp. |
| **4** | **AWS Lambda** | **Lambda API:** Chạy ứng dụng FastAPI backend xử lý logic RESTful API.<br>**Lambda Worker:** Tiêu thụ tin nhắn từ SQS, biên dịch và thực thi code của người dùng (C++, Java, Python, JS) trong môi trường Sandbox cách ly. | Mô hình tính phí theo millisecond (chỉ trả tiền khi code chạy). Tự động scale từ 0 lên hàng nghìn request tức thì mà không cần quản lý server hay container cluster. |
| **5** | **Amazon SQS** | Làm hàng chờ bất đồng bộ lưu trữ các yêu cầu nộp bài (`Submissions Queue`) từ Lambda API trước khi chuyển cho Worker chấm bài. | Đóng vai trò Buffer chống nghẽn hệ thống khi có đợt nộp bài đột biến (Traffic Spikes). Đảm bảo không mất mát dữ liệu nhờ cơ chế Message Retention và Dead-Letter Queue (DLQ). |
| **6** | **AWS IAM** | Quản lý định danh, cấp quyền tối thiểu (Least Privilege Access) cho các dịch vụ Lambda, S3, DynamoDB, SQS giao tiếp với nhau. | Đảm bảo nguyên tắc bảo mật Zero-Trust. Ngăn chặn quyền truy cập trái phép giữa các thành phần hệ thống thông qua IAM Roles & Resource-based Policies. |
| **7** | **Amazon CloudWatch** | Thu thập toàn bộ Logs (CloudWatch Logs) từ Lambda API & Worker, API Gateway. Theo dõi chỉ số hiệu năng (Metrics) và thiết lập Cảnh báo (Alarms). | Giám sát sức khỏe hệ thống thời gian thực. Cảnh báo qua Alarm khi tỷ lệ lỗi > 1% hoặc CPU/RAM quá tải. Hỗ trợ truy vết lỗi (Debugging) nhanh chóng. |
| **8** | **Amazon API Gateway** | Làm cổng vào duy nhất (API Gateway) quản lý các RESTful API endpoints, điều hướng request đến Lambda API Handler. | Hỗ trợ Rate Limiting / Throttling phòng chống Spam API, xác thực JWT Token, tích hợp trực tiếp với CORS và CloudFront Custom Domain. |

---

## 8. KẾ HOẠCH TRIỂN KHAI & TIMELINE 

Dự án được triển khai trong vòng **7 tuần** (từ ngày **15/06/2026** đến ngày **31/07/2026**) chia thành **5 Giai Đoạn (Phases)** với các cột mốc quan trọng (Milestones):

* **Phase 1: Khởi Tạo Hạ Tầng & IaC** *(15/06/2026 – 24/06/2026)*
  > Khởi tạo kiến trúc hạ tầng S3, DynamoDB, SQS, IAM Roles chuẩn AWS Well-Architected bằng AWS SAM & Terraform.

* **Phase 2: Xây Dựng Frontend & Backend Core** *(25/06/2026 – 05/07/2026)*
  > Phát triển giao diện React + Vite (Trình soạn thảo Code, Bộ bài tập) và hệ thống RESTful APIs với FastAPI trên AWS Lambda.

* **Phase 3: Xử Lý Bất Đồng Bộ & Lambda Sandbox** *(06/07/2026 – 16/07/2026)*
  > Xây dựng hàng chờ Amazon SQS và triển khai Lambda Worker thực thi mã nguồn tự động trong môi trường Sandbox cách ly.

* **Phase 4: Kiểm Thử Chịu Tải & Bảo Mật Hardening** *(17/07/2026 – 25/07/2026)*
  > Kiểm thử chịu tải (1,000 VUs với Locust/k6), đánh giá an ninh bảo mật chống RCE và thử nghiệm cơ chế DLQ failover.

* **Phase 5: Tối Ưu Chi Phí & Chính Thức Go-Live** *(26/07/2026 – 31/07/2026)*
  > Tối ưu cấu hình RAM/Timeout Lambda, thiết lập cảnh báo CloudWatch Budgets và chính thức **GO-LIVE ngày 31/07/2026**.

### Chi Tiết Từng Giai Đoạn

| Giai Đoạn | Thời Gian | Công Việc Chi Tiết | Sản Phẩm Đầu Ra (Deliverables) |
| :--- | :--- | :--- | :--- |
| **Phase 1: Khởi Tạo Hạ Tầng & IaC** | **15/06/2026 – 24/06/2026** *(10 ngày)* | - Thiết kế kiến trúc chi tiết chuẩn AWS Well-Architected.<br>- Viết kịch bản AWS SAM / Terraform khởi tạo S3 Buckets, DynamoDB Tables, SQS Queues, IAM Roles.<br>- Thiết lập Repository, CI/CD Pipeline với GitHub Actions. | - Cụm hạ tầng AWS Dev environment sẵn sàng.<br>- Codebase SAM Template hoàn chỉnh. |
| **Phase 2: Xây Dựng Frontend & Backend Core** | **25/06/2026 – 05/07/2026** *(11 ngày)* | - Phát triển ứng dụng React + Vite Frontend (Code Editor, Task filtering, User profile).<br>- Phát triển FastAPI Backend với các endpoint Authentication (JWT), Problems API, Submissions API.<br>- Đóng gói Lambda API Handler. Tích hợp API Gateway & CloudFront. | - Web UI chạy mượt trên CloudFront + S3.<br>- RESTful API hoạt động đầy đủ trên Lambda & DynamoDB. |
| **Phase 3: Xử Lý Bất Đồng Bộ & Lambda Sandbox** | **06/07/2026 – 16/07/2026** *(11 ngày)* | - Xây dựng mô hình đẩy job nộp bài vào Amazon SQS Queue.<br>- Viết Lambda Worker Runner cách ly thực thi 4 ngôn ngữ (C++, Java, Python, JS).<br>- Triển khai AWS Lambda Worker nhận job từ SQS, đọc Testcase từ S3 và chấm điểm. | - Luồng chấm bài tự động bất đồng bộ hoàn tất.<br>- Chống RCE thành công trong Lambda Sandbox. |
| **Phase 4: Kiểm Thử & Bảo Mật Hardening** | **17/07/2026 – 25/07/2026** *(9 ngày)* | - Kiểm thử chịu tải (Load Testing với Locust/k6 lên đến 1,000 VUs).<br>- Kiểm thử bảo mật (Penetration Testing) chống RCE, SQLi, XSS, Resource Exhaustion.<br>- Thử nghiệm trường hợp đứt gãy hạ tầng (Failover & DLQ testing). | - Báo cáo Load Test đạt chỉ số cam kết.<br>- Khắc phục 100% lỗ hổng bảo mật mức High/Critical. |
| **Phase 5: Tối Ưu Chi Phí & Chính Thức Go-Live** | **26/07/2026 – 31/07/2026** *(6 ngày)* | - Thiết lập CloudWatch Alarms & Cost Budgets Alert.<br>- Tối ưu hóa dung lượng RAM/Timeout của AWS Lambda.<br>- Chuyển đổi môi trường Production & Bàn giao toàn bộ tài liệu dự án. | - **Dự án CHÍNH THỨC GO-LIVE ngày 31/07/2026.**<br>- Bộ tài liệu kiến trúc & vận hành chi tiết. |

---

## 9. ƯỚC TÍNH NGÂN SÁCH & ĐÁNH GIÁ COST OPTIMIZATION

### 1. Ước Tính Ngân Sách Hàng Tháng 

Dự toán ngân sách được tính dựa trên quy mô vận hành trung bình: **100,000 bài nộp/tháng** và **500,000 API requests/tháng**.

| Dịch Vụ AWS | Mức Độ Sử Dụng Dự Kiến / Tháng | Đơn Giá Tham Chiếu (ap-southeast-1) | Chi Phí Hàng Tháng (USD) |
| :--- | :--- | :--- | :---: |
| **AWS Lambda** | 500,000 API Requests + 100,000 Worker Sandbox executions (Memory: 512MB, Avg duration: 800ms) | $0.20 / 1M Requests + Compute time | **$3.80** |
| **Amazon API Gateway** | 500,000 HTTP API calls | $1.00 / 1M Requests | **$0.50** |
| **Amazon SQS** | 200,000 SQS Requests (SendMessage + ReceiveMessage) | $0.40 / 1M Requests | **$0.08** |
| **Amazon DynamoDB** | 1,000,000 Read/Write Units (On-Demand Mode) + 5GB Data Storage | $0.25 / 1M WCU, $0.05 / 1M RCU | **$3.20** |
| **Amazon S3** | 15GB Storage (Testcases + User Media + Web Assets) + 100k GET/PUT | $0.023 / GB | **$0.65** |
| **Amazon CloudFront** | 50GB Data Transfer Out + 500k HTTPS Requests | $0.09 / GB | **$4.50** |
| **Amazon CloudWatch** | 3GB Ingestion Logs + 5 Custom Metrics + 3 Alarms | $0.57 / GB Logs | **$2.70** |
| **AWS IAM** | Toàn bộ IAM Users, Roles, Policies | Miễn phí | **$0.00** |
| **TỔNG CỘNG CHI PHÍ DỰ KIẾN / THÁNG:** | | | **~$15.43 USD / tháng** |

> 💡 *Lưu ý:* Trong 12 tháng đầu tiên triển khai, phần lớn chi phí trên sẽ nằm trong gói **AWS Free Tier** (1M Lambda requests/tháng, 1M API Gateway requests/tháng, 25GB DynamoDB storage, 5GB S3 storage), giúp chi phí thực tế duy trì ở mức **< $3.00 USD/tháng**.

---

### 2. Chiến Lược Tối Ưu Chi Phí

1. **Mô hình Pure Serverless Pay-As-You-Go:**
   - Sử dụng **AWS Lambda** và **API Gateway HTTP API** (rẻ hơn 70% so với REST API) giúp hệ thống không tốn bất kỳ chi phí duy trì máy chủ nào khi không có người dùng truy cập.

2. **Tối ưu hóa DynamoDB On-Demand vs Provisioned:**
   - Trong giai đoạn đầu triển khai, sử dụng chế độ **On-Demand Capacity** để trả tiền chính xác theo số lượng read/write. Khi lưu lượng ổn định, chuyển các bảng chính sang **Provisioned Capacity + Auto Scaling** để giảm thêm 40% chi phí.

3. **S3 Lifecycle Rules & Compression:**
   - Cấu hình **S3 Lifecycle Rules** chuyển các log testcase cũ hơn 90 ngày sang lớp lưu trữ **S3 Glacier Flexible Retrieval** (giảm 85% chi phí lưu trữ).
   - Nén bộ Testcase bằng Gzip trước khi lưu trên S3 giúp giảm băng thông truyền tải.

4. **Sử Dụng SQS Long Polling:**
   - Cấu hình SQS `ReceiveMessageWaitTimeSeconds = 20` (Long Polling). Điều này giúp giảm số lượng yêu cầu kiểm tra tin nhắn rỗng (Empty Receive Requests), tiết kiệm chi phí gọi SQS API lên tới 90%.

5. **AWS Lambda Power Tuning:**
   - Sử dụng công cụ *AWS Lambda Power Tuning* để tìm mức RAM/vCPU tối ưu nhất cho Lambda API & Worker, giúp vừa tăng tốc độ phản hồi vừa giảm thời gian tính phí.

---

## 10. ĐÁNH GIÁ RỦI RO DỰ ÁN & BIỆN PHÁP GIẢM THIỂU

| STT | Loại Rủi Ro | Phân Tích Chi Tiết Rủi Ro | Mức Độ | Biện Pháp Giảm Thiểu (Mitigation Strategy) |
| :---: | :--- | :--- | :---: | :--- |
| **1** | **Bảo Mật (Security)** | **RCE (Remote Code Execution) trong Sandbox:** Người dùng viết mã độc nhằm đọc file hệ thống, thực hiện fork bomb làm kiệt quệ CPU/RAM, hoặc truy cập mạng nội bộ AWS. | **CRITICAL** | - Thực thi code trong **Lambda Isolated Sandbox** không có root privilege.<br>- Tắt toàn bộ kết nối mạng ngoài (`VPC Network: Disabled / Strict Subnet Security Groups`) trong Sandbox Runner.<br>- Áp dụng giới hạn cứng tài nguyên: RAM (max 512MB), CPU (max 1 Core), Timeout (max 5s), Process limit (max 20 pids). |
| **2** | **Hiệu Năng (Performance)** | **Nghẽn hàng chờ SQS hoặc Cold Start Lambda:** Khi có đợt thi nộp bài tập trung (hàng nghìn bài nộp/phút), Cold Start của Lambda có thể làm tăng độ trễ chấm bài. | **HIGH** | - Thiết lập **Provisioned Concurrency** cho các hàm Lambda quan trọng trong thời gian diễn ra kỳ thi.<br>- Tăng số lượng Worker tự động scale theo chỉ số `ApproximateNumberOfMessagesVisible` của SQS. |
| **3** | **Bảo Mật (Security)** | **Tấn công DDoS / Spam API:** Kẻ xấu liên tục gửi request rác làm cạn kiệt ngân sách AWS (Financial Exhaustion). | **HIGH** | - Bật **AWS WAF** và cấu hình **API Gateway Throttling / Rate Limiting** (giới hạn max 20 requests/phút cho mỗi IP/User).<br>- Tích hợp CloudFront Geo Blocking và AWS Shield Standard. |
| **4** | **Vận Hành (Operations)** | **Mất mát bài nộp do lỗi hệ thống (Data Loss):** Worker chấm bài bị crash bất ngờ khi đang xử lý job từ SQS. | **MEDIUM** | - Cấu hình SQS `VisibilityTimeout` phù hợp.<br>- Bật **Dead-Letter Queue (DLQ)**: Tin nhắn lỗi quá 3 lần sẽ được đẩy vào DLQ để kỹ sư kiểm tra lại mà không bị mất dữ liệu. |
| **5** | **Quản Lý Chi Phí (Cost)** | **Chi phí gia tăng đột biến (Spike Cost):** Lỗi vòng lặp vô tận trong code Lambda hoặc ghi log quá nhiều vào CloudWatch. | **MEDIUM** | - Thiết lập **AWS Budgets Alert** cảnh báo tự động qua Email/Slack khi chi phí vượt $20 USD/tháng.<br>- Cấu hình CloudWatch Log Retention Period tối đa 14 ngày thay vì để vĩnh viễn. |

---

## 11. KẾT QUẢ KỲ VỌNG

Sau khi hoàn thành triển khai vào ngày **31/07/2026**, hệ thống **CodExecute** dự kiến đạt được các chỉ số kỹ thuật và mục tiêu kinh doanh sau:

### Chỉ Số Kỹ Thuật (Technical KPIs)
* **Độ sẵn sàng (Availability SLA):** Đạt tối thiểu **99.9%** thời gian hoạt động ổn định nhờ hạ tầng Multi-AZ Serverless.
* **Thời gian phản hồi API (API Response Latency):** < 200ms đối với các tác vụ đọc/ghi dữ liệu thông thường qua API Gateway & Lambda.
* **Thời gian chấm bài (Submission Processing Time):** < 2.0 giây kể từ lúc người dùng nhấn Submit đến khi nhận kết quả (đối với các bài tập có bộ Testcase dưới 50MB).
* **Khả năng chịu tải (Concurrent Capacity):** Xử lý mượt mà tối thiểu **1,000 bài nộp đồng thời (concurrent submissions)** mà không xảy ra hiện tượng drop request hay nghẽn hệ thống.
* **An toàn tuyệt đối (Zero RCE Vulnerability):** 100% mã nguồn người dùng được kiểm soát và cách ly hoàn toàn trong Lambda Sandbox, không phát sinh bất kỳ sự cố rò rỉ bảo mật nào.

### Giá Trị Vận Hành & Kinh Doanh (Business Outcomes)
* **Tối ưu chi phí:** Tiết kiệm hơn **75%** chi phí vận hành hạ tầng so với việc thuê máy chủ truyền thống (EC2/Dedicated Server).
* **Khả năng bảo trì cao:** Hạ tầng quản lý 100% bằng kịch bản Code (IaC SAM/CloudFormation), giúp việc tạo mới hoặc khôi phục môi trường thử nghiệm chỉ mất dưới 15 phút.
* **Trải nghiệm người dùng vượt trội:** Cung cấp cho cộng đồng lập trình viên Việt Nam một nền tảng thực thi thuật toán chuẩn hóa, tin cậy, góp phần nâng cao kỹ năng lập trình và chuẩn bị cho các kỳ thi tuyển dụng công nghệ.

---
