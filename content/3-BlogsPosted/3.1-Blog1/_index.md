---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

# BUILDING AN OFFLINE-FIRST APPLICATION WITH OPTIMISTIC UI: COMBINING AWS AMPLIFY, APPSYNC, TANSTACK QUERY, AND MONGODB ATLAS

In modern web and mobile application development, user experience (UX) relies heavily on UI responsiveness. However, unstable network connections or server latency often cause applications to display loading screens continuously, interrupting the user experience. This article guides you on building an Offline-First application combined with Optimistic UI updates using AWS Amplify Gen 2, AWS AppSync, TanStack Query, and MongoDB Atlas. This solution ensures instant data rendering even during network disconnections and automatically synchronizes data when connectivity is restored.

### KEY HIGHLIGHTS:

* **Offline-First Mindset & Optimistic UI**: Instead of waiting for a server roundtrip, the application updates the local cache immediately upon user interaction. If the API fails, the system automatically rolls back to the initial data state.
* **Asynchronous State Management with TanStack Query**: Leverages TanStack Query's caching mechanisms and network mode to manage data flows, automatically pausing or retrying requests when network connectivity is interrupted.
* **Flexible Full-Stack Serverless Architecture**: Combines AWS AppSync (GraphQL API), AWS Lambda (Resolver), Amazon Cognito (Authentication), and AWS Amplify Gen 2 to automate infrastructure and enable easy CI/CD deployment via Git-based workflows.
* **MongoDB Atlas Integration**: Uses MongoDB Atlas as a flexible cloud NoSQL database, seamlessly connected to AWS infrastructure through a Lambda Resolver.
* **Simple Conflict Resolution Mechanism**: Applies a "first-come, first-served" conflict resolution approach based on actual write order at MongoDB Atlas, suitable for applications with low concurrent data conflicts.

### REAL-WORLD SCENARIO:

A Task Management application (To-Do List) requires users to add, edit, and delete tasks continuously even while moving through weak signal areas or completely offline. The system must respond instantly on the interface without forcing users to wait for loading indicators.

The architecture for the Offline-First & Optimistic UI application is as follows:

> **User Action (React UI) → TanStack Query Cache (Instant UI Update) → AWS Amplify / AppSync (GraphQL API) → AWS Lambda Resolver → MongoDB Atlas (Database)**

![Sơ đồ kiến trúc Offline-First & Optimistic UI](/images/blog1architect.png)

In this architecture:

* **User Action & TanStack Local Cache (onMutate)**: When a user adds or edits a task, TanStack Query immediately cancels ongoing queries (`cancelQueries`), saves a snapshot of the previous data, and updates the new data directly into the cache for instant UI rendering.
* **AWS AppSync & Lambda Resolver**: Simultaneously, the application sends a Mutation request via AWS AppSync GraphQL API. AWS Lambda acts as the Resolver to handle the request and perform corresponding database operations on MongoDB Atlas.
* **Error Handling & Rollback (onError)**: If the data persistence operation fails (due to server or logic errors), TanStack Query uses the previously saved snapshot to roll back the cache to its initial state, maintaining data consistency.
* **Network Queueing & Auto-Sync (onSettled / onSuccess)**: When the device loses network connectivity, TanStack Query pauses sending mutations and queues them. As soon as the connection is restored, the queued mutations are sent, and `invalidateQueries` is triggered to re-synchronize the latest data from MongoDB Atlas.

### DEPLOYMENT GUIDE:

To deploy the application in your AWS account, follow the steps below. Once deployed, you can create a user, authenticate yourself, and create to-do entries.

1. **Set up the MongoDB Atlas Cluster:**
   * Set up the MongoDB Atlas cluster, Database, User, and Network access permissions.

2. **Set up and Configure User:**
   * Configure database user credentials and access rules.

3. **Clone the GitHub Repository:**
   * Clone the sample application using the following command:
     ```bash
     git clone https://github.com/mongodb-partners/amplify-mongodb-tanstack-offline
     ```

4. **Setup AWS CLI Credentials (Optional for local debugging):**
   * If you would like to test the application locally using a sandbox environment, configure temporary local AWS credentials:
     ```bash
     export AWS_ACCESS_KEY_ID=<your-access-key-id>
     export AWS_SECRET_ACCESS_KEY=<your-secret-access-key>
     export AWS_SESSION_TOKEN=<your-session-token>
     ```

5. **Deploy the Todo Application in AWS Amplify:**
   * Open the **AWS Amplify Console** and select the **GitHub** option.
   * Configure GitHub repository permissions and grant access.
   * Select the repository (`amplify-mongodb-tanstack-offline`) and branch, then click **Next**.
   * Leave all other settings as default and click **Deploy**.

6. **Configure Environment Variables:**
   * Configure the necessary environment variables (MongoDB connection string, AppSync keys, etc.) after successful deployment.

7. **Open and Test the Application:**
   * Open the application using the URL provided by AWS Amplify and test creating/updating To-Do items.
   * Verify the persisted data in your MongoDB Atlas cluster.

### CONCLUSION:

What is most impressive about this solution is how the author cleverly combines client-side state management (TanStack Query) with AWS Serverless services and MongoDB Atlas to optimize the end-user experience.

By incorporating Offline-First and Optimistic UI principles into the architecture, the application completely eliminates dependency on network latency for basic operations, delivering a seamless native-like feel. This is an extremely practical and valuable architectural pattern for modern Web/Mobile applications where user experience and data availability are top priorities.

---
**Original Article Link:**  
[AWS Mobile Blog - Offline Caching with AWS Amplify, TanStack, AppSync, and MongoDB Atlas](https://aws.amazon.com/blogs/mobile/offline-caching-with-aws-amplify-tanstack-appsync-and-mongodb-atlas/?fbclid=IwY2xjawTQRbBwZG9mBWV4dG4DYWVtAjEwAGJyaWQRMXA4TTZOYVBMTTBOczdNTTBzcnRjBmFwcF9pZBAyMjIwMzkxNzg4MjAwODkyAAEesu05YrTOVk9zmUd67tN6XtZMZw5fg4SRk9wIz4mZ3UxFB2eo7Cggm8k9i6o_aem_kteT-zlGgPrmMNZY1cqaiQ)