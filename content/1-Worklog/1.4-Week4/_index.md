---
title: "Worklog Week 4"
date: 2026-07-06
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

### Week 4 Objectives (06/07/2026 – 12/07/2026):
* Execute **Phase 3: Asynchronous Pipeline & Lambda Sandbox** for the **CodExecute** project.
* Program AWS Lambda Worker Runners to consume submission messages from Amazon SQS, compiling and executing 4 languages (C++, Java, Python, JS).
* Enforce strict isolated sandbox runtime boundaries on AWS Lambda to completely eliminate Remote Code Execution (RCE) risks.

### Weekly Tasks Breakdown:
| Day | Task Description | Start Date | End Date | Resource Links |
| --- | --- | --- | --- | --- |
| Mon | - **SQS Job Producer Integration:** Configure Lambda API to push submission payloads (user code, language, problem_id) into Amazon SQS `Submissions Queue` upon receiving "Submit Code" requests. | 06/07/2026 | 06/07/2026 | [Amazon SQS Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/) |
| Tue | - **AWS Lambda Worker Runner Design:** Program Lambda Worker handlers to consume SQS messages, invoking compiler toolchains (GCC 13 for C++, OpenJDK 21 for Java, Python 3.12, Node.js 20 for JS). | 07/07/2026 | 07/07/2026 | [AWS Lambda Execution Environment](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-context.html) |
| Wed | - **RCE Protection Sandbox Hardening:** Disable external network access (`VPC Network: Disabled / Strict Subnet Security Groups`), enforcing hard limits (512MB RAM, 1 vCPU, 5s Timeout, 20 PIDs limit). | 08/07/2026 | 08/07/2026 | [AWS Lambda Security Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/lambda-security.html) |
| Thu | - **S3 Testcase Processing & Auto-Grading:** Implement logic to fetch testcase files (input/output text) from S3, evaluating output correctness (Accepted, Wrong Answer, TLE, MLE) and updating DynamoDB. | 09/07/2026 | 09/07/2026 | [Amazon S3 Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/) |
| Fri | - **Hands-on Phase 3 Finalization:** End-to-end testing of submission evaluation pipeline: Client → API Gateway → Lambda API → SQS → Lambda Worker Sandbox → S3 Testcases → DynamoDB. | 10/07/2026 | 10/07/2026 |  |

### Week 4 Deliverables & Achievements:
* **Asynchronous Evaluation Pipeline Complete:** Established asynchronous message flow via SQS, allowing the platform to buffer thousands of simultaneous submissions without overloading backend services.
* **Multi-Language Lambda Worker Sandbox:** Successfully built Lambda Worker handlers evaluating C++, Java, Python, and JavaScript submissions with automated Compile Error and Runtime Error capture.
* **100% RCE Security Hardening:** Enforced isolated container runtime boundaries with disabled external network access and strict resource caps (512MB RAM, 5s Timeout), mitigating RCE and Fork Bomb threats.
* **Automated Testcase Evaluation via S3:** Implemented high-speed testcase parsing from Amazon S3, accurately recording execution runtimes (ms) and memory usage (MB) into DynamoDB.
* **Completed 100% of CodExecute Phase 3:** Asynchronous code execution within isolated Lambda Sandboxes operates seamlessly with evaluation response times < 2.0 seconds.
