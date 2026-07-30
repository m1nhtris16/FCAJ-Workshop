---
title: "Deploy Frontend on Amazon S3"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

<!-- # DEPLOYING REACT FRONTEND APPLICATION ON AMAZON S3 -->

In this section, we will create an **Amazon S3 Bucket**, build the **CodExecute React (Vite) Frontend** application with the `VITE_API_URL` environment variable, upload all production build assets (`dist` folder) to S3, enable Bucket Versioning, disable Static Website Hosting to ensure security, and configure a **Bucket Policy** granting exclusive read permissions to **Amazon CloudFront** via Origin Access Control (OAC).

---

### Step 1: Create S3 Bucket & Upload Build Assets in `dist` Folder to S3

1. Access the **Amazon S3 Console**.
2. Click **Create bucket**, enter your Bucket Name (e.g., `codexecute-frontend`), and select your designated AWS Region.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/bucket-fe1.jpg" alt="Create S3 Bucket" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.3.1: Entering S3 Bucket name and region selection for Frontend</i>
</p>

</div>

3. Configure baseline bucket settings and confirm creation.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/bucket-fe2.jpg" alt="Configure S3 Bucket creation" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.3.2: Confirming S3 Bucket configuration settings</i>
</p>

</div>

<div align="center">

<img src="/images/5-Workshop/5.3-fe/bucket-fe3.jpg" alt="S3 Bucket created successfully" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.3.3: Confirming S3 Bucket configuration settings</i>
</p>

</div>

4. Open your terminal in the `fe` directory of the CodExecute codebase on your local machine. Package the source code with the `VITE_API_URL=https://d1hsp5bm4hkjmb.cloudfront.net` environment variable and upload the `dist` folder to S3:

#### Bash / Linux / macOS:
```bash
cd fe

# Set API URL environment variable
export VITE_API_URL=https://d1hsp5bm4hkjmb.cloudfront.net

# Build React Vite production bundle
pnpm build

# Sync compiled dist folder to S3 Bucket
aws s3 sync dist/ s3://codexecute-frontend --delete
```

#### PowerShell (Windows):
```powershell
cd fe

# Set API URL environment variable in PowerShell
$env:VITE_API_URL="https://d1hsp5bm4hkjmb.cloudfront.net"

# Build React Vite production bundle
pnpm build

# Sync compiled dist folder to S3 Bucket
aws s3 sync dist/ s3://codexecute-frontend --delete
```

---

### Step 2: Enable Bucket Versioning & Disable Static Website Hosting for Security

1. In the **Properties** tab of your S3 Bucket, scroll down to **Bucket Versioning** and click **Edit**. Select **Enable** to turn on multi-version storage. This preserves build artifact history and supports rolling back to previous build versions whenever necessary.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/bucket-fe7.jpg" alt="Enable Bucket Versioning" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.3.4: Enabling Bucket Versioning to support rolling back to previous build versions</i>
</p>

</div>

2. Scroll down to **Static website hosting** and ensure it is set to **Disabled**. Disabling direct public Static Website access enforces absolute infrastructure security, mandating all incoming traffic to route securely through **Amazon CloudFront** with HTTPS encryption.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/bucket-fe8.jpg" alt="Disable Static Website Hosting" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.3.5: Disabling Static Website Hosting to secure S3 Bucket data</i>
</p>

</div>

---

### Step 3: Configure CloudFront Origin Access Control (OAC) Bucket Policy

To allow Amazon CloudFront to fetch private static assets from S3 without exposing the bucket to the public Internet, attach the following **Bucket Policy**:

1. Switch to the **Permissions** tab of the S3 Bucket, scroll down to **Bucket policy**, and click **Edit**.
2. Paste the JSON Bucket Policy granting explicit access strictly to CloudFront Distribution (`E14SU7QS7NEEO8`):

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

3. Click **Save changes** to save the security policy.

---

### Verification

At this point, the `codexecute-frontend` S3 Bucket is securely hardened with direct public access disabled, granting access exclusively to **Amazon CloudFront** to serve web traffic at: [https://d1hsp5bm4hkjmb.cloudfront.net](https://d1hsp5bm4hkjmb.cloudfront.net).
