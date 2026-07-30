---
title: "Setup Amazon SQS"
date: 2026-07-30
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

# SETTING UP AMAZON SQS FOR CODEXECUTE SUBMISSION QUEUE

In this section, we document how **Amazon Simple Queue Service (SQS)** was configured as the asynchronous submission buffer between the **API Lambda** (`codeexecute-api`) and the **Worker Lambda** (`codeexecute-worker`).

When a user clicks **SUBMIT**, the API Lambda does not execute the code directly. Instead, it pushes a submission job to an SQS queue, returns immediately to the user, and the Worker Lambda is automatically triggered to pick up and grade the submission asynchronously.

```
User clicks SUBMIT
      │
      ▼
codeexecute-api Lambda
  ├─ Saves Submission to DynamoDB (Status: "Pending")
  └─ Pushes job to SQS Queue
              │
              ▼ (SQS Event Source Mapping)
      codeexecute-worker Lambda
        ├─ Reads submission payload from SQS record
        ├─ Fetches testcases from S3
        ├─ Executes code in sandbox container
        └─ Updates Submission result in DynamoDB
```

The SQS message payload pushed by the API contains:
```json
{
  "submission_id": "...",
  "user_id": "...",
  "problem_id": "...",
  "language": "python|cpp|java|javascript",
  "code": "..."
}
```

#### Section Overview

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

  <a class="toc-link" href="5.7.1-create-queue/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">5.7.1</span>
      <span style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Create SQS Queue</span>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.7.2-event-source-mapping/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">5.7.2</span>
      <span style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Connect SQS to Lambda Worker (Event Source Mapping)</span>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

</div>
