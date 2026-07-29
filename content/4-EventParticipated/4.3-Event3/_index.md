---
title: "Event 3"
date: 2026-07-25
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report, including this warning.
{{% /notice %}} -->

# Summary Report: “AWS Agentic AI Build Week - Pitching & Demo Day Showcase”

### Event Objectives

- **Showcase Real-World AI Agent Products**: Evaluate and celebrate solutions built during the AWS Agentic AI Build Week (AABW) Hackathon.
- **Apply Agentic AI Architecture on AWS**: Explore how competing teams leveraged **Amazon Bedrock AgentCore**, **Strands Agent**, **Model Context Protocol (MCP)**, **Serverless**, and Multi-Agent Systems to solve complex business problems.
- **Cost & Performance Optimization**: Analyze cloud infrastructure costs, latency, and scalability when deploying Generative AI and Agentic AI solutions to production.
- **Networking & Community Building**: Connect AI engineers, Solution Architects, young developers, and AWS experts.

### Teams & Showcase Projects

- **Signal Scout Team** – *Project:* **Signal Scout** (Agentic AI-powered early detection system for corporate strategic changes)
- **Plan V Team** – *Project:* **Solution Architect Professional AI Native App** (AI Native app automating cloud architecture design, diagrams, and cost estimation for Solution Architects)
- **3KA Team** – *Project:* **S.H.E.P.H.E.R.D** (Real-time crowd flow evaluation, queue monitoring, and hazard detection system combining Computer Vision & Agentic AI)
- **One Team (AWS Track Winner 🏆)** – *Project:* **KFC Bot Agent** (Multi-channel AI-powered conversational ordering system)

---

### Key Highlights 

#### 1. Signal Scout - Automated Corporate Strategy Signal Analysis

- **Business Problem**: Tracking strategic changes of competitors or partners (restructuring, executive shifts, financial reports) is scattered across public web sources and time-consuming to gather manually.
- **Solution & Features**:
  - Automatically collects and validates evidence to detect early strategic restructuring signals.
  - Analyzes financial and operational metrics to construct predictive scenarios.
  - Presents verifiable findings on an Executive Dashboard with evidence linked to original data sources.
- **Partners & Integrated Tools**: AWS, Langfuse (LLM Observability & Tracing), TinyFish, and Apify (Web Crawling & Scraping).
- **Agentic AI Architecture**:
  - Utilizes **AgentCore Runtime**, **AgentCore Memory** (Short-Term Memory), and **Strands Agent**.
  - Multi-Agent design split into two subagents: **Crawler Subagent** (integrating TinyFish/Apify) and **Analysis Subagent** (connecting Bedrock Guardrails, Lambda, and Langfuse).
  - Proposed a cost-efficient architecture using **AgentCore Gateway** paired with **MCP (Model Context Protocol)** for WebSearch and Browser tools.
- **Cost Analysis**: Estimated total operational cost ranges from **$81 to $359/month** (with AWS infrastructure costs at only **$17 - $130/month**, making it accessible for enterprises).

#### 2. Solution Architect Professional AI Native App (Plan V) - Work Automation for Solution Architects

- **Real-World Challenge**: When clients urgently request AI system designs, Solution Architects (SAs) spend significant time reading long BRD/PRD documents line by line, drawing architecture diagrams manually from scratch, writing IaC code (Terraform), and manually estimating costs based on experience.
- **AI Native App Solution**:
  - Automatically analyzes natural language and structured project requirement documents.
  - Drafts high-level architecture options (hybrid-cloud aware) aligned with corporate standards.
  - Generates editable **Draw.io** diagrams and AWS architecture diagrams using official **AWS Architecture Icons**.
  - Produces real-time directional AWS cost estimates for `ap-southeast-1`.
  - Surfaces requirement gaps and allows interactive refinement via a chat sidebar with custom per-project instructions.
- **Workflow & Technical Architecture**:
  - Client uploads document / chats -> System queries **Knowledge Base** (Internal Docs, Architecture References) + **Amazon Bedrock** + **Draw.io MCP** + **AWS Pricing MCP** -> Outputs Summary, Architecture Diagrams, Cost Estimate, and Terraform IaC.
  - AWS Infrastructure: Deployed via Terraform, S3, CloudFront, Cognito, WAF, Application Load Balancer, VPC (Public/Private Subnets), **ECS Fargate** (Backend & Agent containers), EFS, PostgreSQL, and Bedrock.
- **Impact**: Reduces architecture proposal preparation time from days to minutes; provides an immediate grounded draft instead of starting from a blank page.

#### 3. S.H.E.P.H.E.R.D (Team 3KA) - Crowd Flow Monitoring & Analysis using Computer Vision & AI Agents

- **Project Background**: Originally planned as a Capstone Project, Team 3KA prototyped S.H.E.P.H.E.R.D during AABW to validate the concept under 24-hour hackathon pressure.
- **Problem Statement**: Venue staff at event halls, shopping malls, or airports struggle to manually monitor multiple entrances, queues, and crowded areas simultaneously, leading to reactive responses to congestion.
- **Solution Architecture**:
  - **Computer Vision Layer**: Uses **YOLO + ByteTrack** for real-time person detection and tracking, running on **Amazon SageMaker** and **Kinesis Video Streams**.
  - **Agentic AI Layer**:
    - *Autonomous Monitor:* Continuously monitors crowd density metrics, detects congestion indicators, predicts overcrowding pressure, and generates proactive alerts.
    - *Operator Copilot:* Allows staff to ask natural language questions (e.g., *"Is Booth Area A congested?"*) and receive concise answers with recommended staff actions.
