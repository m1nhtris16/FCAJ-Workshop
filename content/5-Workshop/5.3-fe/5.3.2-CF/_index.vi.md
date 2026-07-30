---
title: "Cấu hình Amazon CloudFront & Multi-Origin Routing"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

<!-- # CẤU HÌNH AMAZON CLOUDFRONT DISTRIBUTION & MULTI-ORIGIN ROUTING -->

Trong phần này, chúng ta sẽ thực hiện cấu hình mạng phân phối nội dung **Amazon CloudFront Distribution** làm lớp đại diện (Reverse Proxy) duy nhất cho toàn bộ hệ thống **CodExecute**. 

Hạ tầng CloudFront sẽ định tuyến linh hoạt giữa **S3 Bucket** (chứa giao diện static web) và **Amazon API Gateway** (chạy dịch vụ backend APIs) dựa trên đường dẫn URL request (Path-based Routing).

---

### Bước 1: Khởi Tạo CloudFront Distribution Với Origin Ban Đầu Là Amazon S3

1. Truy cập vào bảng điều khiển **Amazon CloudFront Console** và chọn **Create distribution**.
2. Tại mục **Origin domain**, chọn S3 Bucket `codexecute-frontend.s3.amazonaws.com`.
3. Tại mục **Origin access**, chọn **Origin access control settings (recommended)** (OAC) để phân quyền kết nối an toàn với S3 riêng tư.
4. Tại mục **Default cache behavior**:
   - **Viewer protocol policy:** Chọn **Redirect HTTP to HTTPS**.
   - **Allowed HTTP methods:** Chọn **GET, HEAD**.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf2.jpg" alt="Cấu hình OAC và Cache Behavior" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.3.2.1: Cấu hình Origin Access Control (OAC) và Viewer Protocol Policy</i>
</p>

</div>

5. Nhấn **Create distribution** để khởi tạo phân phối CDN.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf3.jpg" alt="Khởi tạo Distribution thành công" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.3.2.2: Phân phối CloudFront Distribution được khởi tạo thành công</i>
</p>

</div>

---

### Bước 2: Thêm Origin Thứ Hai Là Amazon API Gateway

Để ứng dụng React Frontend có thể gọi các API Endpoint `/api/*` qua cùng một domain gốc mà không gặp lỗi CORS (Cross-Origin Resource Sharing), chúng ta thêm API Gateway làm Origin thứ 2:

1. Chọn CloudFront Distribution vừa tạo, chuyển sang tab **Origins** và nhấn **Create origin**.
2. Tại mục **Origin domain**, dán địa chỉ endpoint của API Gateway (ví dụ: `g0wll7768b.execute-api.ap-southeast-1.amazonaws.com`).

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf4.jpg" alt="Thêm API Gateway Origin" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.3.2.3: Thêm Origin thứ 2 trỏ tới Amazon API Gateway Endpoint</i>
</p>

</div>

3. Tại mục **Origin path**, nhập `/prod` (để CloudFront tự động định tuyến toàn bộ request vào đúng deployment stage `/prod` của API Gateway).
4. Tại mục **Protocol**, chọn **HTTPS only**.
5. Nhấn **Create origin** để lưu thiết lập.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf5.jpg" alt="Cấu hình Origin Path /prod" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.3.2.4: Điền Origin Path /prod và giao thức kết nối HTTPS Only</i>
</p>

</div>

---

### Bước 3: Cấu Hình Behavior Routing & Thiết Lập Thứ Tự Ưu Tiên (Priority)

Chuyển sang tab **Behaviors** để phân luồng truy cập giữa trang tĩnh Frontend và RESTful APIs Backend:

1. **Tạo Behavior Mới Cho REST APIs (`/api/*`):**
   - Nhấn **Create behavior**.
   - **Path pattern:** Nhập `/api/*`.
   - **Target origin:** Chọn Origin API Gateway vừa tạo.
   - **Allowed HTTP methods:** Chọn **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE** (cho phép đầy đủ các phương thức HTTP).
   - **Cache policy:** Chọn **CachingDisabled** (hoặc cấu hình truyền tải đầy đủ Query Strings và Headers để dữ liệu API luôn được cập nhật theo thời gian thực).
   - Nhấn **Create behavior**.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf6.jpg" alt="Tạo Behavior /api/*" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.3.2.5: Cấu hình Behavior cho đường dẫn /api/* trỏ về API Gateway</i>
</p>

</div>

2. **Kiểm Tra & Điều Chỉnh Thứ Tự Ưu Tiên (Priority):**
   Sau khi hoàn tất cấu hình, bảng danh sách Behaviors sẽ hiển thị thứ tự ưu tiên xử lý như sau:

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf7.jpg" alt="Danh sách thứ tự ưu tiên Behavior" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.3.2.6: Bảng sắp xếp thứ tự ưu tiên (Priority 0 cho /api/* và Priority 1 cho Default)</i>
</p>

</div>

| Priority (Độ ưu tiên) | Path Pattern | Target Origin | Chức năng |
| :---: | :---: | :--- | :--- |
| **0 (Cao nhất)** | `/api/*` | **API Gateway** (`/prod`) | Điều hướng các yêu cầu gọi backend API đến API Gateway stage `/prod`. |
| **1 (Default)** | `Default (/*)` | **Amazon S3** (`codexecute-frontend`) | Phân phối các tệp giao diện tĩnh (HTML/JS/CSS) từ S3 Bucket cho người dùng. |

---

### Kết Quả

Sau khi hoàn tất cấu hình Multi-Origin Routing trên CloudFront:
- Mọi yêu cầu truy cập giao diện tĩnh (ví dụ: `https://d1hsp5bm4hkjmb.cloudfront.net/`) sẽ được ưu tiên `1` tải từ **Amazon S3**.
- Mọi yêu cầu gọi dữ liệu API (ví dụ: `https://d1hsp5bm4hkjmb.cloudfront.net/api/v1/problems`) sẽ được ưu tiên `0` chuyển trực tiếp đến **Amazon API Gateway** tại stage `/prod`.
- Hệ thống hoạt động nhất quán trên một tên miền HTTPS duy nhất, giảm độ trễ truy cập toàn cầu và loại bỏ hoàn toàn rào cản CORS.
