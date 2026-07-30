---
title: "Khởi tạo & Cấu hình AWS Lambda"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

<!-- # KHỞI TẠO VÀ CẤU HÌNH AWS LAMBDA FUNCTIONS TỪ CONTAINER IMAGE -->

Trong phần này, chúng ta sẽ thực hiện khởi tạo hai hàm **AWS Lambda Functions** (`codeexecute-worker` và `codeexecute-api`) trực tiếp trên **AWS Console** sử dụng **Container Image** đã được push lên Amazon ECR ở mục 5.4.1. Sau đó, chúng ta tiến hành cấu hình bộ nhớ (Memory), thời gian chờ tối đa (Timeout), biến môi trường (Environment Variables) và kích hoạt SQS Trigger.

---

### Bước 1: Khởi Tạo Lambda Function Cho Worker (`codeexecute-worker`)

1. Truy cập vào bảng điều khiển **AWS Lambda Console** và chọn **Create function**.
2. Thiết lập các tham số cơ bản cho Lambda Worker:
   - **Options:** Chọn **Container image**.
   - **Function name:** Nhập `codeexecute-worker`.
   - **Container image URI:** Nhấn **Browse images**, chọn repository `codexecute-lambda-worker` và chọn image tag `latest`.
   - **Architecture:** Chọn **x86_64**.
   - **Execution role:** Chọn **Use an existing role** và chọn IAM Role có quyền đọc S3, ghi DynamoDB và nhận tin nhắn từ SQS.
3. Nhấn **Create function**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-worker-create.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-worker-create.jpg" alt="Khởi tạo Lambda Worker từ Container Image" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.2.1: Khởi tạo Lambda codeexecute-worker từ ECR Container Image</i>
</p>

</div>

4. Cấu hình thông số thực thi (**General configuration**):
   - Chuyển sang tab **Configuration** → mục **General configuration** và chọn **Edit**.
   - **Memory:** Cấu hình **1024 MB** (hoặc 2048 MB để tăng tốc độ chạy g++/Corretto Java compiler trong sandbox).
   - **Timeout:** Nhập **30 seconds** (hoặc 1 minute).
   - Nhấn **Save**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-worker-config.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-worker-config.jpg" alt="Cấu hình Memory và Timeout cho Lambda Worker" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.2.2: Thiết lập dung lượng bộ nhớ Memory và Timeout cho Lambda Worker</i>
</p>

</div>

5. Thêm Trigger nhận bài nộp tự động từ **Amazon SQS Queue**:
   - Tại sơ đồ **Function overview**, nhấn **Add trigger**.
   - **Select a trigger:** Chọn **SQS**.
   - **SQS queue:** Chọn queue `codexecute-submissions-queue`.
   - **Batch size:** Nhập `1` hoặc `5`.
   - Nhấn **Add**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-worker-sqs-trigger.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-worker-sqs-trigger.jpg" alt="Gán SQS Trigger cho Lambda Worker" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.2.3: Gán SQS Queue codexecute-submissions-queue làm Trigger cho Worker</i>
</p>

</div>

---

### Bước 2: Khởi Tạo Lambda Function Cho API (`codeexecute-api`)

1. Chọn nút **Create function** trên **AWS Lambda Console**.
2. Thiết lập các tham số cơ bản cho Lambda API Backend:
   - **Options:** Chọn **Container image**.
   - **Function name:** Nhập `codeexecute-api`.
   - **Container image URI:** Nhấn **Browse images**, chọn repository `codexecute-lambda-api` và chọn image tag `latest`.
   - **Architecture:** Chọn **x86_64**.
   - **Execution role:** Chọn **Use an existing role** với các quyền truy cập DynamoDB, S3, SQS.
3. Nhấn **Create function**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-api-create.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-api-create.jpg" alt="Khởi tạo Lambda API từ Container Image" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.2.4: Khởi tạo Lambda codeexecute-api từ ECR Container Image</i>
</p>

</div>

4. Cấu hình thông số thực thi (**General configuration**):
   - Chuyển sang tab **Configuration** → **General configuration** → nhấn **Edit**.
   - **Memory:** Cấu hình **512 MB** (hoặc 1024 MB).
   - **Timeout:** Nhập **15 seconds** (hoặc 30 seconds).
   - Nhấn **Save**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-api-config.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-api-config.jpg" alt="Cấu hình Memory và Timeout cho Lambda API" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.2.5: Thiết lập bộ nhớ Memory và Timeout cho Lambda API</i>
</p>

</div>

5. Cấu hình biến môi trường hệ thống (**Environment variables**):
   - Chuyển sang mục **Configuration** → **Environment variables** → chọn **Edit**.
   - Nhập danh sách các biến môi trường cho FastAPI kết nối với DynamoDB, S3 và SQS:

| Key | Value | Mô tả |
| :--- | :--- | :--- |
| `AWS_REGION` | `ap-southeast-1` | Vùng AWS đang triển khai hệ thống |
| `SQS_QUEUE_URL` | `https://sqs.ap-southeast-1.amazonaws.com/.../codexecute-submissions-queue` | URL của hàng đợi SQS nộp bài |
| `TESTCASES_BUCKET` | `codeexecute-testcases` | S3 Bucket lưu trữ bộ dữ liệu testcase |
| `USER_MEDIA_BUCKET` | `codeexecute-user-media` | S3 Bucket lưu trữ avatar và media |

   - Nhấn **Save**.

---

### Bước 3: Xác Minh Trạng Thái Sẵn Sàng Của Các Hàm Lambda

1. Truy cập lại màn hình chính **AWS Lambda Console** → **Functions**.
2. Xác nhận cả hai hàm `codeexecute-worker` và `codeexecute-api` đều hiển thị trạng thái **Active** và trỏ đúng ECR Container Image URI.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-list-verify.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-list-verify.jpg" alt="Xác minh danh sách Lambda Functions" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.2.6: Hai hàm Lambda codeexecute-worker và codeexecute-api hiển thị sẵn sàng</i>
</p>

</div>

---

### Kết Quả

Đến bước này, cả hai dịch vụ Backend chính của hệ thống CodExecute đã được khởi tạo thành công trên hạ tầng **AWS Lambda** dưới dạng Container Images và sẵn sàng tiếp nhận các yêu cầu xử lý từ API Gateway cũng như SQS Queue.
