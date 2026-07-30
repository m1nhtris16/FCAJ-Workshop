---
title: "Create Amazon ECR & Build Docker Images"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

<!-- # CREATING AMAZON ECR REPOSITORIES & BUILDING DOCKER IMAGES -->

In this section, we will create two **Amazon ECR Private Repositories**, compile the **CodExecute** backend source code into Docker Container Images, and push them to ECR.

The CodExecute system comprises **2 decoupled container services**:
- **`codexecute-lambda-worker`** — Contains the multi-language execution sandbox environment (Python 3.12, C++17, Java 17, Node.js) and the handler for processing asynchronous code submissions from SQS (built from `Dockerfile.lambda`).
- **`codexecute-lambda-api`** — Contains the FastAPI REST API framework and handler for synchronous code execution when users click the RUN button (built from `Dockerfile.lambda_api`).

---

### Step 1: Create Private ECR Repositories on AWS Console

1. Open the **Amazon ECR Console**.
2. Under **Private registry**, select **Repositories** and click **Create repository**.
3. Create the first repository for **Lambda Worker**:
   - **Visibility settings:** Select **Private**.
   - **Repository name:** Enter `codexecute-lambda-worker`.
   - **Tag immutability:** Select **Mutable**.
   - **Encryption configuration:** Keep default (AES-256).
4. Click **Create repository**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-repo-worker.jpg -->
<img src="/images/5-Workshop/5.4-be/ecr-repo-worker.jpg" alt="Create ECR Repository for Worker" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.1.1: Creating Private ECR Repository codexecute-lambda-worker</i>
</p>

</div>

5. Repeat the process to create the second repository for **Lambda API**:
   - **Repository name:** Enter `codexecute-lambda-api`.
6. Click **Create repository**.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-repos-list.jpg -->
<img src="/images/5-Workshop/5.4-be/ecr-repo-api.jpg" alt="Create ECR Repository for API" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.1.2: Creating Private ECR Repository codexecute-lambda-api</i>
</p>

</div>

7. Verify the repository list on the ECR Console to confirm both repositories are ready.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-list-repos.jpg -->
<img src="/images/5-Workshop/5.4-be/ecr-repos-list.jpg" alt="ECR Repositories List" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.1.3: Confirming two ECR repositories created successfully on AWS Console</i>
</p>

</div>

---

### Step 2: Build Docker Images & Push to ECR

Open your terminal in the `be/` directory of the CodExecute project on your local machine. Ensure **Docker Desktop** and **AWS CLI** are running.

#### 1. Push Image for Lambda Worker (`codexecute-lambda-worker`):

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

The script automatically authenticates Docker with ECR, builds `Dockerfile.lambda` targeting `linux/amd64`, tags it as `latest`, and pushes the image:

```bash
# ECR Authentication
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com

# Build Container Image
docker buildx build --platform linux/amd64 --provenance=false --sbom=false --load -t codexecute-lambda-worker -f Dockerfile.lambda .

# Tag & Push to ECR Repository
docker tag codexecute-lambda-worker:latest <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/codexecute-lambda-worker:latest
docker push <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/codexecute-lambda-worker:latest
```

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: push-worker-ecr.jpg -->
<img src="/images/5-Workshop/5.4-be/ecr-worker-push.jpg" alt="Push Worker Image to ECR" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.1.4: Building and pushing Worker Docker image to ECR Repository</i>
</p>

</div>

#### 2. Push Image for Lambda API (`codexecute-lambda-api`):

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
# Build Container Image from Dockerfile.lambda_api
docker buildx build --platform linux/amd64 --provenance=false --sbom=false --load -t codexecute-lambda-api -f Dockerfile.lambda_api .

# Tag & Push to ECR Repository
docker tag codexecute-lambda-api:latest <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/codexecute-lambda-api:latest
docker push <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/codexecute-lambda-api:latest
```

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: push-api-ecr.jpg -->
<img src="/images/5-Workshop/5.4-be/ecr-api-push.jpg" alt="Push API Image to ECR" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.1.5: Building and pushing API Docker image to ECR Repository</i>
</p>

</div>

---

### Step 3: Verify Tags & Digest on ECR Console

1. Navigate to both `codexecute-lambda-worker` and `codexecute-lambda-api` repositories on **Amazon ECR Console**.
2. Confirm that Tag `latest` and Image URI are properly listed.

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-verify-images.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-worker-updated.jpg" alt="Verify Images on ECR Console" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.1.6: Confirming Docker Images pushed successfully on ECR Console</i>
</p>

</div>

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-verify-images.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-api-updated.jpg" alt="Verify Images on ECR Console" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.1.7: Confirming Docker Images pushed successfully on ECR Console</i>
</p>

</div>

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-verify-images.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-worker-image-uri.jpg" alt="Verify Images on ECR Console" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.1.8: Confirming Docker Images pushed successfully on ECR Console</i>
</p>

</div>

<div align="center">

<!-- PLACEHOLDER FOR IMAGE: ecr-verify-images.jpg -->
<img src="/images/5-Workshop/5.4-be/lambda-api-image-uri.jpg" alt="Verify Images on ECR Console" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.4.1.9: Confirming Docker Images pushed successfully on ECR Console</i>
</p>

</div>