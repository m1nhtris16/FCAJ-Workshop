---
title: "Cấu hình API Gateway cho Lambda API"
date: 2026-07-30
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

<!-- # CẤU HÌNH AMAZON API GATEWAY LÀM TRIGGER CHO LAMBDA API (`/{proxy+}`) -->

Trong phần này, chúng ta sẽ thực hiện tạo một **HTTP API Gateway**, cấu hình **Lambda Proxy Integration** với tuyến đường (route) đại diện `ANY /{proxy+}` trỏ trực tiếp đến hàm **AWS Lambda API Handler** (`codeexecute-api`), và tiến hành cấu hình stage `/prod` với tính năng **Auto-deploy** để tiếp nhận các yêu cầu RESTful API từ giao diện người dùng.

---

### Tổng Quan Về Lambda Proxy Integration

Trong hạ tầng Serverless của **CodExecute**, khung ứng dụng **FastAPI** backend được đóng gói và thực thi trực tiếp bên trong hàm AWS Lambda (`codeexecute-api`). 

Để API Gateway có thể chuyển tiếp toàn bộ lưu lượng HTTP request (đường dẫn URL, HTTP Method, Query Parameters, Headers, Request Body) trực tiếp cho ứng dụng FastAPI xử lý mà không cần định nghĩa thủ công từng endpoint riêng biệt, chúng ta áp dụng cơ chế **Lambda Proxy Integration** với đường dẫn Wildcard `/{proxy+}` và phương thức `ANY`.

---

### Bước 1: Khởi Tạo HTTP API Gateway

1. Truy cập vào bảng điều khiển **Amazon API Gateway Console**.
2. Tại màn hình chính, tìm mục **HTTP API** và nhấn nút **Build**.

<div align="center">

<img src="/images/5-Workshop/5.6-APIGateway/ag1.jpg" alt="Chọn HTTP API Type" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.6.1: Lựa chọn loại API Gateway là HTTP API</i>
</p>

</div>

---

### Bước 2: Cấu Hình Stage (`prod` - Auto-deploy: ON)

1. Thiết lập thông tin Stage cho API:
   - **Stage name:** Nhập `prod`.
   - **Auto-deploy:** Bật công tắc chọn **ON** để API Gateway tự động triển khai những thay đổi về route mới ngay lập tức mà không cần bấm Deploy thủ công.

<div align="center">

<img src="/images/5-Workshop/5.6-APIGateway/ag2.jpg" alt="Cấu hình Stage prod" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.6.2: Cấu hình Stage name prod và kích hoạt tính năng Auto-deploy ON</i>
</p>

</div>

---

### Bước 3: Xem Lại Thông Số & Khởi Tạo (Review and Create)

1. Kiểm tra lại toàn bộ thông tin cấu hình API, Integrations và Stage.
2. Nhấn nút **Create** để tiến hành khởi tạo HTTP API Gateway.

<div align="center">

<img src="/images/5-Workshop/5.6-APIGateway/ag3.jpg" alt="Review và Tạo API Gateway" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.6.3: Kiểm tra thông tin cấu hình và chọn Create để tạo API Gateway</i>
</p>

</div>

---

### Bước 4: Tạo Tuyến Đường Route `ANY /{proxy+}` Trỏ Đến Lambda API

1. Truy cập vào mục **Routes** của API Gateway vừa tạo.
2. Nhấn **Create** để thêm tuyến đường mới:
   - **Method:** Chọn **ANY**.
   - **Path:** Nhập `/{proxy+}`.
3. Chọn Integration trỏ trực tiếp đến hàm **AWS Lambda API Handler** (`codeexecute-api`).
4. Nhấn **Create** để lưu tuyến đường.

<div align="center">

<img src="/images/5-Workshop/5.6-APIGateway/ag4.jpg" alt="Tạo route ANY /{proxy+}" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.6.4: Cấu hình tuyến đường ANY /{proxy+} kết nối tới hàm Lambda API</i>
</p>

</div>

---

### Bước 5: Kiểm Tra Trạng Thái Sẵn Sàng Của API Gateway (Availability Test)

Để kiểm tra cổng API Gateway đã kết nối thành công và truyền nhận dữ liệu thông suốt tới hàm Lambda API Backend (`codeexecute-api`), ta thực hiện kiểm thử endpoint bằng công cụ `curl`:

1. **Lấy Invoke URL của API Gateway:**
   - Trong giao diện API Gateway Console → chọn API `codeexecute-api-gateway` → vào mục **Stages** → chọn stage `prod`.
   - Sao chép đường dẫn **Invoke URL** (ví dụ: `https://3y6w9810cb.execute-api.ap-southeast-1.amazonaws.com/prod`).

2. **Chạy lệnh `curl` để kiểm tra Endpoint Health Check / API Docs:**

```bash
# Kiểm tra endpoint OpenAPI docs hoặc health endpoint của FastAPI
curl -i https://3y6w9810cb.execute-api.ap-southeast-1.amazonaws.com/prod/docs
```

3. **Kết quả kỳ vọng (Expected Output):**
   - Lệnh `curl` trả về mã phản hồi HTTP `200 OK`.
   - Header cho thấy phản hồi từ FastAPI Serverless backend:

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: curl-test-apigateway.jpg -->
<img src="/images/5-Workshop/5.6-APIGateway/curl-test-apigateway.jpg" alt="Kiểm tra kết quả cURL thành công trên Terminal" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.6.5: Kết quả thực thi lệnh cURL kiểm tra API Gateway thành công trong Terminal</i>
</p>

</div>

Mã phản hồi HTTP `200 OK` kèm theo dữ liệu JSON khẳng định API Gateway đã hoạt động ổn định và sẵn sàng tiếp nhận request từ hệ thống Frontend.

---

### Kết Quả

Đến bước này, cổng kết nối **Amazon API Gateway** đã được thiết lập và kiểm thử thành công làm Trigger đại diện cho hàm Lambda `codeexecute-api`. Mọi yêu cầu HTTP gửi đến đường dẫn `/prod/api/*` sẽ được API Gateway tự động đóng gói theo chuẩn Proxy Payload và chuyển tiếp tới hàm Lambda để thực thi logic ứng dụng FastAPI.