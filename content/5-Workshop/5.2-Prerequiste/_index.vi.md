---
title: "Các bước chuẩn bị"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

Để sẵn sàng thực hành bài workshop này, bạn cần thiết lập môi trường phát triển tại máy local (Local Development Environment), cấu hình tài khoản AWS CLI và khởi tạo cụm hạ tầng bằng CloudFormation.

---

### 1. Môi Trường Phát Triển Local (Local Development Environment)

Đảm bảo máy tính cá nhân của bạn đã được cài đặt đầy đủ các công cụ phát triển và công cụ dòng lệnh sau:

1. **AWS CLI (Command Line Interface):**
   - Công cụ dòng lệnh giao tiếp với AWS API.
   - **Xác thực tài khoản:** Đăng nhập và cấu hình ở môi trường local bằng tài khoản AWS có quyền Admin (**AdministratorAccess**) thông qua cặp **Access Key ID** và **Secret Access Key**.
   - Kiểm tra và cấu hình bằng lệnh:
     ```bash
     aws configure
     # AWS Access Key ID: <YOUR_ACCESS_KEY>
     # AWS Secret Access Key: <YOUR_SECRET_KEY>
     # Default region name: ap-southeast-1
     # Default output format: json
     ```

2. **Git CLI:**
   - Công cụ quản lý mã nguồn phiên bản dùng để clone các repository dự án, lưu trữ script và CloudFormation templates.
   - Kiểm tra bằng lệnh: `git --version`

3. **Python (v3.x):**
   - Môi trường thực thi script tự động hóa, kiểm thử API và chạy AWS SDK (`boto3`).
   - Kiểm tra bằng lệnh: `python --version` hoặc `python3 --version`

4. **Node.js & npm + pnpm:**
   - Môi trường JavaScript runtime và các trình quản lý gói (Package Managers) phục vụ việc chạy công cụ CLI, CDK hoặc các ứng dụng web.
   - Kiểm tra bằng lệnh: `node -v`, `npm -v`, `pnpm -v`

5. **Docker Desktop:**
   - Môi trường ảo hóa container tại máy local giúp đóng gói ứng dụng, chạy thử nghiệm các dịch vụ hoặc mô phỏng môi trường trước khi deploy lên Cloud.
   - Kiểm tra bằng lệnh: `docker --version`

---

### 2. Cấu Hình IAM Roles Cho Các Dịch Vụ (Principle of Least Privilege)

Để đảm bảo hệ thống **CodExecute** vận hành an toàn và bảo mật theo nguyên tắc **Least Privilege** (Quyền tối thiểu), mỗi dịch vụ Serverless được gán một **IAM Execution Role** riêng biệt với các chính sách phân quyền thu hẹp chính xác theo đúng nhu cầu hoạt động:

---

