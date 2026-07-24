---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

# BUILDING REAL-TIME APPLICATIONS WITH AWS APPSYNC EVENTS: DIRECT DATA PUBLISHING VIA WEBSOCKET CONNECTION

In modern applications such as collaborative tools, live dashboards, or chat applications, real-time functionality has become a mandatory standard. AWS AppSync Events – a fully managed serverless service for WebSocket APIs – recently introduced a major enhancement: allowing direct message publishing via WebSocket connections, rather than being restricted to HTTP endpoints as before. This upgrade enables developers to use a single WebSocket connection for both sending and receiving data, significantly streamlining client-side architecture and optimizing system performance.

### KEY HIGHLIGHTS:

* **Unified Two-Way Connection (Single Connection Pub/Sub)**: Developers only need to maintain a single WebSocket connection to both subscribe to data updates and publish messages, eliminating the complexity of managing multiple client-side connections.
* **Cost Optimization & Reduced Connection Overhead**: Completely resolves the overhead of continuous HTTP handshakes for "chatty" applications with high data exchange frequency, such as Chat apps, Games, or Collaborative Tools.
* **Flexible System Architecture**: Allows choosing the optimal approach: client-side WebSocket Publishing (Web/Mobile) for smooth interaction, or HTTP Endpoints on the backend when high throughput data ingestion is required.
* **AWS CDK L2 Infrastructure as Code Integration**: Easily create and configure AppSync Event APIs using official L2 constructs from the AWS Cloud Development Kit (CDK), facilitating automated and standardized infrastructure deployment.
* **Clear Quotas & Limits**: Supports a publishing rate of up to 25 requests/second per client WebSocket connection, while HTTP endpoints continue to handle high-throughput scenarios (up to 10,000 events/second by default).

### REAL-WORLD SCENARIO:

Building a simple real-time Chat application where users can send and receive instant messages via a web interface without needing to issue separate HTTP POST calls for message sending.

Two-way data processing flow via a single WebSocket connection:

> **User Input (Frontend) → Single WebSocket (type: publish) → AWS AppSync Event API → Event Channel (/default/messages) → Broadcast via WebSocket (type: data) → Real-time UI Update**

In this architecture:

* **AppSync Event API Initialization (AWS CDK)**: Define the Event API with API Key authentication and configure a Channel Namespace named `default`.
* **Single WebSocket Connection Setup**: The client opens a `wss://...` connection to the AppSync Realtime Domain and subscribes to events on channel `/default/*` using a `type: "subscribe"` frame.
* **Direct Data Publishing via WebSocket (type: "publish")**: When a user clicks send, the client transmits a JSON payload containing content, target channel (`/default/messages`), and auth headers directly over the open WebSocket connection.
* **Real-time Broadcast**: AppSync receives the message, validates permissions, and immediately broadcasts the event to all clients subscribed to `/default/*` for instant UI updates.

### IMPLEMENTATION & DEPLOYMENT GUIDE:

#### 1. Testing on AWS AppSync Console:
Getting started with AppSync Events is simple, allowing you to publish over WebSocket or HTTP directly from the console. Creating an API automatically generates a default channel namespace and API key. On the Pub/Sub Editor, select the **Publish** button and choose **WebSocket** in the dropdown to publish events directly over WebSocket.

**Message Format:**
To publish events over WebSocket, send a JSON payload specifying `type: "publish"`, a unique `id`, target `channel`, an array of `events` (up to 5), and `authorization` headers:

```json
{
  "type": "publish",
  "id": "an-identifier-for-this-request",
  "channel": "/namespace/my/path",
  "events": [ "{ \"msg\": \"Hello World!\" }" ],
  "authorization": {
    "x-api-key": "da2-12345678901234567890123456-example"
  }
}
```
Upon publishing, you will receive a `publish_success` response with details for each event sent, or a `publish_error` response if the operation fails.

---

#### 2. Integrating with a Real-Time Application:

