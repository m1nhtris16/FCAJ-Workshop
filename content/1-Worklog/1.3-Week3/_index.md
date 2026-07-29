---
title: "Worklog Week 3"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

### Week 3 Objectives (29/06/2026 – 05/07/2026):
* Launch **Phase 2: Frontend & Backend Core Development** for the **CodExecute** project.
* Develop a modern Single Page Application (React + Vite + TailwindCSS) featuring an interactive Monaco Code Editor.
* Develop a robust RESTful API backend using FastAPI, packaged to execute on AWS Lambda connected to Amazon DynamoDB.

### Weekly Tasks Breakdown:
| Day | Task Description | Start Date | End Date | Resource Links |
| --- | --- | --- | --- | --- |
| Mon | - **React Frontend UI Development:** Initialize React + Vite + TailwindCSS project. <br> - Build Problemset browsing UI with difficulty filters (Easy, Medium, Hard) and algorithmic topic tagging (Binary Tree, DP, Graph, etc.). | 29/06/2026 | 29/06/2026 | [Vite Documentation](https://vitejs.dev/) |
| Tue | - **Interactive Code Editor (Monaco Integration):** Integrate Monaco Editor supporting syntax highlighting for 4 languages (**C++, Java, Python, JavaScript**). <br> - Implement dual modes: **Run Code** (custom input) and **Submit Code** (full testcase suite grading). | 30/06/2026 | 30/06/2026 | [Monaco Editor React](https://github.com/react-monaco-editor/react-monaco-editor) |
| Wed | - **FastAPI RESTful Backend Development:** Author FastAPI application endpoints: JWT Authentication (login/register), Problems API, Submissions API, User Profile & Social Network (Posts, Comments, Follow). | 01/07/2026 | 01/07/2026 | [FastAPI Documentation](https://fastapi.tiangolo.com/) |
| Thu | - **AWS Lambda API Handler Packaging:** Wrap FastAPI app using Mangum Serverless Adapter to execute seamlessly on AWS Lambda (512MB RAM, 10s Timeout). <br> - Connect Lambda API with Amazon DynamoDB for user and problem data persistence. | 02/07/2026 | 02/07/2026 | [AWS Lambda Python Guide](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html) |
| Fri | - **Hands-on Frontend & Backend Integration (Phase 2 Completion):** Integrate React Frontend with API Gateway & Lambda API Handler; deploy finalized Web UI to CloudFront CDN + S3. | 03/07/2026 | 03/07/2026 | [Amazon CloudFront S3 Integration](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-https-alternate-domain-names.html) |

### Week 3 Deliverables & Achievements:
* **Interactive Code Editor UI:** Built a responsive React + Vite web application integrated with Monaco Code Editor supporting 4 programming languages with real-time syntax checking and theme selection.
* **High-Performance FastAPI RESTful APIs:** Developed complete RESTful API suite handling JWT authentication, problem management, submission history, and social developer interactions.
* **Serverless Backend on AWS Lambda:** Successfully packaged FastAPI to execute on AWS Lambda connected to DynamoDB, delivering API response latencies < 200ms.
* **CloudFront CDN & S3 Deployment:** Deployed compiled React SPA assets to Amazon S3 behind CloudFront CDN for global low-latency page loads.
* **Completed Phase 2 of CodExecute:** Core Frontend and RESTful Backend Serverless architecture are operational and ready for asynchronous evaluation integration.
