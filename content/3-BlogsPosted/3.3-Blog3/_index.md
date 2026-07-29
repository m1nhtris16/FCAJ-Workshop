---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

# BUILD FULLSTACK AI APPS IN MINUTES WITH THE NEW AWS AMPLIFY AI KIT

In the Generative AI era, integrating intelligent capabilities such as Chatbots, conversational search, or text summarization into web and mobile applications often requires deep expertise in cloud infrastructure, data streaming pipelines, and Machine Learning model management. The AWS Amplify AI Kit abstracts away this complexity, enabling Full-Stack developers to build GenAI features rapidly using familiar TypeScript constructs on AWS Serverless infrastructure.

### KEY HIGHLIGHTS:

* **TypeScript-First AI Routes**: Built on Amplify Gen 2, developers can define AI Routes directly within their backend data schema using TypeScript. It supports two primary types: *Conversation* (real-time multi-turn chat with automatic history persistence) and *Generation* (structured data generation on request).
* **Type-Safe Experience & Built-in React Hooks/UI Components**: Ensures complete type synchronization between Backend and Frontend. The `@aws-amplify/ui-react-ai` library provides React Hooks (`useAIConversation`, `useAIGeneration`) and pre-built UI components (`<AIConversation/>`) to set up a real-time chat interface in just a few lines of code.
* **Generative AI as Data (Data Tools)**: Enables Large Language Models (LLMs) on Amazon Bedrock to directly query application Data Models using `a.ai.dataTool`. All AI data access strictly honors user owner-based authorization rules.
* **Generative UI Support**: Allows the AI assistant to respond not just with plain text, but directly with custom React UI components (`responseComponents`) defined in your code (e.g., `WeatherCard`, `RecipeCard`) for a rich conversational experience.
* **Serverless Architecture & Rapid Cloud Sandbox Workflow**: System automatically scales based on actual usage (pay-per-use). Developers can leverage `ampx sandbox` to iterate on backend cloud resources in real time without disrupting production environments.

### REAL-WORLD SCENARIO:

A full-stack developer wants to build an AI Assistant application capable of real-time multi-turn conversation, querying private user `Post` records, and rendering dynamic React UI components (Generative UI) inside the chat stream.

The architecture for the Full-Stack AI Application using Amplify AI Kit is as follows:

> **User Interaction (React UI / `<AIConversation/>`) → AWS AppSync (GraphQL & Auth Check) → AWS Lambda (Orchestrator) → Amazon Bedrock (Claude 3.5 Sonnet / Streaming API) → Amazon DynamoDB (Chat History & App Data)**

![Sơ đồ kiến trúc AI Full-Stack với AWS Amplify AI Kit](/images/blog3architect.png)

In this architecture:

* **Frontend & AppSync API Gateway**: The user sends prompts from the React interface. AWS AppSync receives the request, validates user authentication (Cognito User Pools), and enforces security before forwarding.
* **AWS Lambda (Orchestrator)**: Lambda serves as the orchestration bridge, fetching conversation history from Amazon DynamoDB, building context, and invoking Amazon Bedrock's Streaming Converse API.
* **Amazon Bedrock & Data Tools Execution**: The LLM (such as Anthropic Claude 3.5 Sonnet) analyzes the message. If user data is needed, Bedrock invokes the Data Tool (`PostQuery`) via AppSync to retrieve only the records owned by that authenticated user.
* **Realtime Streaming & Generative UI Render**: Responses from Bedrock are streamed back to the client via AppSync. The React UI renders the answer instantly along with matching React components (Generative UI), while persisting conversation logs into DynamoDB.

### IMPLEMENTATION & DEPLOYMENT GUIDE:

#### 1. Initial Setup & Cloud Sandbox:
* Ensure your AWS account has model access enabled for Foundation Models (such as Anthropic Claude 3.5 Sonnet or Claude 3 Haiku) in the Amazon Bedrock console.
* In your frontend project directory (Next.js or Vite), run:
  ```bash
  npm create amplify@latest
  ```
* Start your personal cloud sandbox environment for real-time iteration:
  ```bash
  npx ampx sandbox
  ```

#### 2. Define AI Routes in Data Schema (`amplify/data/resource.ts`):
Define your AI routes alongside your data models in TypeScript:

