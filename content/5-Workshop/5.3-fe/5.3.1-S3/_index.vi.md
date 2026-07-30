---
title: "Triển khai Frontend trên Amazon S3"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

<!-- # TRIỂN KHAI ỨNG DỤNG REACT FRONTEND TRÊN AMAZON S3 -->

Trong phần này, chúng ta sẽ thực hiện khởi tạo **Amazon S3 Bucket**, biên dịch ứng dụng **React (Vite) Frontend CodExecute** với biến môi trường `VITE_API_URL`, tải toàn bộ tệp sản phẩm (`dist`) lên S3, bật tính năng quản lý phiên bản (Bucket Versioning), tắt Static Website Hosting để đảm bảo an toàn bảo mật, và cấu hình **Bucket Policy** cấp quyền duy nhất cho **Amazon CloudFront** truy cập thông qua Origin Access Control (OAC).

---

### Bước 1: Tạo S3 Bucket & Tải (Upload) Các Tệp Trong Thư Mục `dist` Lên S3

1. Truy cập vào bảng điều khiển **Amazon S3 Console**.
2. Nhấn nút **Create bucket**, đặt tên cho Bucket (ví dụ: `codexecute-frontend`) và chọn AWS Region tương ứng.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/bucket-fe1.jpg" alt="Khởi tạo S3 Bucket" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.3.1: Nhập tên và chọn vùng khởi tạo S3 Bucket cho Frontend</i>
</p>

</div>

3. Cấu hình các thiết lập cơ bản cho Bucket và xác nhận khởi tạo.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/bucket-fe2.jpg" alt="Cấu hình khởi tạo Bucket" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.3.2: Xác nhận các thông số khởi tạo S3 Bucket</i>
</p>

</div>

<div align="center">

<img src="/images/5-Workshop/5.3-fe/bucket-fe3.jpg" alt="Tạo S3 Bucket thành công" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.3.3: Xác nhận các thông số khởi tạo S3 Bucket</i>
</p>

</div>

4. Mở Terminal tại thư mục `fe` của dự án CodExecute trên máy local. Đóng gói mã nguồn với biến môi trường `VITE_API_URL=https://d1hsp5bm4hkjmb.cloudfront.net` và upload thư mục `dist` lên S3:

#### Bash / Linux / macOS:
```bash
cd fe

# Thiết lập biến môi trường API URL
export VITE_API_URL=https://d1hsp5bm4hkjmb.cloudfront.net

# Biên dịch dự án React Vite
pnpm build

# Tải các tệp trong thư mục dist lên S3 Bucket
aws s3 sync dist/ s3://codexecute-frontend --delete
```

#### PowerShell (Windows):
```powershell
cd fe

# Thiết lập biến môi trường API URL trong PowerShell
$env:VITE_API_URL="https://d1hsp5bm4hkjmb.cloudfront.net"

# Biên dịch dự án React Vite
pnpm build

# Tải các tệp trong thư mục dist lên S3 Bucket
aws s3 sync dist/ s3://codexecute-frontend --delete
```

---

### Bước 2: Bật Bucket Versioning & Tắt Static Website Hosting Để Bảo Mật

1. Tại tab **Properties** của S3 Bucket, cuộn xuống mục **Bucket Versioning** và nhấn **Edit**. Chọn **Enable** để bật tính năng lưu trữ nhiều phiên bản. Việc này giúp lưu trữ lịch sử các bản build và hỗ trợ khôi phục (rollback) lại các bản build cũ khi cần thiết.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/bucket-fe7.jpg" alt="Bật Bucket Versioning" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.3.4: Bật tính năng Bucket Versioning để hỗ trợ khôi phục (rollback) bản build cũ</i>
</p>

</div>

2. Cuộn xuống mục **Static website hosting** và đảm bảo chọn **Disable** (Tắt). Việc tắt truy cập Static Website công khai giúp tăng cường bảo mật tuyệt đối cho hạ tầng, buộc toàn bộ lưu lượng truy cập phải đi qua dịch vụ **Amazon CloudFront** với chứng chỉ HTTPS an toàn.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/bucket-fe8.jpg" alt="Tắt Static Website Hosting" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.3.5: Tắt tính năng Static Website Hosting để bảo mật dữ liệu S3 Bucket</i>
</p>

</div>

---

### Bước 3: Cấu Hình Bucket Policy Cho Phép CloudFront Truy Cập (Origin Access Control)

Để Amazon CloudFront có thể đọc dữ liệu tĩnh từ S3 Bucket riêng tư mà không cần mở công khai cho toàn Internet, chúng ta gán chính sách **Bucket Policy** sau:

1. Chuyển sang tab **Permissions** của S3 Bucket, cuộn xuống mục **Bucket policy** và nhấn **Edit**.
2. Dán đoạn mã JSON Bucket Policy phân quyền riêng cho CloudFront Distribution (`E14SU7QS7NEEO8`):

```json
{
    "Version": "2008-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Sid": "AllowCloudFrontServicePrincipal",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::codexecute-frontend/*",
            "Condition": {
                "ArnLike": {
                    "AWS:SourceArn": "arn:aws:cloudfront::014936669466:distribution/E14SU7QS7NEEO8"
                }
            }
        }
    ]
}
```

3. Nhấn **Save changes** để lưu chính sách bảo mật.

---

### Kết Quả

Đến bước này, S3 Bucket `codexecute-frontend` đã được bảo vệ an toàn, tắt truy cập công khai trực tiếp và chỉ cho phép duy nhất dịch vụ **Amazon CloudFront** đọc dữ liệu để phân phối tới người dùng tại địa chỉ website: [https://d1hsp5bm4hkjmb.cloudfront.net](https://d1hsp5bm4hkjmb.cloudfront.net).
