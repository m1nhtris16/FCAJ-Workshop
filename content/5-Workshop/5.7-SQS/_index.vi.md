---
title: "Thiết lập Amazon SQS"
date: 2026-07-30
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---


Trong phần này, chúng ta ghi lại quá trình cấu hình **Amazon Simple Queue Service (SQS)** làm bộ đệm bất đồng bộ giữa **Lambda API** (`codeexecute-api`) và **Lambda Worker** (`codeexecute-worker`).

Khi người dùng bấm **SUBMIT**, Lambda API không thực thi code trực tiếp. Thay vào đó, nó đẩy một job chấm bài vào SQS queue, trả về kết quả ngay cho người dùng, và Lambda Worker sẽ được kích hoạt tự động để nhận và chấm bài bất đồng bộ.

```
Người dùng bấm SUBMIT
      │
      ▼
Lambda codeexecute-api
  ├─ Lưu Submission vào DynamoDB (Status: "Pending")
  └─ Đẩy job vào SQS Queue
              │
              ▼ (SQS Event Source Mapping)
      Lambda codeexecute-worker
        ├─ Đọc payload bài nộp từ SQS record
        ├─ Lấy testcases từ S3
        ├─ Thực thi code trong sandbox container
        └─ Cập nhật kết quả Submission vào DynamoDB
```

Payload SQS được API đẩy lên có dạng:
```json
{
  "submission_id": "...",
  "user_id": "...",
  "problem_id": "...",
  "language": "python|cpp|java|javascript",
  "code": "..."
}
```

#### Các bước thực hiện

<style>
#body a.toc-link, 
#body a.toc-link:hover, 
#body a.toc-link:focus, 
#body a.toc-link:active {
  border: 1px solid #e2e8f0 !important;
  border-bottom: 1px solid #e2e8f0 !important;
  text-decoration: none !important;
  background-image: none !important;
  box-shadow: 0 1px 3px rgba(0,0,0,0.04) !important;
}
#body a.toc-link:after, 
#body a.toc-link:before {
  display: none !important;
  content: "" !important;
}
#body a.toc-link:hover {
  border-color: #FF9900 !important;
  background-color: #FFFDF7 !important;
}
</style>

<div style="display: flex; flex-direction: column; gap: 10px; margin: 20px 0;">

  <a class="toc-link" href="5.7.1-create-queue/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">5.7.1</span>
      <span style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Tạo SQS Queue</span>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.7.2-event-source-mapping/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">5.7.2</span>
      <span style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Gắn SQS vào Lambda Worker (Event Source Mapping)</span>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

</div>