- **24-Hour Hackathon Challenges**: Managing live stream video latency, preserving tracking across frames, managing AWS costs, debugging code until 3 AM, accidentally pushing `.env` files to GitHub, drinking 5 Redbulls, and midnight strolls for stress relief.

#### 4. KFC Bot Agent (One Team - AWS Track Winner 🏆) - Multi-Channel Conversational Ordering

- **The Trigger**: Lessons from McDonald's ending their AI drive-thru trial in over 100 US locations demonstrated that AI ordering is a complex system problem (understanding items, quantities, variants, voucher rules, cart state, and error handling).
- **The Problem**: Customers chatting on Zalo or Messenger are forced to switch apps to place food orders, causing friction and lost momentum. Human support agents cannot scale during traffic spikes.
- **KFC Bot Agent Solution**:
  - Enables seamless ordering directly within existing chat channels (Zalo OA, Messenger, WhatsApp, etc.) without app downloads or account creation.
  - Core Philosophy: **"A Chatbot Replies. An Agent Acts."**
  - 5-Step Agent Loop: **Goal** (Understand intent) -> **Plan** (Decide steps) -> **Tools** (Query trusted data) -> **Act** (Update cart & apply vouchers) -> **Verify** (Confirm against real cart). The model understands natural language, but tools determine actual system state.
- **"Design Once | Deploy Everywhere" Architecture**:
  - **Channel Adapters** (Zalo, WhatsApp, Telegram, Instagram) decouple messaging channels, allowing new channels and tools to be added without rebuilding the core engine.
  - **Ingestion Layer**: WAF, API Gateway, Lambda Webhook Handler, SQS.
  - **AgentCore Runtime**: Bedrock AgentCore, Agent Orchestration, Tool Use, Guardrails.
  - **Tool & Memory Layers**: AgentCore Gateway, Lambda Workers, DynamoDB (Session/State, Products, Orders), OpenSearch Service (Vector Store & Full-text Search).
  - **Observability & Security**: CloudWatch, X-Ray, CloudTrail, GuardDuty, AWS Secrets Manager, IAM.
- **Four Numbers Worth Writing Down**:
  - **$0.006 per order**: Highly cost-effective infrastructure (calculated for 500 orders/day).
  - **$88 per month**: Total cloud infrastructure cost (Bedrock accounts for 75%).
  - **3–5s latency**: End-to-end response time from message sent to reply received.
  - **-60% infrastructure code**: AgentCore replaces conventional infrastructure management layers.

---

### Lessons Learned

#### 1. Agentic AI Design Mindset
- **Shift from Chatbot to Agent**: Traditional chatbots only reply with text, whereas true AI Agents plan, execute tools, take actions, and verify results.
- **"The model understands. The tools decide what is real"**: Avoid relying solely on LLMs for business logic execution; use LLMs for natural language understanding while enforcing accuracy via deterministic tools/APIs.
- **"Design Once | Deploy Everywhere"**: Building a modular architecture with Channel Adapters and AgentCore Gateway allows seamless multi-channel scaling.

#### 2. Technical Architecture & Cost Optimization
- Leveraging **Amazon Bedrock AgentCore**, **Strands Agent**, and **MCP (Model Context Protocol)** reduces infrastructure code by up to 60% and simplifies tool integrations (Draw.io, Pricing API, Web Search).
- Combining Serverless services (Lambda, DynamoDB, S3) with Bedrock AgentCore keeps operational costs exceptionally low ($0.006/order or $35–$88/month for small-to-medium systems).

#### 3. Real-World Hackathon Insights & Teamwork
- **"Clear direction beats too many options"**: A focused direction outperforms overly complex feature sets.
- **"Execution matters more than perfection"**: Delivering a working MVP is far more valuable than pursuing an unfinished perfect design.
- **"Small, finished work beats big, broken ideas"**: A polished, functional feature always beats large, broken concepts.

---

### Practical Application to Work

- **Applying AI Agent Patterns**: Transition chatbot design patterns to Agentic AI models incorporating Tool Use, Memory, and Guardrails on AWS Bedrock.
- **Standardizing Multi-Channel Architecture**: Use Adapter and Event-Driven patterns to design resilient, extensible backend systems.
- **Optimizing Personal Workflows**: Utilize AI tools for automated diagram generation, IaC (Terraform) generation, and cost estimation.
- **Enhancing Teamwork & Pitching Skills**: Present visual demo scenarios focusing on Problem-Solution-Impact and concrete metrics.

---

### Event Experience

The **AWS Agentic AI Build Week Demo Day (July 25, 2026)** was an inspiring and high-energy event. Highlights included:

- **Diverse Solution Showcase**: Ranging from corporate strategy tools (Signal Scout), developer automation (Plan V), real-time computer vision (S.H.E.P.H.E.R.D), to commercial multi-channel AI ordering (KFC Bot Agent).
- **24-Hour Hackathon Spirit**: Experiencing the dedication of competing teams, complete with overnight coding stories, Redbull cans, GitHub `.env` mishaps, and late-night walks.
- **High-Quality Feedback**: Constructive insights from AWS judges regarding cost management, security guardrails, and real-world enterprise viability.
- **Celebration & Networking**: Celebrating One Team's win with KFC Bot Agent and connecting with fellow developers and AWS experts concluded Build Week on a high note.

#### Participation Evidence Photos

![Event 3 Evidence 1](/images/4-EventParticipated/4.3-Event3/event3_evidence1.jpg)
*Photo 1: Selfie at the event*  

![Event 3 Evidence 2](/images/4-EventParticipated/4.3-Event3/event3_evidence2.jpg)
*Photo 2: Event showcase & presentation*