#### 2.1. IAM Role Cho Lambda API Handler (`CodExecuteAPILambdaRole`)
- **Dịch vụ gán Role:** Hàm Lambda `codeexecute-api` (FastAPI REST Server).
- **Service Principal (Trust Policy):** `lambda.amazonaws.com`
- **Quyền hạn được cấp (Permissions):**
  1. **CloudWatch Logs:** Cho phép ghi nhật ký ứng dụng (`logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`).
  2. **Amazon DynamoDB:** Cho phép đọc/ghi dữ liệu người dùng, bài tập, bài nộp trên các bảng DynamoDB hệ thống (`dynamodb:GetItem`, `dynamodb:PutItem`, `dynamodb:UpdateItem`, `dynamodb:DeleteItem`, `dynamodb:Query`, `dynamodb:Scan`).
  3. **Amazon S3:** Cho phép đọc/ghi dữ liệu media trên `codeexecute-user-media/*` và đọc bộ testcase trên `codeexecute-testcases/*`.
  4. **Amazon SQS:** Cho phép đẩy bài nộp vào hàng chờ bất đồng bộ `codexecute-submissions-queue` (`sqs:SendMessage`).

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "CloudWatchLogging",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:ap-southeast-1:*:log-group:/aws/lambda/codeexecute-api:*"
        },
        {
            "Sid": "DynamoDBAccess",
            "Effect": "Allow",
            "Action": [
                "dynamodb:GetItem",
                "dynamodb:PutItem",
                "dynamodb:UpdateItem",
                "dynamodb:DeleteItem",
                "dynamodb:Query",
                "dynamodb:Scan"
            ],
            "Resource": "arn:aws:dynamodb:ap-southeast-1:*:table/*"
        },
        {
            "Sid": "S3BucketAccess",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::codeexecute-user-media/*",
                "arn:aws:s3:::codeexecute-testcases/*"
            ]
        },
        {
            "Sid": "SQSSendMessage",
            "Effect": "Allow",
            "Action": "sqs:SendMessage",
            "Resource": "arn:aws:sqs:ap-southeast-1:*:codexecute-submissions-queue"
        }
    ]
}
```

---

#### 2.2. IAM Role Cho Lambda Sandbox Worker (`CodExecuteWorkerLambdaRole`)
- **Dịch vụ gán Role:** Hàm Lambda `codeexecute-worker` (Async Code Evaluator Sandbox).
- **Service Principal (Trust Policy):** `lambda.amazonaws.com`
- **Quyền hạn được cấp (Permissions):**
  1. **CloudWatch Logs:** Cho phép ghi nhật ký quá trình biên dịch và thực thi mã nguồn (`logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`).
  2. **Amazon SQS:** Cho phép nhận và xóa bài nộp từ SQS Queue (`sqs:ReceiveMessage`, `sqs:DeleteMessage`, `sqs:GetQueueAttributes`).
  3. **Amazon S3:** Chỉ đọc (Read-only) bộ dữ liệu testcase `codeexecute-testcases/*` (`s3:GetObject`).
  4. **Amazon DynamoDB:** Đọc dữ liệu bài tập/bài nộp và cập nhật kết quả thực thi (Status, Execution Time, Memory, Output) trên bảng `Submissions` và `TestCases` (`dynamodb:GetItem`, `dynamodb:UpdateItem`).

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "CloudWatchLogging",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:ap-southeast-1:*:log-group:/aws/lambda/codeexecute-worker:*"
        },
        {
            "Sid": "SQSPollingAccess",
            "Effect": "Allow",
            "Action": [
                "sqs:ReceiveMessage",
                "sqs:DeleteMessage",
                "sqs:GetQueueAttributes"
            ],
            "Resource": "arn:aws:sqs:ap-southeast-1:*:codexecute-submissions-queue"
        },
        {
            "Sid": "S3TestcasesReadAccess",
            "Effect": "Allow",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::codeexecute-testcases/*"
        },
        {
            "Sid": "DynamoDBSubmissionsUpdate",
            "Effect": "Allow",
            "Action": [
                "dynamodb:GetItem",
                "dynamodb:UpdateItem",
                "dynamodb:PutItem"
            ],
            "Resource": [
                "arn:aws:dynamodb:ap-southeast-1:*:table/Submissions",
                "arn:aws:dynamodb:ap-southeast-1:*:table/TestCases"
            ]
        }
    ]
}
```

---

#### 2.3. IAM Role Cho API Gateway CloudWatch Logging (`CodExecuteAPIGatewayRole`)
- **Dịch vụ gán Role:** Amazon API Gateway.
- **Service Principal (Trust Policy):** `apigateway.amazonaws.com`
- **Quyền hạn được cấp (Permissions):** Ghi log truy cập và lỗi HTTP preflight requests vào CloudWatch (`logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:DescribeLogGroups`, `logs:PutLogEvents`).

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "APIGatewayCloudWatchLogs",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DescribeLogGroups",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:ap-southeast-1:*:*"
        }
    ]
}
```

---

### 3. Tải Mã Nguồn Dự Án & Thiết Lập Môi Trường Local

Trước khi bước vào các bài thực hành triển khai lên AWS Cloud, hãy thực hiện clone repository mã nguồn chính **CodExecute** và khởi chạy thử nghiệm trên máy local:

#### Bước 3.1: Clone Mã Nguồn Dự Án

Mở Terminal và chạy lệnh Git clone:

```bash
git clone https://github.com/phuvi301/CodExecute
cd CodExecute
```

#### Bước 3.2: Khởi Chạy Frontend (React + Vite) tại Local

1. Di chuyển vào thư mục `fe` và cài đặt các phụ thuộc (dependencies) bằng `pnpm`:
   ```bash
   cd fe
   pnpm install
   ```

2. Khởi chạy máy chủ phát triển (Development Server):
   ```bash
   pnpm run dev
   ```
   Ứng dụng Frontend sẽ sẵn sàng truy cập tại địa chỉ local: `http://localhost:3000`.

#### Bước 3.3: Khởi Chạy Backend API (FastAPI) tại Local

1. Di chuyển vào thư mục `be` và tạo môi trường ảo Python (Virtual Environment):
   
   ##### Linux / macOS:
   ```bash
   cd be
   python3 -m venv venv
   source venv/bin/activate
   ```

   ##### Windows (PowerShell):
   ```powershell
   cd be
   python -m venv venv
   .\venv\Scripts\activate
   ```

2. Cài đặt các thư viện cần thiết và chạy máy chủ FastAPI:
   ```bash
   pip install -r requirements.txt
   fastapi dev
   ```
   Backend REST API sẽ được khởi chạy và sẵn sàng tiếp nhận yêu cầu.

---

### 4. Tổng Quan Các Tài Nguyên AWS Serverless Sẽ Khởi Tạo

Bảng tổng hợp danh sách hạ tầng Serverless sẽ được tạo và vận hành trong suốt bài workshop:

| Thành Phần Hạ Tầng | Tên Tài Nguyên AWS | Vai Trò & Chức Năng |
| :--- | :--- | :--- |
| **Amazon S3 (3 Buckets)** | `codexecute-prod-frontend`<br>`codeexecute-testcases`<br>`codeexecute-user-media` | Lưu trữ file tĩnh Frontend, bộ dữ liệu Testcases (Input/Output) và media/avatar người dùng. |
| **Amazon DynamoDB (8 Tables)** | `Users`, `Problems`, `Submissions`, `TestCases`, `Posts`, `Notifications`, `UserFollows`, `Solutions` | Lưu trữ dữ liệu tài khoản, đề bài, lịch sử bài nộp, mạng xã hội và lời giải. |
| **Amazon SQS (Queue)** | `codexecute-submissions-queue` | Hàng chờ đệm bài nộp bất đồng bộ xử lý traffic tăng đột biến. |
| **AWS Lambda (2 Functions)** | `codeexecute-api`<br>`codeexecute-worker` | - `codeexecute-api`: REST API backend server (FastAPI).<br>- `codeexecute-worker`: Sandbox chấm bài đa ngôn ngữ. |
| **Amazon API Gateway** | `codeexecute-api-gateway` | Cổng HTTP API duy nhất tiếp nhận request từ client. |
| **Amazon CloudFront** | Distribution CDN | Mạng phân phối nội dung toàn cầu và Reverse Proxy HTTPS. |

Sau khi hoàn tất cài đặt môi trường local và xác thực `aws configure`, bạn đã sẵn sàng bước vào các nội dung triển khai thực tế tiếp theo của dự án **CodExecute**!