##### Step 1: Install AWS CDK CLI & Initialize Project
Install the CDK CLI (if not already installed) and initialize the CDK app:
```bash
npm install -g aws-cdk
mkdir -p events-app/cdk-events-publish
cd events-app/cdk-events-publish
cdk init app --language javascript
```

##### Step 2: Define AppSync Event API with AWS CDK L2 Constructs
Update `lib/cdk-events-publish-stack.js` with the following implementation:
```javascript
const { Stack, CfnOutput } = require('aws-cdk-lib');
const { EventApi, AppSyncAuthorizationType } = require('aws-cdk-lib/aws-appsync');

class CdkEventsPublishStack extends Stack {
  constructor(scope, id, props) {
    super(scope, id, props);
    const apiKeyProvider = { authorizationType: AppSyncAuthorizationType.API_KEY };

    // Create an API called `my-event-api` using API Key authorization
    const api = new EventApi(this, 'api', {
      apiName: 'my-event-api',
      authorizationConfig: { authProviders: [apiKeyProvider] }
    });

    // Add a channel namespace called `default`
    api.addChannelNamespace('default');

    // Output configuration properties
    new CfnOutput(this, 'apiKey', { value: api.apiKeys['Default'].attrApiKey });
    new CfnOutput(this, 'httpDomain', { value: api.httpDns });
    new CfnOutput(this, 'realtimeDomain', { value: api.realtimeDns });
  }
}

module.exports = { CdkEventsPublishStack }
```

##### Step 3: Deploy the CDK Stack
Deploy the stack and output configuration to `output.json`:
```bash
npm run cdk deploy -- -O output.json
```
Output will look like:
```text
Outputs:
CdkStack.apiKey = da2-12345678901234567890123456-example
CdkStack.httpDomain = a12345678901234567890123456.appsync-api.us-east-2.amazonaws.com
CdkStack.realtimeDomain = a12345678901234567890123456.appsync-realtime-api.us-east-2.amazonaws.com
```

##### Step 4: Create Frontend Web App (Vite Vanilla JS)
From the `events-app` folder, generate a web application:
```bash
npm create vite@latest app -- --template vanilla
cd app
npm install
ln -s ../../cdk-events-publish/output.json src
```

##### Step 5: Configure Frontend Application (`src/main.js`)
Replace `src/main.js` with the code below to open a WebSocket connection, subscribe to `/default/*`, and publish messages over the active WebSocket connection when submitting the form:
```javascript
import './style.css'
import output from './output.json'

// Use output from CDK stack deployment
const HTTP_DOMAIN = output.CdkEventsPublishStack.httpDomain
const REALTIME_DOMAIN = output.CdkEventsPublishStack.realtimeDomain
const API_KEY = output.CdkEventsPublishStack.apiKey 
     
const authorization = { 'x-api-key': API_KEY, host: HTTP_DOMAIN }

document.querySelector('#app').innerHTML = `
    <style>
      #top{width: calc(100vw - 8rem); max-width: 1280px; background: oklch(0.977 0.013 236.62); margin: 2rem 0; height: calc(100vh - 8rem); box-shadow: inset 0 0 20px rgba(0,0,0,0.1); position: relative; margin-inline: auto;}
      #container{display: flex;flex-direction: column;height: calc(100% - 6.5rem);}
      h2{color: oklch(0.3 0.013 236.62); margin-bottom: 1.5em; font-size: 1.8rem;}
      #messages{display: flex; flex-direction: column-reverse; gap: 1em; max-height: calc(100vh - 200px); flex: 1; min-height: 0; overflow-y: auto; text-align: left;}
      form{position: absolute; bottom: 2rem; left: 2rem; right: 2rem; padding: 1rem 0;}
      input{width: 90%; padding: 0.8em 1.2em; border: 1px solid oklch(0.8 0.013 236.62); border-radius: 25px; font-size: 1rem; outline: none; transition: all 0.2s ease; box-shadow: 0 2px 5px rgba(0,0,0,0.05);}
      .msg{padding: 0 1rem; font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, 'Liberation Mono', 'Courier New', monospace;}
    </style>
    <div id="top">
      <div id="container"><h2>Messages</h2><div id="messages"></div></div>
      <form id="form"> <input id="messageInput" name="message" type="text" autocomplete="off" /></form>
    </div>`

// Construct protocol header for WebSocket authentication
function getAuthProtocol() {
  const header = btoa(JSON.stringify(authorization))
    .replace(/\+/g, '-')
    .replace(/\//g, '_')
    .replace(/=+$/, '')
  return `header-${header}`
}

const socket = await new Promise((resolve, reject) => {
  const socket = new WebSocket(`wss://${REALTIME_DOMAIN}/event/realtime`, [
    'aws-appsync-event-ws',
    getAuthProtocol(),
  ])
  socket.onopen = () => resolve(socket)
  socket.onclose = (event) => reject(new Error(event.reason))
  socket.onmessage = (_evt) => {
    const data = JSON.parse(_evt.data)
    // Add message to chat display when data event arrives
    if (data.type === 'data') {
      const event = JSON.parse(data.event)
      const div = document.createElement('div')
      div.className = 'msg'
      div.innerHTML = `↑ ${event.time} | ↓ ${new Date().toISOString().split('T')[1]} | ${event.message}`
      messages.prepend(div)
    }
  }
})

