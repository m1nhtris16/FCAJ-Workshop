---
title: "Cài đặt SNS & Lambda Alarms"
date: 2026-07-30
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---


Trong phần này, chúng ta ghi lại cách **Amazon Simple Notification Service (SNS)** được cấu hình để gửi cảnh báo email theo thời gian thực khi Lambda function của CodExecute gặp lỗi. SNS đóng vai trò kênh thông báo mà CloudWatch Alarms sử dụng để gửi cảnh báo tới các subscriber.

Kiến trúc cảnh báo tổng thể:

```
CloudWatch Alarm (Lambda Errors / Throttles)
          │
          ▼ kích hoạt
    SNS Topic: codeexecute-alerts
          │
          ├─► Email: team@example.com
          └─► (mở rộng được: Slack, PagerDuty, SMS, v.v.)
```

---

### Bước 1: Tạo SNS Topic

1. Mở **Amazon SNS Console** → **Topics** → nhấn **Create topic**.
2. Chọn loại **Standard** (không phải FIFO).
3. Nhập tên topic: `codeexecute-alerts`.
4. Giữ nguyên các cài đặt còn lại.
5. Nhấn **Create topic**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-create.jpg" alt="Tạo SNS topic codeexecute-alerts" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.1: Tạo SNS Standard topic codeexecute-alerts</i>
</p>

</div>

6. Sau khi tạo xong, sao chép **Topic ARN** — cần dùng khi cấu hình CloudWatch Alarms:

```
arn:aws:sns:ap-southeast-1:014936669466:codeexecute-alerts
```

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-arn.jpg" alt="Topic ARN của SNS" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.2: Topic codeexecute-alerts đã tạo — sao chép Topic ARN</i>
</p>

</div>

---

### Bước 2: Đăng Ký Nhận Thông Báo Qua Email

1. Trên trang chi tiết topic `codeexecute-alerts` → nhấn **Create subscription**.
2. Cấu hình subscription:
   - **Protocol**: Email
   - **Endpoint**: Nhập địa chỉ email của bạn (ví dụ: `your-email@example.com`)
3. Nhấn **Create subscription**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-sub-create.jpg" alt="Tạo email subscription cho SNS" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.3: Tạo email subscription cho topic codeexecute-alerts</i>
</p>

</div>

4. Trạng thái subscription sẽ hiển thị **Pending confirmation**. Kiểm tra hộp thư và nhấn **Confirm subscription** trong email xác nhận từ AWS.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-sub-pending.jpg" alt="SNS subscription đang chờ xác nhận" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.4: Email subscription đang chờ xác nhận — nhấn link trong email thông báo từ AWS</i>
</p>

</div>

5. Sau khi xác nhận, trạng thái subscription chuyển sang **Confirmed**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-sub-confirmed.jpg" alt="SNS subscription đã xác nhận thành công" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.5: Email subscription đã được xác nhận và đang hoạt động</i>
</p>

</div>

---

### Bước 3: Tạo Alarm — Lambda Worker Errors

Alarm này kích hoạt khi Lambda `codeexecute-worker` gặp bất kỳ lỗi invocation nào, gửi ngay email cảnh báo qua SNS.

1. Mở **CloudWatch Console** → **Alarms** → **Create alarm** → **Select metric**.
2. Điều hướng đến **Lambda** → **By Function Name** → chọn `codeexecute-worker` → **Errors**.
3. Cấu hình điều kiện alarm:
   - **Statistic**: Sum
   - **Period**: 1 phút
   - **Threshold type**: Static
   - **Condition**: Greater than or equal to `1`
   - **Datapoints to alarm**: 1 out of 1

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/alarm-worker-condition.jpg" alt="Điều kiện alarm Lambda Worker errors" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.6: Cấu hình điều kiện alarm — kích hoạt khi Worker errors ≥ 1 trong 1 phút</i>
</p>

</div>

4. Tại mục **Notification** → **In alarm** → **Select an existing SNS topic** → chọn `codeexecute-alerts`.
5. Đặt tên alarm: `codeexecute-worker-errors`.
6. Nhấn **Create alarm**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/alarm-worker-sns.jpg" alt="Kết nối alarm với SNS topic" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.7: Chọn SNS topic codeexecute-alerts làm đích nhận thông báo cho Worker alarm</i>
</p>

