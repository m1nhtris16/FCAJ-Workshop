---
title: "Gắn SQS vào Lambda Worker"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---



Trong bước này, chúng ta cấu hình **SQS Event Source Mapping** kết nối `codeexecute-submission-queue` với Lambda function `codeexecute-worker`. Mapping này khiến Lambda tự động polling SQS queue và kích hoạt Worker với một batch message mỗi khi có bài nộp mới vào queue.

---

### Bước 1: Thêm SQS Trigger Vào Lambda Worker

1. Mở **AWS Lambda Console** → chọn function `codeexecute-worker`.
2. Tại panel **Function overview**, nhấn **Add trigger**.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/esm-add-trigger.jpg" alt="Nút Add trigger trên Lambda Worker" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.7.6: Nhấn Add trigger trên trang tổng quan Lambda codeexecute-worker</i>
</p>

</div>

3. Tại panel **Add trigger**:
   - **Trigger source**: Chọn **SQS**
   - **SQS queue**: Chọn `codeexecute-submission-queue`
   - **Batch size**: `1`
   - **Batch window**: `0` giây
   - **Enabled**: ✅ Bật

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/esm-config.jpg" alt="Cấu hình SQS Event Source Mapping" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.7.7: Cấu hình SQS trigger với batch size = 1</i>
</p>

</div>

> **Tại sao Batch Size = 1?**  
> Mỗi lần Lambda được kích hoạt chỉ xử lý đúng một bài nộp. Chạy nhiều bài trong một lần invocation sẽ gây xung đột tài nguyên bên trong sandbox container (filesystem `/tmp`, các tiến trình compiler). Batch size = 1 đảm bảo mỗi bài được cô lập hoàn toàn.

4. Nhấn **Add**.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/esm-created.jpg" alt="Đã thêm SQS trigger vào Lambda Worker" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.7.8: SQS trigger đã được kết nối thành công với Lambda codeexecute-worker</i>
</p>

</div>

---

### Bước 2: Xác Nhận Event Source Mapping

Sau khi trigger được thêm, **Function overview** sẽ hiển thị `codeexecute-submission-queue` là trigger source đang Active của function `codeexecute-worker`.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/esm-active.jpg" alt="SQS trigger Active trong Lambda function overview" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.7.9: SQS Event Source Mapping Active trong trang tổng quan Lambda Worker</i>
</p>

</div>

Chuyển sang **Configuration** → **Triggers** để xác nhận chi tiết:

| Trường | Giá trị |
|---|---|
| **Source** | `codeexecute-submission-queue` |
| **Batch size** | 1 |
| **State** | Enabled |

---

### Bước 3: Kiểm Tra Luồng Chấm Bài End-to-End

Với SQS và Event Source Mapping đã Active, thử submit một bài giải từ frontend CodExecute:

1. Truy cập một đề bài tại `https://d1hsp5bm4hkjmb.cloudfront.net`.
2. Viết lời giải và nhấn **SUBMIT**.
3. Lambda API trả về ngay `SubmissionID` với trạng thái `Pending`.
4. Theo dõi **SQS Console** → chọn `codeexecute-submission-queue` → **Send and receive messages** → **Poll for messages** để quan sát message được tiêu thụ.

<div align="center">

<img src="/images/5-Workshop/5.7-SQS/sqs-monitor.jpg" alt="Theo dõi SQS queue trong quá trình submit" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.7.10: Theo dõi quá trình message được đẩy vào và tiêu thụ khỏi SQS trong khi submit bài</i>
</p>

</div>

---

### Kết Quả

Toàn bộ luồng chấm bài bất đồng bộ đã hoạt động:

| Thành phần | Vai trò |
|---|---|
| **Lambda `codeexecute-api`** | Đẩy `{"submission_id", "problem_id", "language", "code"}` vào SQS mỗi khi SUBMIT |
| **`codeexecute-submission-queue`** | Lưu trữ và phân phối job với visibility timeout = 300 giây |
| **Event Source Mapping** | Polling SQS và kích hoạt `codeexecute-worker` với batch size = 1 |
| **Lambda `codeexecute-worker`** | Lấy message, thực thi code trong sandbox, ghi kết quả vào DynamoDB |

