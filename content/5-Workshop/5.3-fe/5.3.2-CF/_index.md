---
title: "Configure Amazon CloudFront & Multi-Origin Routing"
date: 2026-07-30
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

<!-- # CONFIGURING AMAZON CLOUDFRONT DISTRIBUTION & MULTI-ORIGIN ROUTING -->

In this section, we will configure an **Amazon CloudFront Distribution** to serve as the unified global CDN edge and Reverse Proxy for the **CodExecute** platform.

CloudFront provides intelligent URL path-based routing, seamlessly directing incoming traffic between the **Amazon S3 Bucket** (hosting static web assets) and **Amazon API Gateway** (handling RESTful backend APIs).

---

### Step 1: Create Initial CloudFront Distribution with Amazon S3 Origin

1. Navigate to the **Amazon CloudFront Console** and click **Create distribution**.
2. Under **Origin domain**, select your S3 Bucket `codexecute-frontend.s3.amazonaws.com`.
3. Under **Origin access**, select **Origin access control settings (recommended)** (OAC) to securely authorize CloudFront access to the private S3 bucket.
4. Under **Default cache behavior**:
   - **Viewer protocol policy:** Select **Redirect HTTP to HTTPS**.
   - **Allowed HTTP methods:** Select **GET, HEAD**.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf2.jpg" alt="Configure OAC and Cache Behavior" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.3.2.1: Configuring Origin Access Control (OAC) and Viewer Protocol Policy</i>
</p>

</div>

5. Click **Create distribution** to initialize deployment.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf3.jpg" alt="CloudFront Distribution Created Successfully" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.3.2.2: CloudFront Distribution successfully provisioned</i>
</p>

</div>

---

### Step 2: Add Second Origin for Amazon API Gateway

To allow the React Frontend application to make API requests to `/api/*` over the same domain root without encountering CORS (Cross-Origin Resource Sharing) restrictions, add API Gateway as a second origin:

1. Select your CloudFront Distribution, switch to the **Origins** tab, and click **Create origin**.
2. Under **Origin domain**, paste your API Gateway endpoint domain (e.g., `g0wll7768b.execute-api.ap-southeast-1.amazonaws.com`).

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf4.jpg" alt="Add API Gateway Origin" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.3.2.3: Adding second Origin pointing to Amazon API Gateway Endpoint</i>
</p>

</div>

3. In **Origin path**, enter `/prod` (to automatically route API traffic to the `/prod` deployment stage).
4. Under **Protocol**, select **HTTPS only**.
5. Click **Create origin** to save origin configuration.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf5.jpg" alt="Configure Origin Path /prod" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.3.2.4: Setting Origin Path /prod and HTTPS Only protocol</i>
</p>

</div>

---

### Step 3: Configure Behavior Routing & Set Priority Order

Switch to the **Behaviors** tab to set up path-based routing rules separating Frontend static assets from Backend REST APIs:

1. **Create Behavior for REST APIs (`/api/*`):**
   - Click **Create behavior**.
   - **Path pattern:** Enter `/api/*`.
   - **Target origin:** Select the API Gateway Origin created in Step 2.
   - **Allowed HTTP methods:** Select **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE** (enabling full RESTful method support).
   - **Cache policy:** Select **CachingDisabled** (or forward all query strings and headers to prevent stale API response caching).
   - Click **Create behavior**.

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf6.jpg" alt="Create Behavior for /api/*" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.3.2.5: Configuring Cache Behavior for /api/* path targeting API Gateway</i>
</p>

</div>

2. **Verify & Adjust Behavior Priority Order:**
   After configuration, the Behaviors table should display the following priority execution order:

<div align="center">

<img src="/images/5-Workshop/5.3-fe/cf7.jpg" alt="Behavior Priority Order List" style="width: 80%; max-width: 900px; border-radius: 6px;">

<p style="font-size: 1rem; font-weight: 600; margin-top: 8px;">
<i>Figure 5.3.2.6: Behavior priority list (Priority 0 for /api/* and Priority 1 for Default)</i>
</p>

</div>

| Priority | Path Pattern | Target Origin | Description |
| :---: | :---: | :--- | :--- |
| **0 (Highest)** | `/api/*` | **API Gateway** (`/prod`) | Routes API request calls directly to the API Gateway `/prod` stage. |
| **1 (Default)** | `Default (/*)` | **Amazon S3** (`codexecute-frontend`) | Serves React static web assets (HTML/JS/CSS) from S3. |

---

### Verification

Upon completing Multi-Origin Routing configuration on CloudFront:
- All static webpage requests (e.g., `https://d1hsp5bm4hkjmb.cloudfront.net/`) are prioritized at `1` and served directly from **Amazon S3**.
- All backend API requests (e.g., `https://d1hsp5bm4hkjmb.cloudfront.net/api/v1/problems`) are prioritized at `0` and routed seamlessly to **Amazon API Gateway** (`/prod` stage).
- The platform functions cohesively on a single HTTPS domain, accelerating global response times and eliminating CORS barriers.
