---
title: "Tạo Amazon ECR & Build Docker Image"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

<!-- # TẠO AMAZON ECR REPOSITORIES VÀ BUILD DOCKER IMAGES -->

Trong phần này, chúng ta sẽ thực hiện khởi tạo hai **Amazon ECR Private Repositories**, sau đó đóng gói mã nguồn backend của ứng dụng **CodExecute** thành Docker Container Images và tải (push) lên ECR.

Hệ thống CodExecute có **2 dịch vụ container độc lập**:
- **`codexecute-lambda-worker`** — Chứa môi trường sandbox đa ngôn ngữ (Python 3.12, C++17, Java 17, Node.js) và handler xử lý các bài nộp bất đồng bộ từ SQS (build từ `Dockerfile.lambda`).
- **`codexecute-lambda-api`** — Chứa khung ứng dụng FastAPI REST API và handler thực thi code đồng bộ khi người dùng bấm nút RUN (build từ `Dockerfile.lambda_api`).

---

### Bước 1: Tạo Private ECR Repositories Trên AWS Console

1. Truy cập vào bảng điều khiển **Amazon ECR Console**.
2. Tại mục **Private registry**, chọn **Repositories** và nhấn **Create repository**.
3. Khởi tạo repository thứ nhất cho **Lambda Worker**:
   - **Visibility settings:** Chọn **Private**.
   - **Repository name:** Nhập `codexecute-lambda-worker`.
   - **Tag immutability:** Chọn **Mutable**.
   - **Encryption configuration:** Giữ mặc định (AES-256).
4. Nhấn **Create repository**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-repo-worker.jpg -->
<img src="/images/5-Workshop/5.4-be/ecr-repo-worker.jpg" alt="Tạo ECR Repository cho Worker" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.1.1: Tạo ECR Private Repository codexecute-lambda-worker</i>
</p>

</div>

5. Lặp lại thao tác trên để tạo repository thứ hai cho **Lambda API**:
   - **Repository name:** Nhập `codexecute-lambda-api`.
6. Nhấn **Create repository**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-repo-api.jpg -->
<img src="/images/5-Workshop/5.4-be/ecr-repo-api.jpg" alt="Tạo ECR Repository cho API" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.1.2: Tạo ECR Private Repository codexecute-lambda-api</i>
</p>

</div>

7. Kiểm tra danh sách repositories trên ECR Console để xác nhận cả 2 repository đã sẵn sàng.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-repos-list.jpg -->
<img src="/images/5-Workshop/5.4-be/ecr-repos-list.jpg" alt="Danh sách ECR Repositories" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.1.3: Xác nhận 2 ECR repositories đã được tạo thành công trên AWS Console</i>
</p>

</div>

---

### Bước 2: Đóng Gói Docker Image & Push Lên ECR

Mở Terminal tại thư mục `be/` của dự án CodExecute trên máy local. Đảm bảo dịch vụ **Docker Desktop** và **AWS CLI** đang hoạt động.

#### 1. Đẩy Image Cho Lambda Worker (`codexecute-lambda-worker`):

##### Bash / Linux / macOS:
```bash
cd be
chmod +x scripts/build_and_push_lambda.sh
./scripts/build_and_push_lambda.sh
```

##### PowerShell (Windows):
```powershell
cd be
.\scripts\build_and_push_lambda.ps1
```

Script sẽ tự động đăng nhập Docker vào ECR, đóng gói `Dockerfile.lambda` cho kiến trúc `linux/amd64`, gán nhãn `latest` và tải image lên ECR:

```bash
# Đăng nhập ECR
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com

# Build Container Image
docker buildx build --platform linux/amd64 --provenance=false --sbom=false --load -t codexecute-lambda-worker -f Dockerfile.lambda .

# Tag & Push lên ECR Repository
docker tag codexecute-lambda-worker:latest <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/codexecute-lambda-worker:latest
docker push <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/codexecute-lambda-worker:latest
```

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-worker-push.jpg -->
<img src="/images/5-Workshop/5.4-be/ecr-worker-push.jpg" alt="Push Image Worker lên ECR" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.1.4: Quá trình build và push Docker image Worker lên ECR Repository</i>
</p>

</div>

#### 2. Đẩy Image Cho Lambda API (`codexecute-lambda-api`):

##### Bash / Linux / macOS:
```bash
cd be
chmod +x scripts/build_and_push_lambda_api.sh
./scripts/build_and_push_lambda_api.sh
```

##### PowerShell (Windows):
```powershell
cd be
.\scripts\build_and_push_lambda_api.ps1
```

```bash
# Build Container Image từ Dockerfile.lambda_api
docker buildx build --platform linux/amd64 --provenance=false --sbom=false --load -t codexecute-lambda-api -f Dockerfile.lambda_api .

# Tag & Push lên ECR Repository
docker tag codexecute-lambda-api:latest <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/codexecute-lambda-api:latest
docker push <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/codexecute-lambda-api:latest
```

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-api-push.jpg -->
<img src="/images/5-Workshop/5.4-be/ecr-api-push.jpg" alt="Push Image API lên ECR" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.1.5: Quá trình build và push Docker image API lên ECR Repository</i>
</p>

</div>

---

### Bước 3: Xác Minh Tag & Digest Trên ECR Console

1. Truy cập vào từng repository `codexecute-lambda-worker` và `codexecute-lambda-api` trên **Amazon ECR Console**.
2. Xác nhận Tag `latest` và Image URI đã xuất hiện đầy đủ trong giao diện điều khiển.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-worker-updated.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-worker-updated.jpg" alt="Xác minh Image trên ECR Console" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.1.6: Xác nhận Docker image Worker được cập nhật thành công</i>
</p>

</div>

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-api-updated.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-api-updated.jpg" alt="Xác minh Image API trên ECR Console" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.1.7: Xác nhận Docker image API được cập nhật thành công</i>
</p>

</div>

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-worker-image-uri.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-worker-image-uri.jpg" alt="Xác minh Image URI Worker" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.1.8: Kiểm tra chi tiết ECR Image URI của Lambda Worker</i>
</p>

</div>

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: lambda-api-image-uri.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-api-image-uri.jpg" alt="Xác minh Image URI API" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Hình 5.4.1.9: Kiểm tra chi tiết ECR Image URI của Lambda API</i>
</p>

</div>

