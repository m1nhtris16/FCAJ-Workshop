---
title: "Prerequisites"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

To prepare for this workshop, you need to configure your local development environment, set up AWS CLI credentials, and provision infrastructure resources via CloudFormation.

---

### 1. Local Development Environment

Ensure your personal workstation is equipped with the following tools and command-line interfaces:

1. **AWS CLI (Command Line Interface):**
   - Command-line tool to interact with AWS services.
   - **Account Authentication:** Authenticate in your local environment using an AWS account with **AdministratorAccess** permissions via an **Access Key ID** and **Secret Access Key**.
   - Verify and configure using:
     ```bash
     aws configure
     # AWS Access Key ID: <YOUR_ACCESS_KEY>
     # AWS Secret Access Key: <YOUR_SECRET_KEY>
     # Default region name: ap-southeast-1
     # Default output format: json
     ```

2. **Git CLI:**
   - Version control tool for cloning project repositories, storing automation scripts, and CloudFormation templates.
   - Verify with: `git --version`

3. **Python (v3.x):**
   - Runtime environment for automation scripts, API testing, and running the AWS SDK (`boto3`).
   - Verify with: `python --version` or `python3 --version`

4. **Node.js & npm + pnpm:**
   - JavaScript runtime and package managers for executing CLI tools, AWS CDK, or web applications.
   - Verify with: `node -v`, `npm -v`, `pnpm -v`

5. **Docker Desktop:**
   - Container virtualization engine on your local machine for packaging applications, testing containerized microservices, or local service emulation before cloud deployment.
   - Verify with: `docker --version`

---

### 2. IAM Roles Configuration for Services (Principle of Least Privilege)

To ensure secure operations for the **CodExecute** platform following the **Principle of Least Privilege**, each Serverless component is assigned a dedicated **IAM Execution Role** with tightly scoped permission policies aligned strictly to its operational requirements:

---

#### 2.1. IAM Role for Lambda API Handler (`CodExecuteAPILambdaRole`)
- **Attached Service:** Lambda Function `codeexecute-api` (FastAPI REST Server).
- **Service Principal (Trust Policy):** `lambda.amazonaws.com`
- **Granted Permissions:**
  1. **CloudWatch Logs:** Allows writing application logs (`logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`).
  2. **Amazon DynamoDB:** Allows CRUD access to users, problemsets, and submission records across system DynamoDB tables (`dynamodb:GetItem`, `dynamodb:PutItem`, `dynamodb:UpdateItem`, `dynamodb:DeleteItem`, `dynamodb:Query`, `dynamodb:Scan`).
  3. **Amazon S3:** Allows read/write access to `codeexecute-user-media/*` and read-only access to `codeexecute-testcases/*`.
  4. **Amazon SQS:** Allows dispatching submission tasks into the asynchronous queue `codexecute-submissions-queue` (`sqs:SendMessage`).

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

#### 2.2. IAM Role for Lambda Sandbox Worker (`CodExecuteWorkerLambdaRole`)
- **Attached Service:** Lambda Function `codeexecute-worker` (Async Code Evaluator Sandbox).
- **Service Principal (Trust Policy):** `lambda.amazonaws.com`
- **Granted Permissions:**
  1. **CloudWatch Logs:** Allows logging compilation outputs and sandbox execution logs (`logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`).
  2. **Amazon SQS:** Allows polling and deleting submission jobs from SQS Queue (`sqs:ReceiveMessage`, `sqs:DeleteMessage`, `sqs:GetQueueAttributes`).
  3. **Amazon S3:** Read-only access to testcase datasets `codeexecute-testcases/*` (`s3:GetObject`).
  4. **Amazon DynamoDB:** Reads problemset/submission metadata and updates evaluation verdicts (Status, Execution Time, Memory, Output) on `Submissions` and `TestCases` tables (`dynamodb:GetItem`, `dynamodb:UpdateItem`).

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

#### 2.3. IAM Role for API Gateway CloudWatch Logging (`CodExecuteAPIGatewayRole`)
- **Attached Service:** Amazon API Gateway.
- **Service Principal (Trust Policy):** `apigateway.amazonaws.com`
- **Granted Permissions:** Logs API access traffic and preflight CORS errors to CloudWatch (`logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:DescribeLogGroups`, `logs:PutLogEvents`).

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

### 3. Repository Setup & Local Development Execution

Before proceeding with AWS Cloud deployment labs, clone the primary **CodExecute** repository and run the local development environment:

#### Step 3.1: Clone Project Repository

Open your terminal and execute the Git clone command:

```bash
git clone https://github.com/phuvi301/CodExecute
cd CodExecute
```

#### Step 3.2: Launch Local Frontend (React + Vite)

1. Navigate to the `fe` directory and install project dependencies using `pnpm`:
   ```bash
   cd fe
   pnpm install
   ```

2. Start the local development server:
   ```bash
   pnpm run dev
   ```

   The Frontend application will run locally at: `http://localhost:3000`.
   

#### Step 3.3: Launch Local Backend API (FastAPI)

1. Navigate to the `be` directory and create a Python virtual environment:
   
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

2. Install backend dependencies and launch the FastAPI server:
   ```bash
   pip install -r requirements.txt
   fastapi dev
   ```
   The Backend REST API will start and accept incoming requests.

---

### 4. Overview of Provisioned AWS Serverless Resources

Summary of Serverless infrastructure components provisioned throughout this workshop:

| Infrastructure Component | AWS Resource Name | Role & Responsibility |
| :--- | :--- | :--- |
| **Amazon S3 (3 Buckets)** | `codexecute-prod-frontend`<br>`codeexecute-testcases`<br>`codeexecute-user-media` | Stores static web assets, testcase datasets (Input/Output), and user media. |
| **Amazon DynamoDB (8 Tables)** | `Users`, `Problems`, `Submissions`, `TestCases`, `Posts`, `Notifications`, `UserFollows`, `Solutions` | Stores user profiles, problemsets, submission logs, social posts, and solution code. |
| **Amazon SQS (Queue)** | `codexecute-submissions-queue` | Asynchronous submission buffer queue absorbing traffic spikes. |
| **AWS Lambda (2 Functions)** | `codeexecute-api`<br>`codeexecute-worker` | - `codeexecute-api`: REST API backend server (FastAPI).<br>- `codeexecute-worker`: Multi-language code evaluation sandbox. |
| **Amazon API Gateway** | `codeexecute-api-gateway` | Unified HTTP API Gateway entrypoint. |
| **Amazon CloudFront** | CDN Distribution | Global edge content delivery network and HTTPS Reverse Proxy. |

After configuring local environments and verifying `aws configure`, you are ready to begin hands-on Cloud deployment for **CodExecute**!