---
title: "Event 2"
date: 2026-07-04
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

# Summary Report: “AWS Meetup: Real Stories to Corporate Culture at Multinational Corporations & Cloud/DevOps Career Orientation”

### Event Objectives

The event was designed to comprehensively equip young tech talents with mindset, professional technical skills, and career orientation:

- **Equip 3rd and 4th-year tech students with an adaptable mindset** in response to volatile recruitment market dynamics while establishing long-term personal capability roadmaps.
- **Provide a realistic view of Data Analytics roles** within enterprise operations and decode corporate culture mechanics inside Multinational Corporations (MNCs).
- **Accurately define the core responsibilities of a DevOps Engineer**, evaluate market demand trends, and map out essential learning paths.
- **Foster an active learning mindset**, connecting individual growth journeys from student communities to professional partners within the cloud computing ecosystem.

### Speakers

The meetup brought together experienced practitioners and tech community leaders:

- **Mr. Dat Pham** – Data Analytics Engineer
- **Mr. Cuong Nguyen** – Process Engineer
- **Mr. Trong H. Truong** – DevOps Engineer @ Endava Vietnam
- **Mr. Danh Hoang Hieu Nghi** – AI Engineer, AWS Community Builder, AWS Student Builder Group Leader
- **Mr. Dinh Trung Kien** – Lead Developer @ Startup
- **Mr. Nguyen Minh Tho** – Student Representative

---

### Key Highlights

#### 1. Real-world Work and Essential Skills of a Data Analytics Engineer

The daily responsibilities of a Data Analytics Engineer vary dynamically based on business domain, operating model, and supported department needs:

- **At Kamereo**: Data Engineers focus on building periodic reporting pipelines (daily, weekly, monthly, quarterly) to closely track operational metrics. They design executive dashboards to monitor data trends, detect anomalies early, and collaborate across departments to resolve operational incidents.
- **At Colgate-Palmolive**: Responsibilities focus on supply chain and manufacturing data projects, integrating IoT sensors and factory machinery to identify operational cost reduction opportunities and maximize Equipment Efficiency.

**Core Skill Set for Data Analytics Engineers:**

| Core Skill Set | Professional Requirements & Practical Application |
| :--- | :--- |
| **Critical Thinking** | Analyze the truth behind metrics, asking the right questions to uncover business root causes. |
| **Communication** | Translate complex technical metrics into simple business language, enabling seamless cross-functional alignment. |
| **Data Storytelling** | Transform raw data into structured visual narratives with clear context, arguments, and actionable proposals. |
| **Problem Solving** | Convert analytical insights into process improvement solutions, delivering measurable business value. |

---

#### 2. Career Development Mindset and Standard Recruitment Roadmap at MNCs

Individual career progression in corporate environments follows a clear 5-stage evolution model:

1. **Follower / New Junior**: Primary focus on completing assigned tasks.
2. **Learner**: Actively seeks to understand the "why" behind processes and asks deep technical questions.
3. **Problem Solver**: Independently solves complex business problems.
4. **System Thinker**: Comprehends the big picture and interconnected dependencies across the organization.
5. **Super Star (Leader)**: Shapes strategic vision and leads organizational growth.

**Standard 4-Step MNC Recruitment Process:**

| Recruitment Step | Assessment Scope & Methodology |
| :--- | :--- |
| **1. Screening & Initial Interview** | Automated ATS CV screening; Recruiter evaluation of English communication fluency and general attitude. |
| **2. Capability Test** | Logic reasoning, algorithmic tests, or Situation Tests evaluating real-world problem resolution. |
| **3. Technical Interview** | Deep technical interview with Tech Lead/Manager utilizing the STAR framework (Situation, Task, Action, Result). |
| **4. Cultural Fit** | Executive leadership interview evaluating alignment with core company values and long-term vision. |

---

#### 3. Decoding Corporate Culture, Asian Lessons & the "Right Work" Philosophy

- **Modern Corporate Culture Models**:
  - **"No-Blame Post-Mortem" Culture** (Tech Sector): Focuses on analyzing root systemic causes rather than assigning individual fault when incidents occur.
  - **"Caring & Inclusive" Culture** (FMCG Sector): Places people at the core of organizational growth.
- **Strategic Development Lessons from Asia**:
  - **Japan**: Demonstrates the *"Wakon Yosai"* spirit (Japanese Spirit, Western Technology) via the Toyota Production System (TPS) with rigorous quality standards.
  - **South Korea**: Created the *"Miracle on the Han River"* by concentrating national resources into export-driven conglomerates (Chaebols) adhering strictly to international standards.
- **Standard Shifts in Vietnam**:
  - Transitioning from a *"Can do"* mindset to **"DOING IT RIGHT TO STANDARDS"** is a critical necessity.
  - Adhering to international security frameworks like **ISO 27001, SOC 2, and GDPR** protects corporate digital assets and national data sovereignty.
- **The "Right Work" Philosophy**: Encapsulates the mindset of carrying the nation's digital lifeline through three pillars:
  - **Being a Person**: Self-fulfillment.
  - **Doing a Profession**: Purpose and craftsmanship.
  - **Being a Citizen**: Community legacy.