// Subscribe to `/default/*`
socket.send(
  JSON.stringify({
    type: 'subscribe',
    id: crypto.randomUUID(),
    channel: '/default/*',
    authorization,
  }),
)

const form = document.querySelector('#form')
const messageInput = document.querySelector('#messageInput')
const messages = document.querySelector('#messages')

// Send event to `/default/messages` via WebSocket on form submit
form.addEventListener('submit', (e) => {
  e.preventDefault()
  const message = new FormData(e.currentTarget).get('message')
  messageInput.value = ''
  socket.send(
    JSON.stringify({
      type: 'publish',
      id: crypto.randomUUID(),
      channel: '/default/messages',
      events: [JSON.stringify({ message, time: new Date().toISOString().split('T')[1] })],
      authorization,
    }),
  )
})
```

##### Step 6: Run Application
Start dev server in `app` directory:
```bash
npm run dev
```

---

#### 3. Cleaning Up:
Remove deployed resources with CDK when finished:
```bash
cd ../cdk-events-publish
npm run cdk destroy
```

### CONCLUSION:

The most impressive aspect of this AWS AppSync Events upgrade is the simplified Developer Experience. Eliminating the requirement to maintain parallel HTTP endpoints (for sending) and WebSockets (for receiving) makes client-side handling much cleaner, less error-prone, and significantly smoother.

Combining Serverless WebSockets with fully managed Pub/Sub capabilities from AWS allows engineering teams to rapidly deliver real-time features without worrying about operating or scaling complex WebSocket clusters.

---
**Original Article Link:**  
[AWS Mobile Blog - Building Real-Time Apps with AWS AppSync Events WebSocket Publishing](https://aws.amazon.com/blogs/mobile/building-real-time-apps-with-aws-appsync-events-websocket-publishing/?content_source=fb&fb_content_id=Q9-wBQFwx5Yd66n6jtrlxpOWP3d7Ai1Z7Qhlr_NhxwMZQM8H3rgdqa3L5yjLiJ_zJw&channel_type=fb&fbclid=IwY2xjawTQRshhZmRrCVJvUEdfVmNxaXBkb2YFZXh0bgNhZW0CMTEAc3J0YwZhcHBfaWQQMjIyMDM5MTc4ODIwMDg5MgABHopepW3Rg8DlwlNQTVRRSIliLvuErLGNkjdiOoVSIM-HwlueXudnHeYSKZoQ_aem_exW5yGY1VG2nNo6i6_eNQw)
