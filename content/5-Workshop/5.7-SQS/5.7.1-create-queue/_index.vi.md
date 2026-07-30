---
title: "Tạo SQS Queue"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

# TẠO AMAZON SQS SUBMISSION QUEUE

Trong bước này, chúng ta tạo **Amazon SQS Standard Queue** (`codeexecute-submission-queue`) làm bộ đệm bất đồng bộ giữa Lambda API và Lambda Worker. Tên queue được ứng dụng tham chiếu qua biến môi trường `SQS_QUEUE_URL` trong `app/core/config.py`.

---

### Bước 1: Tạo SQS Queue

1. Truy cập **Amazon SQS Console** → nhấn **Create queue**.
2. Chọn loại queue **Standard** (không phải FIFO — Standard đủ cho workload chấm bài và cho thông lượng cao hơn).
3. Nhập tên queue: `codeexecute-submission-queue`.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/sqs-create-1.jpg" alt="Tạo SQS queue - tên và loại" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.7.1: Chọn loại Standard queue và nhập tên queue</i>
</p>

</div>

4. Cấu hình các thông số queue:

   | Thông số | Giá trị | Lý do |
   |---|---|---|
   | **Visibility timeout** | `300` giây (5 phút) | Phải ≥ timeout của Lambda Worker để tránh xử lý trùng lặp |
   | **Message retention period** | `86400` giây (1 ngày) | Giữ lại message chưa xử lý trong 24 giờ |
   | **Maximum message size** | `256 KB` (mặc định) | Đủ cho payload code submission |
   | **Delivery delay** | `0` giây | Phân phối ngay đến Worker |
   | **Receive message wait time** | `0` giây | Short polling (Lambda dùng cơ chế polling riêng) |

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/sqs-create-2.jpg" alt="Cấu hình thông số SQS queue" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.7.2: Cấu hình visibility timeout và message retention period</i>
</p>

</div>

5. Giữ nguyên **Access policy** mặc định (IAM role của Lambda sẽ tự động được phép truy cập queue).
6. Nhấn **Create queue**.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/sqs-created.jpg" alt="Tạo SQS queue thành công" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.7.3: codeexecute-submission-queue đã được tạo thành công</i>
</p>

</div>

---

### Bước 2: Sao Chép Queue URL

Sau khi tạo xong, sao chép **Queue URL** từ trang chi tiết của queue. URL này được ứng dụng sử dụng để gửi message:

```
https://sqs.ap-southeast-1.amazonaws.com/014936669466/codeexecute-submission-queue
```

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/sqs-url.jpg" alt="SQS Queue URL" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.7.4: Sao chép Queue URL từ SQS Console</i>
</p>

</div>

---

### Bước 3: Cấu Hình Queue URL Vào Biến Môi Trường Lambda API

Lambda `codeexecute-api` đọc `SQS_QUEUE_URL` từ biến môi trường để biết nơi đẩy job chấm bài.

1. Mở **AWS Lambda Console** → chọn function `codeexecute-api` → **Configuration** → **Environment variables** → **Edit**.
2. Thêm biến môi trường:

   - **Key**: `SQS_QUEUE_URL`
   - **Value**: `https://sqs.ap-southeast-1.amazonaws.com/014936669466/codeexecute-submission-queue`

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/lambda-env-sqs.jpg" alt="Thêm SQS_QUEUE_URL vào biến môi trường Lambda" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.7.5: Thêm biến môi trường SQS_QUEUE_URL vào Lambda codeexecute-api</i>
</p>

</div>

3. Nhấn **Save**.

Biến môi trường này ánh xạ trực tiếp đến setting trong `app/core/config.py`:

```python
SQS_QUEUE_URL: str = ""   # nạp từ biến môi trường Lambda
```

Và được sử dụng trong `app/services/sqs_service.py`:

```python
response = sqs_client.send_message(
    QueueUrl=settings.SQS_QUEUE_URL,
    MessageBody=json.dumps(message_body)
)
```

---

### Kết Quả

SQS Standard Queue `codeexecute-submission-queue` đã Active. Bước tiếp theo kết nối queue này với Lambda `codeexecute-worker` thông qua **Event Source Mapping**, để mỗi message đẩy vào queue sẽ tự động kích hoạt Worker xử lý.
