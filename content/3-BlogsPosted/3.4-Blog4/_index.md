---
title: "Blog 4"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# REVOLUTIONIZING FAN ENGAGEMENT: BUNDESLIGA GENERATIVE AI-POWERED LIVE COMMENTARY

In modern sports streaming and media distribution, engaging a global fanbase requires delivering real-time match updates tailored to diverse languages and localized personas. However, traditional media outlets often lack localized broadcasts or coverage in fans' preferred writing styles. This article explores a Generative AI-powered live commentary (ticker) solution developed by AWS in collaboration with Sportec Solutions AG for Germany's Bundesliga. The solution automatically converts raw pitch event data into rich, multi-lingual, and multi-style commentary in real time.

### KEY HIGHLIGHTS:

* **Multi-Language & Personalized Personas**: Generates automated match commentaries simultaneously in various languages and tailored writing tones, such as "Sports Journalist", "Casual", or "Bro (Gen Z)".
* **Data-Driven Event Processing**: Ingests approximately 1,600 live pitch event data points per Bundesliga match (e.g., shot types, player speed, defender count, and pressure metrics).
* **Fully Serverless AWS Architecture**: Leverages AWS Lambda, Amazon ECS Fargate, AWS AppSync, and Amazon Bedrock to auto-scale seamlessly during fast-paced matches and scale down to zero off-match days for cost efficiency.
* **Low-Latency Streaming Sync**: Achieves end-to-end processing latency of 7 to 12 seconds from the moment an action occurs on the pitch to UI rendering, aligning within standard video broadcasting delays.

### REAL-WORLD SCENARIO:

International football fans following Bundesliga teams abroad need real-time, minute-by-minute live updates in their native language and preferred tone, even when live regional broadcasts or local language commentary are unavailable.

### ARCHITECTURE OVERVIEW:

```text
Live Pitch Event Data ➔ Bundesliga Datahub / Amazon ECS Fargate ➔ AWS Lambda & Amazon Bedrock ➔ AWS AppSync (GraphQL API) ➔ Amazon DynamoDB & Frontend UI
```

In this architecture:

* **Event Ingestion & Extraction (Amazon ECS Fargate)**: Live event data from the pitch (~1,600 events per game) is ingested via Bundesliga's Datahub and processed on ECS Fargate to extract key match attributes (e.g., player metrics, pressure, chance evaluation).
* **Prompt Engineering & GenAI Generation (AWS Lambda & Amazon Bedrock)**: AWS Lambda formats the extracted data into structured prompts specifying target languages and writing styles, then makes API calls to Amazon Bedrock to generate captivating ticker commentary.
* **Real-time API & Persistence (AWS AppSync & Amazon DynamoDB)**: AWS AppSync receives the generated commentary and distributes it instantly via GraphQL subscriptions to downstream consumers (web UI), while automatically persisting all entries into Amazon DynamoDB for durability.

### IMPLEMENTATION STEPS:

1. **Capture Event Data**: Pitch actions trigger structured event data (shots, passes, fouls) sent through the Bundesliga Datahub network.
2. **Context Extraction**: ECS Fargate parses raw JSON payloads to extract detailed contextual metrics (e.g., shot condition, goalkeeper distance).
3. **LLM Ingestion & Styling**: AWS Lambda forwards the payload alongside persona prompts to Amazon Bedrock foundation models.
4. **Real-time Distribution**: AWS AppSync broadcasts live ticker entries to web applications via GraphQL subscriptions within 7–12 seconds.

### CONCLUSION:

By integrating rich sports data feeds with Amazon Bedrock and serverless AWS services, Bundesliga and Sportec Solutions have created an automated solution to engage international sports audiences at scale. This architectural foundation sets the stage for future innovations, including expressive AI voice commentary generation and automated visual asset production for global sports broadcasts.

---
**Original Article Link:**  
[AWS Media Blog - Revolutionizing Fan Engagement: Bundesliga Generative AI-Powered Live Commentary](https://aws.amazon.com/blogs/media/revolutionizing-fan-engagementcer-bundesliga-generative-ai-powered-live-commentary/)