</div>

---

### Bước 4: Tạo Alarm — Lambda Worker Throttles

Alarm này phát hiện khi Lambda Worker bị throttle do giới hạn concurrency — khiến bài nộp tích lũy trong queue mà không được chấm.

1. **Select metric** → **Lambda** → **By Function Name** → `codeexecute-worker` → **Throttles**.
2. Cấu hình:
   - **Statistic**: Sum
   - **Period**: 5 phút
   - **Threshold**: ≥ 1
   - **Datapoints to alarm**: 1 out of 1
3. **Notification**: Chọn SNS topic `codeexecute-alerts`.
4. Đặt tên alarm: `codeexecute-worker-throttles`.
5. Nhấn **Create alarm**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/alarm-throttle.jpg" alt="Cấu hình alarm Lambda throttle" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.8: Tạo throttle alarm cho codeexecute-worker với thông báo qua SNS</i>
</p>

</div>

---

### Bước 5: Tạo Alarm — Số Message Tồn Đọng Trong SQS

Alarm này kích hoạt khi bài nộp tích lũy trong SQS nhanh hơn Worker có thể xử lý, cho thấy Worker có thể đang gặp lỗi hoặc không đủ tài nguyên.

1. **Select metric** → **SQS** → chọn `codeexecute-submission-queue` → **ApproximateNumberOfMessagesVisible**.
2. Cấu hình:
   - **Statistic**: Maximum
   - **Period**: 5 phút
   - **Threshold**: ≥ 10 (điều chỉnh theo tải thực tế)
   - **Datapoints to alarm**: 2 out of 3
3. **Notification**: Chọn SNS topic `codeexecute-alerts`.
4. Đặt tên alarm: `codeexecute-queue-depth`.
5. Nhấn **Create alarm**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/alarm-sqs-depth.jpg" alt="Alarm số message tồn đọng SQS" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.9: Tạo alarm backlog SQS — cảnh báo khi tồn đọng ≥ 10 message</i>
</p>

</div>

---

### Bước 6: Xác Minh Alarm Và Kiểm Tra SNS

1. Trong **CloudWatch** → **Alarms**, xác nhận cả ba alarm đều ở trạng thái **OK**.

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/alarms-ok.jpg" alt="Tất cả CloudWatch alarms đang OK" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.10: Ba CloudWatch alarm đang Active và ở trạng thái OK</i>
</p>

</div>

2. Để xác minh luồng email SNS hoạt động đúng, gửi một message test thủ công đến topic:
   - Trong **SNS Console** → `codeexecute-alerts` → **Publish message**
   - **Subject**: `[CodExecute] Test Alert`
   - **Message**: `This is a test notification from codeexecute-alerts SNS topic.`
   - Nhấn **Publish message**

<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-test.jpg" alt="Gửi test message đến SNS topic" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.11: Gửi message test để xác minh email subscription SNS hoạt động đúng</i>
</p>

</div>

3. Xác nhận email test đến hộp thư từ địa chỉ `no-reply@sns.amazonaws.com`.
<div align="center">

<img src="/images/5-Workshop/5.9-SNS/sns-test-result.jpg" alt="Kết quả gửi test message đến SNS topic" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.9.12: Kết quả gửi test message để xác minh email subscription SNS hoạt động đúng</i>
</p>

</div>

---

### Kết Quả

Toàn bộ pipeline cảnh báo đã hoạt động:

| Alarm | Metric | Ngưỡng | Hành động SNS |
|---|---|---|---|
| `codeexecute-worker-errors` | Lambda `Errors` | ≥ 1 / phút | Email qua `codeexecute-alerts` |
| `codeexecute-worker-throttles` | Lambda `Throttles` | ≥ 1 / 5 phút | Email qua `codeexecute-alerts` |
| `codeexecute-queue-depth` | SQS `MessagesVisible` | ≥ 10 / 5 phút | Email qua `codeexecute-alerts` |

Bất kỳ sự cố nào trong pipeline chấm bài — Lambda crash, throttle do giới hạn concurrency, hay backlog bài nộp tích lũy — sẽ kích hoạt cảnh báo email tức thì đến toàn bộ subscriber đã xác nhận của topic `codeexecute-alerts`.
