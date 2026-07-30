---
title: "Dọn dẹp tài nguyên"
date: 2026-07-30
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

<!-- # DỌN DEP TÀI NGUYÊN HỆ THỐNG CODEXECUTE -->

Sau khi hoàn thành các bài lab thực hành triển khai nền tảng **CodExecute**, việc dọn dẹp các tài nguyên AWS đã khởi tạo là rất quan trọng để tránh phát sinh chi phí duy trì ngoài ý muốn.

Trong phần này, chúng ta sẽ lần lượt xóa sạch toàn bộ tài nguyên theo thứ tự phụ thuộc hệ thống: **CloudFront**, **API Gateway**, **AWS Lambda**, **Amazon ECR**, **Amazon SQS**, **Amazon DynamoDB**, **Amazon S3**, **Amazon CloudWatch Log Groups**, **Amazon CloudWatch Alarms** và **Amazon SNS Topics**.

---

### Bước 1: Vô Hiệu Hóa & Xóa Amazon CloudFront Distribution

1. Truy cập **Amazon CloudFront Console** → **Distributions**.
2. Chọn Distribution đã khởi tạo cho dự án CodExecute (ID: `E14SU7QS7NEEO8`).
3. Nhấn **Disable** và chờ trạng thái chuyển từ *Enabled* sang *Disabled*.
4. Sau khi đã vô hiệu hóa hoàn tất, chọn Distribution và nhấn **Delete**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: disable-cloudfront.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/disable-cloudfront.jpg" alt="Vô hiệu hóa CloudFront Distribution" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.1.1: Vô hiệu hóa CloudFront Distribution E14SU7QS7NEEO8</i>
</p>

<!-- PLACEHOLDER FOR IMAGE: cleanup-cloudfront.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-cloudfront.jpg" alt="Xóa CloudFront Distribution" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.1.2: Xóa CloudFront Distribution E14SU7QS7NEEO8</i>
</p>

</div>

---

### Bước 2: Xóa Amazon API Gateway

1. Truy cập **Amazon API Gateway Console**.
2. Chọn API Gateway `codeexecute-api-gateway`.
3. Nhấn **Actions** (hoặc nút **Delete**) → Nhập tên API để xác nhận xóa.
4. Nhấn **Delete API**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-apigateway.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-apigateway.jpg" alt="Xóa API Gateway" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.2: Xóa HTTP API Gateway codeexecute-api-gateway</i>
</p>

</div>

---

### Bước 3: Xóa AWS Lambda Functions

1. Truy cập **AWS Lambda Console** → **Functions**.
2. Chọn hai hàm Lambda Backend:
   - `codeexecute-worker`
   - `codeexecute-api`
3. Nhấn vào menu **Actions** → chọn **Delete**.
4. Nhập từ khóa `delete` để xác nhận xóa cả 2 hàm Lambda.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-lambda.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-lambda.jpg" alt="Xóa AWS Lambda Functions" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.3: Xóa hai hàm Lambda codeexecute-worker và codeexecute-api</i>
</p>

</div>

---

### Bước 4: Xóa Amazon ECR Repositories

1. Truy cập **Amazon ECR Console** → **Repositories**.
2. Chọn hai private repositories:
   - `codexecute-lambda-worker`
   - `codexecute-lambda-api`
3. Nhấn **Delete** → Đánh dấu chọn tùy chọn xóa tất cả các tệp container image bên trong repository.
4. Nhập `delete` để xác nhận xóa hoàn toàn.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-ecr.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-ecr.jpg" alt="Xóa ECR Repositories" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.4: Xóa hai ECR Repositories codexecute-lambda-worker và codexecute-lambda-api</i>
</p>

</div>

---

### Bước 5: Xóa Amazon SQS Queue

1. Truy cập **Amazon SQS Console** → **Queues**.
2. Chọn hàng chờ bài nộp: `codexecute-submissions-queue`.
3. Nhấn nút **Delete**.
4. Nhập `delete` để xác nhận xóa SQS Queue.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-sqs.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-sqs.jpg" alt="Xóa SQS Queue" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.5: Xóa SQS Queue codexecute-submissions-queue</i>
</p>

</div>

---

### Bước 6: Xóa Các Bảng Amazon DynamoDB