---

#### 4. The Essence of DevOps and Tech Talent Trends in Vietnam

- **Market Trends (2016–2025)**: Specialized roles such as AI/ML, Data, Cloud, Security, and DevOps Engineers maintain superior growth in recruitment demand and compensation levels.
- **The Essence of DevOps**:
  - Far beyond writing CI/CD pipelines, containerizing with Docker/Kubernetes, or configuring Cloud infrastructure.
  - Practical scope heavily depends on project scale and business context, including on-call rotations, incident troubleshooting, emergency response, and cloud cost optimization.
- **Effective DevOps Learning Path**: Demands mastering core foundational knowledge (**Linux, Networking, Python/Golang, Git, Containers**) and understanding application execution mechanics rather than memorizing CLI commands.

---

#### 5. The Journey from "First Cloud AI Journey" to "AWS Partner"

A comprehensive **8-step progression roadmap** empowering young talent to continuously grow and share back value:

1. **Student Curiosity**: Starting from curiosity and proactive tech exploration.
2. **First Cloud Journey**: Accessing standardized cloud computing learning environments.
3. **Workshop & Community**: Attending meetups to network and learn from experienced mentors.
4. **Hands-on Labs**: Applying theory through hands-on labs to build practical skills.
5. **School Projects**: Applying cloud knowledge to real university projects.
6. **Portfolio**: Building a personal product portfolio demonstrating hands-on capabilities.
7. **AWS Partner**: Becoming a trusted partner solving real enterprise business challenges.
8. **Share Back**: Returning to contribute and guide the next generation of students.

---

#### 6. Designing a Scalable URL Shortening Service on AWS

The AWS Serverless URL Shortener architecture is engineered for high-concurrency traffic with sub-10ms response latency:

- **API & Compute Tier**: Uses **Amazon API Gateway** for request entry and **AWS Lambda** for Serverless encoding/decoding logic, eliminating server management costs and scaling automatically on demand.
- **Database Tier**: **Amazon DynamoDB** provides high-speed key-value NoSQL storage delivering single-digit millisecond lookup performance and seamless horizontal partitioning.
- **Caching & Edge Tier**: **Amazon CloudFront** paired with **Amazon ElastiCache (Redis)** caches high-frequency "hot URLs" at edge locations for instant user redirection.

**URL Shortener Architecture Service Breakdown:**

| Architecture Layer | AWS Service | Core Technical Function |
| :--- | :--- | :--- |
| **API Endpoint** | **Amazon API Gateway** | Ingests, routes, and manages HTTP request traffic. |
| **Serverless Compute** | **AWS Lambda** | Executes URL encoding/decoding logic without server overhead. |
| **Caching Layer** | **Amazon ElastiCache (Redis) & CloudFront** | Caches data at the edge, reducing latency and database load. |
| **Database Layer** | **Amazon DynamoDB** | High-performance key-value storage (short URL - long URL mapping). |

---

### Key Takeaways

#### Orientation and Mindset (Design Mindset)
- **Apply System Thinking**: Evaluate technical problems within the big picture of business operations rather than isolated tasks.
- **Always ask "Why" before "How"** to pinpoint true core objectives.
- **Deeply understand corporate culture values** and the "Right Work" philosophy to shape a professional service attitude.

#### Technical Knowledge & Strategy (Technical Architecture)
- Tooling contexts continuously evolve, but **core foundational knowledge remains timeless**.
- **Upgrade workflows to international standards** (ISO 27001, SOC 2, GDPR) to safeguard organizational digital assets.
- **Leverage AI as a productivity tool** without becoming overly dependent or losing active critical thinking.

#### Modernization Strategy
- Follow a step-by-step capacity growth roadmap (**Follower → Learner → Problem Solver**) instead of chasing titles prematurely.
- Integrate physical supply chain discipline with digital supply chain governance to protect enterprise resources.

---

### Practical Application

- **Self-Positioning & Development**: Identify current skill levels, ask deep questions, and learn actively from senior engineers.
- **Develop Complementary Soft Skills**: Practice critical thinking and Data Storytelling when building reporting dashboards.
- **Strengthen Core Foundations**: Focus on mastering Linux, Networking, Git, and Container technologies (Docker) as prerequisites for Cloud/DevOps paths.
- **Active Community Participation**: Complete hands-on labs, refine personal portfolios, and engage with networks like AWS Student Builder Group to prepare for the "Share Back" phase.

---

### Event Experience

The AWS Meetup delivered deep practical value, helping students and young engineers reshape career development mindsets and better understand corporate environments:

- **Learning from Experienced, Hands-on Engineers**: Diverse speakers across Data, Process, DevOps, and AI representing major companies (Kamereo, Colgate-Palmolive, Endava Vietnam) provided vivid real-world perspectives.
- **Gaining Deep Insights into Culture and Professional Philosophy**: Successfully decoded modern corporate culture models (No-Blame Post-Mortem, Caring & Inclusive) and the humanistic "Right Work" philosophy.
- **Clearly Shaping the Future Development Roadmap**: Gained clarity on recruitment trends and salary scales in Vietnam, absorbing a structured 8-step roadmap from curious student to cloud professional.