```typescript
import { a, defineData, type ClientSchema } from '@aws-amplify/backend';

const schema = a.schema({
  // Conversation route: Realtime, multi-turn AI chat
  chat: a.conversation({
    aiModel: a.ai.model('Claude 3 Haiku'),
    systemPrompt: 'You are a helpful assistant',
  })
  .authorization((allow) => allow.owner()),

  // Generation route: Request-response structured data generation
  generateRecipe: a.generation({
    aiModel: a.ai.model('Claude 3 Haiku'),
    systemPrompt: 'You are a helpful assistant that generates recipes.',
  })
  .arguments({
    description: a.string(),
  })
  .returns(
    a.customType({
      name: a.string(),
      ingredients: a.string().array(),
      instructions: a.string(),
    })
  )
  .authorization((allow) => allow.authenticated()),
});

export type Schema = ClientSchema<typeof schema>;

export const data = defineData({
  schema,
  authorizationModes: {
    defaultAuthorizationMode: "userPool",
  },
});
```

#### 3. Connect Frontend with Type-Safe Client & React Hooks:
Use `@aws-amplify/ui-react-ai` to interface with your AI routes:

```typescript
import { generateClient } from "aws-amplify/api";
import { Schema } from "../amplify/data/resource";
import { createAIHooks } from "@aws-amplify/ui-react-ai";

const client = generateClient<Schema>();
const { useAIGeneration, useAIConversation } = createAIHooks(client);

function Chat() {
  const [
    {
      data: { messages },
      isLoading,
      hasError,
    },
    sendMessage,
  ] = useAIConversation('chat');
  
  // Render Chat UI...
}

function RecipeGenerator() {
   const [{ data, isLoading }, handleGenerate] = useAIGeneration('generateRecipe');
   // Render Recipe Generator UI...
}
```

#### 4. Generative AI as Data (Data Tools):
Grant the LLM access to query your application data models (e.g., searching user `Post` records) by adding a `dataTool`:

```typescript
const schema = a.schema({
  Post: a.model({
    title: a.string(),
    body: a.string()
  })
  .authorization((allow) => allow.owner()),

  chat: a.conversation({
    aiModel: a.ai.model('Claude 3 Haiku'),
    systemPrompt: 'You are a helpful assistant',
    tools: [
      a.ai.dataTool({
        name: 'PostQuery',
        description: 'Searches for Post records',
        model: a.ref('Post'),
        modelOperation: 'list',
      }),
    ]
  })
  .authorization((allow) => allow.owner()),
});
```

#### 5. Configure Generative UI with `<AIConversation />`:
Render custom React UI components inside the conversation stream:

```typescript
import { AIConversation } from '@aws-amplify/ui-react-ai';

function Chat() {
  const [
    {
      data: { messages },
      isLoading,
    },
    sendMessage,
  ] = useAIConversation('chat');

  return (
    <AIConversation
      messages={messages}
      handleSendMessage={sendMessage}
      isLoading={isLoading}
      allowAttachments
      responseComponents={{
        WeatherCard: {
          description: 'Used to display the weather of a given city to the user',
          component: ({ city }) => <Card>{city}</Card>,
          props: {
            city: {
              type: 'string',
              required: true,
            },
          },
        },
      }}
    />
  );
}
```

#### 6. Production Deployment & Cleanup:
* **Production Deployment:** Push your repository to Git and connect it to the **AWS Amplify Console**. Every `git push` triggers a full continuous deployment pipeline for both frontend and backend.
* **Cleaning Up Sandbox Resources:** Press `Ctrl + C` to stop the sandbox process, then run:
  ```bash
  npx ampx sandbox delete
  ```

### CONCLUSION:

The most impressive aspect of the AWS Amplify AI Kit is its ability to abstract away Generative AI complexities into familiar web development paradigms. Integrating AI Routes directly into the Data Schema enables full-stack developers to interact with advanced LLMs on Amazon Bedrock as effortlessly as writing standard CRUD operations.

Combining Amplify Gen 2's TypeScript-first approach with Serverless architecture (AppSync, Lambda, DynamoDB) and Generative UI components provides the ultimate blueprint for shipping production-ready AI applications in minutes.

---

**Original Article Link:**  
[AWS Mobile Blog - Build Fullstack AI Apps in Minutes with the New Amplify AI Kit](https://aws.amazon.com/blogs/mobile/build-fullstack-ai-apps-in-minutes-with-the-new-amplify-ai-kit/)

**Facebook Community Post:**  
[AWS Study Group FCJ - Blog 3 Post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2223956578369302)