1. Truy cập **Amazon DynamoDB Console** → **Tables**.
2. Lần lượt chọn 8 bảng dữ liệu của hệ thống:
   - `Users`, `Problems`, `Submissions`, `TestCases`, `Posts`, `Notifications`, `UserFollows`, `Solutions`.
3. Nhấn nút **Delete table**.
4. Nhập `delete` để xác nhận xóa tất cả các bảng dữ liệu.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-dynamodb.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-dynamodb.jpg" alt="Xóa các bảng DynamoDB" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.6: Xóa 8 bảng cơ sở dữ liệu DynamoDB</i>
</p>

</div>

---

### Bước 7: Làm Rỗng & Xóa Các Amazon S3 Buckets

1. Truy cập **Amazon S3 Console** → **Buckets**.
2. Thực hiện lần lượt với 3 S3 Buckets:
   - `codexecute-prod-frontend`
   - `codeexecute-testcases`
   - `codeexecute-user-media`
3. Nhấn **Empty** → Nhập `permanently delete` để làm rỗng toàn bộ đối tượng bên trong bucket.
4. Chọn lại bucket đã rỗng → Nhấn **Delete** → Nhập tên bucket để xóa hoàn tất.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: empty-s3.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/empty-s3.jpg" alt="Làm rỗng S3 Buckets" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.7.1: Làm rỗng các Amazon S3 Buckets dự án</i>
</p>

<!-- PLACEHOLDER FOR IMAGE: cleanup-s3.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-s3.jpg" alt="Xóa S3 Buckets" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.7.2: Xóa các Amazon S3 Buckets dự án</i>
</p>

</div>

---

### Bước 8: Xóa Amazon CloudWatch Log Groups

1. Truy cập **Amazon CloudWatch Console** → **Logs** → **Log groups**.
2. Tìm và chọn các log groups liên quan đến Lambda và API Gateway:
   - `/aws/lambda/codeexecute-worker`
   - `/aws/lambda/codeexecute-api`
   - Các log group của API Gateway.
3. Nhấn menu **Actions** → Chọn **Delete log group(s)** và xác nhận xóa.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-cloudwatch.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-cloudwatch.jpg" alt="Xóa CloudWatch Log Groups" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.8: Xóa các CloudWatch Log Groups phát sinh trong quá trình chạy hệ thống</i>
</p>

</div>

---

### Bước 9: Xóa Amazon CloudWatch Alarms

1. Truy cập **Amazon CloudWatch Console** → **Alarms** → **All alarms**.
2. Chọn các cảnh báo đã khởi tạo cho dự án CodExecute (ví dụ: Cảnh báo lỗi Lambda `codeexecute-api-errors`, Cảnh báo số lượng bài nộp đệm trong SQS Queue `sqs-high-message-count-alarm`).
3. Nhấn **Actions** → Chọn **Delete**.
4. Xác nhận xóa CloudWatch Alarms.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-cloudwatch-alarms.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-cloudwatch-alarms.jpg" alt="Xóa CloudWatch Alarms" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.9: Xóa các cảnh báo CloudWatch Alarms hệ thống</i>
</p>

</div>

---

### Bước 10: Xóa Amazon SNS Topics & Subscriptions

1. Truy cập **Amazon SNS Console** → **Topics**.
2. Chọn SNS Topic thông báo hệ thống (ví dụ: `codexecute-system-alerts-topic`).
3. Nhấn **Delete** → Nhập `delete me` để xác nhận xóa.
4. Truy cập **Subscriptions** → Chọn các đăng ký email/SMS thuộc topic và chọn **Delete**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: cleanup-sns.jpg -->
<img src="/images/5-Workshop/5.10-Cleanup/cleanup-sns.jpg" alt="Xóa Amazon SNS Topics" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.10.10: Xóa Amazon SNS Topics và Subscriptions thông báo</i>
</p>

</div>

---

### Kết Quả

Cuối cùng, tất cả tài nguyên và dịch vụ Serverless thuộc hệ thống **CodExecute** đã được dọn dẹp sạch sẽ. Tài khoản AWS của bạn hiện tại hoàn toàn giải phóng khỏi các hạ tầng tạm thời, giúp ngăn ngừa mọi chi phí phát sinh duy trì ngoài ý muốn.
