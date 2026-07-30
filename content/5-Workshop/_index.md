---
title: "Workshop"
date: 2026-07-30
weight: 5
chapter: false
pre: " <b> 5. </b> "
includeInReport: false
---

# CODEXECUTE WORKSHOP: ONLINE JUDGE & AUTOMATED ALGORITHM EVALUATION ON AWS SERVERLESS

#### Overview

The **CodExecute** workshop guides you step-by-step through designing, deploying, and operating an automated Online Judge evaluation platform built entirely on a **Cloud-Native AWS Serverless** architecture.

This lab integrates core Serverless services including **AWS Lambda** (REST API backend & isolated execution sandbox), **Amazon API Gateway** (REST API entrypoint), **Amazon SQS** (Asynchronous submission buffer queue), **Amazon DynamoDB** (High-speed metadata & problemset storage), **Amazon S3** (Testcase repository & frontend static hosting), and **Amazon CloudFront** (Global CDN content delivery).

#### Workshop Sections

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

  <a class="toc-link" href="5.1-Workshop-overview/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 01</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Introduction &amp; Overview</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">CodExecute platform overview, architecture diagram, and AWS services design</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.2-Prerequiste/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 02</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Prerequisites</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Environment setup, AWS CLI configuration, and local project initialization</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.3-fe/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 03</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Deploy Frontend &amp; CloudFront CDN</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Deploy React Frontend to Amazon S3 and configure CloudFront Distribution with OAC</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.4-be/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 04</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Deploy Lambda via Docker &amp; ECR</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Build container images for API and Sandbox Worker, push to ECR, and create AWS Lambda functions</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.5-DynamoDB/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 05</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Setup Amazon DynamoDB Tables</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Provision DynamoDB tables for Users, Problems, Submissions, and TestCases with Primary Keys</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.6-APIGateway/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 06</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Configure API Gateway for Lambda API</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Configure HTTP API Gateway proxy routes, CORS policies, and integration with API Lambda</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.7-SQS/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 07</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Setup Amazon SQS</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Create asynchronous submission queue and configure Event Source Mapping trigger to Worker Lambda</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.8-CloudWatch/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 08</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Monitor with CloudWatch</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Inspect Lambda execution log groups, SQS queue metrics, and system performance dashboards</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.9-SNS/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 09</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Setup SNS &amp; Lambda Alarms</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Configure Amazon SNS notification topics and CloudWatch Alarms for automated failure alerts</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.10-Cleanup/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #ef4444; background: #FEF2F2; padding: 5px 10px; border-radius: 6px; border: 1px solid #FEE2E2;">STEP 10</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Clean Up Resources</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Tear down all AWS Serverless infrastructure resources to prevent unexpected charges</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

</div>