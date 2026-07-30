---
title: "Giám sát với CloudWatch"
date: 2026-07-30
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---


Trong phần này, chúng ta ghi lại cách **Amazon CloudWatch** được sử dụng để giám sát hệ thống serverless CodExecute — bao gồm log thực thi Lambda, metric SQS queue, và thiết lập alarm để phát hiện bất thường trong pipeline chấm bài.

Toàn bộ Lambda function tự động gửi log có cấu trúc lên **CloudWatch Logs** thông qua module `logging` của Python được cấu hình trong `app/main.py`:

```python
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(name)s: %(message)s",
    handlers=[logging.StreamHandler(sys.stdout)]
)
```

Trên Lambda, `stdout` được tự động thu thập và stream lên CloudWatch Logs mà không cần cấu hình thêm bất kỳ điều gì.

---

### Bước 1: Truy Cập Lambda Log Groups

Mỗi Lambda function được tạo riêng một **Log Group** tự động ngay trong lần invocation đầu tiên:

| Lambda Function | Log Group |
|---|---|
| `codeexecute-api` | `/aws/lambda/codeexecute-api` |
| `codeexecute-worker` | `/aws/lambda/codeexecute-worker` |

1. Mở **Amazon CloudWatch Console** → **Logs** → **Log groups**.
2. Tìm kiếm `/aws/lambda/codeexecute` để thấy cả hai log group.

<div align="center">

<img src="/images/5-Workshop/5.8-CloudWatch/log-groups.jpg" alt="Lambda Log Groups trong CloudWatch" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.8.1: CloudWatch Log Groups của codeexecute-api và codeexecute-worker</i>
</p>

</div>

---

### Bước 2: Theo Dõi Log Thực Thi Của Lambda Worker

Lambda Worker phát ra log có cấu trúc chi tiết ở từng giai đoạn chấm bài. Chọn log group `/aws/lambda/codeexecute-worker` → mở **Log stream** gần nhất.

Log stream của một lần chấm bài thành công điển hình:

```
📥 Lambda Worker invoked with event: {"Records": [...]}
[Worker] Starting execution for submission abc-123 (Problem: two-sum, Lang: python)
Loaded 3 testcases for problem two-sum
[Worker] Execution finished for submission abc-123. Result: Accepted
```

Bài nộp thất bại (ví dụ: Wrong Answer):

```
📥 Lambda Worker invoked with event: {"Records": [...]}
[Worker] Starting execution for submission def-456 (Problem: two-sum, Lang: cpp)
Loaded 3 testcases for problem two-sum
[Worker] Execution finished for submission def-456. Result: Wrong Answer
```

<div align="center">

<img src="/images/5-Workshop/5.8-CloudWatch/worker-logs.jpg" alt="CloudWatch log stream của Lambda Worker chấm bài" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.8.2: Log stream của Lambda Worker hiển thị một chu kỳ chấm bài hoàn chỉnh</i>
</p>

</div>

---

### Bước 3: Theo Dõi Log Của Lambda API

Lambda API ghi log mỗi HTTP request đến qua Mangum/FastAPI, bao gồm đường dẫn endpoint, thời gian thực thi và các lỗi service-level (DynamoDB, SQS). Chọn log group `/aws/lambda/codeexecute-api` → mở log stream mới nhất.

Các mẫu log quan trọng cần chú ý:

```
[INFO] sqs_service: Starting push_submission_to_queue for submission_id: abc-123
[INFO] sqs_service: Successfully pushed submission abc-123 to SQS Queue. MessageId: ...
[WARNING] sqs_service: SQS_QUEUE_URL is not configured. Skipping SQS message push.
[ERROR] sqs_service: Error sending message to SQS (falling back to local background execution): ...
```

<div align="center">

<img src="/images/5-Workshop/5.8-CloudWatch/api-logs.jpg" alt="CloudWatch log stream của Lambda API" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.8.3: Log stream của Lambda API hiển thị các sự kiện đẩy SQS và xử lý request</i>
</p>

</div>

---

### Bước 4: Xem Dashboard Metrics Của Lambda

CloudWatch tự động thu thập các metric thực thi quan trọng cho mỗi Lambda mà không cần instrumentation.

1. Trong CloudWatch Console → **Metrics** → **Lambda** → chọn `codeexecute-worker`.
2. Các metric cần theo dõi:

   | Metric | Mô tả | Ngưỡng Alert |
   |---|---|---|
   | **Invocations** | Tổng số lần Lambda được kích hoạt | Theo dõi baseline |
   | **Errors** | Số lần invocation thất bại | > 0 kích hoạt alert |
   | **Duration** | Thời gian thực thi mỗi lần (ms) | > 200,000ms (timeout Lambda Worker) |
   | **Throttles** | Số lần bị từ chối do giới hạn concurrency | > 0 kích hoạt alert |
   | **ConcurrentExecutions** | Số invocation đang chạy đồng thời | Theo dõi khi tải cao |

<div align="center">

<img src="/images/5-Workshop/5.8-CloudWatch/lambda-metrics.jpg" alt="Lambda metrics trong CloudWatch" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.8.4: Dashboard CloudWatch metrics cho Lambda codeexecute-worker</i>
</p>

</div>

---

### Bước 5: Theo Dõi Metrics Của SQS Queue

SQS cũng phát ra CloudWatch metrics sẵn có cho `codeexecute-submission-queue`.

1. Trong CloudWatch → **Metrics** → **SQS** → chọn `codeexecute-submission-queue`.
2. Các metric cần theo dõi:

   | Metric | Mô tả |
   |---|---|
   | **NumberOfMessagesSent** | Số bài nộp được Lambda API đẩy vào queue |
   | **NumberOfMessagesDeleted** | Số bài đã được Worker xử lý thành công |
   | **ApproximateNumberOfMessagesVisible** | Số message đang chờ trong queue (backlog) |
   | **ApproximateAgeOfOldestMessage** | Tuổi của message cũ nhất chưa xử lý — chỉ số alert quan trọng |

<div align="center">

<img src="/images/5-Workshop/5.8-CloudWatch/sqs-metrics.jpg" alt="SQS queue metrics trong CloudWatch" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.8.5: CloudWatch SQS metrics cho codeexecute-submission-queue</i>
</p>

</div>



### Kết Quả

CloudWatch monitoring đã được cấu hình đầy đủ cho hệ thống CodExecute:

| Thành phần | Tài nguyên CloudWatch | Mục đích |
|---|---|---|
| `codeexecute-api` | Log Group `/aws/lambda/codeexecute-api` | Log request API, sự kiện đẩy SQS |
| `codeexecute-worker` | Log Group `/aws/lambda/codeexecute-worker` | Log thực thi chấm bài, output sandbox |
| `codeexecute-worker` | Metrics: Errors, Duration, Throttles | Giám sát hiệu năng và độ tin cậy |
| `codeexecute-submission-queue` | SQS Metrics | Giám sát backlog và thông lượng queue |

Toàn bộ dữ liệu log và metric được lưu trữ trong CloudWatch, phục vụ debug bài nộp thất bại, tối ưu hóa giá trị timeout Lambda và theo dõi tình trạng hệ thống theo thời gian.
