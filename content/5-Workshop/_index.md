---
title: "Workshop"
date: 2026-07-30
weight: 5
chapter: false
pre: " <b> 5. </b> "
includeInReport: false
---

# CODEXECUTE WORKSHOP: ONLINE JUDGE & AUTOMATED CODE EVALUATION SYSTEM ON AWS SERVERLESS

#### Overview

The **CodExecute** Workshop guides you step-by-step through designing, deploying, and operating an automated Online Judge evaluation platform built entirely on **Cloud-Native AWS Serverless**.

This lab combines core Serverless services including **AWS Lambda** (API backend &amp; isolated Sandbox execution), **Amazon API Gateway** (REST API entrypoint), **Amazon SQS** (Submission buffer queue), **Amazon DynamoDB** (Metadata &amp; problem storage), **Amazon S3** (Testcases &amp; Static Web hosting), and **Amazon CloudFront** (CDN distribution).

#### Workshop Content Outline

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

  <a class="toc-link" href="./5.1-workshop-overview/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 01</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Introduction &amp; Overview</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">CodExecute platform overview, system architecture diagram, and AWS services used</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="./5.2-prerequiste/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 02</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Prerequisites &amp; Setup</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Dev environment setup, AWS CLI configuration, and local repository initialization</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="./5.3-fe/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 03</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Frontend Deployment &amp; CloudFront CDN Configuration</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Upload React Frontend to Amazon S3 and configure CloudFront distribution with OAC</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="./5.4-be/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 04</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Lambda Deployment via Docker &amp; ECR</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Build container images for API &amp; Sandbox Worker, push to ECR, and instantiate AWS Lambda</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="./5.5-dynamodb/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 05</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Amazon DynamoDB Setup</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Provision DynamoDB tables for Users, Problems, Submissions, TestCases, and define Primary Keys</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="./5.6-apigateway/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 06</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">API Gateway Configuration for Lambda API</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Configure HTTP API Gateway proxy routing, CORS headers, and integrate with Lambda API Backend</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="./5.7-sqs/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 07</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Amazon SQS Setup</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Create asynchronous submission buffer queue and configure Event Source Mapping to trigger Lambda Worker</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="./5.8-cloudwatch/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 08</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Monitoring with CloudWatch</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Monitor Lambda execution logs, SQS queue metrics, and system operational dashboards</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="./5.9-sns/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">STEP 09</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Amazon SNS &amp; Lambda Alarms Setup</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Configure Amazon SNS Topic notifications and build CloudWatch Alarms for automated fault detection</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="./5.10-cleanup/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #ef4444; background: #FEF2F2; padding: 5px 10px; border-radius: 6px; border: 1px solid #FEE2E2;">STEP 10</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Resource Cleanup</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Clean up all AWS Serverless infrastructure resources to prevent unexpected recurring charges</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

</div>