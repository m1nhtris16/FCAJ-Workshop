---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

# XÂY DỰNG ỨNG DỤNG AI FULL-STACK TRONG VỎN VẸN VÀI PHÚT VỚI AWS AMPLIFY AI KIT

Trong kỷ nguyên Generative AI, việc tích hợp các tính năng thông minh như Chatbot, tìm kiếm hội thoại hay tóm tắt văn bản vào ứng dụng web/mobile thường đòi hỏi lập trình viên phải có kiến thức chuyên sâu về hạ tầng đám mây, quy trình xử lý luồng dữ liệu (pipeline) và quản lý mô hình Machine Learning. AWS Amplify AI Kit ra đời nhằm xóa bỏ hoàn toàn rào cản này, giúp các lập trình viên Full-Stack xây dựng tính năng GenAI nhanh chóng thông qua ngôn ngữ TypeScript quen thuộc trên nền tảng Serverless của AWS.

### NHỮNG ĐIỂM NỔI BẬT:

* **Tích hợp AI Route chuẩn TypeScript (TypeScript-First AI Routes)**: Dựa trên nền tảng Amplify Gen 2, lập trình viên có thể định nghĩa các đường dẫn AI (AI Routes) trực tiếp trong Schema dữ liệu backend bằng TypeScript. Hỗ trợ 2 dạng chính: *Conversation* (trò chuyện đa lượt real-time, tự động lưu lịch sử) và *Generation* (sinh dữ liệu có cấu trúc theo yêu cầu).
* **Trải nghiệm Type-Safe & Tích hợp sẵn React Hooks/UI Components**: Cung cấp sự đồng bộ kiểu dữ liệu tuyệt đối giữa Backend và Frontend. Bộ thư viện `@aws-amplify/ui-react-ai` mang đến các React Hooks (`useAIConversation`, `useAIGeneration`) và Component dựng sẵn `<AIConversation/>` giúp thiết lập giao diện chat real-time chỉ với vài dòng code.
* **Cơ chế "Generative AI as Data" (Data Tools)**: Cho phép mô hình ngôn ngữ lớn (LLM) trên Amazon Bedrock truy vấn trực tiếp vào các mô hình dữ liệu (Data Models) của ứng dụng thông qua công cụ (`a.ai.dataTool`). Mọi thao tác truy vấn của AI đều tuân thủ nghiêm ngặt cơ chế phân quyền theo từng người dùng (Owner-based Authorization).
* **Hỗ trợ Generative UI (Giao diện động do AI sinh ra)**: AI không chỉ phản hồi bằng văn bản thuần túy mà có thể trả về trực tiếp các thành phần giao diện React (`responseComponents`) được định nghĩa sẵn (ví dụ: `WeatherCard`, `RecipeCard`) để hiển thị mượt mà trên khung chat.
* **Kiến trúc Serverless & Quy trình phát triển Sandbox tốc độ**: Hệ thống tự động mở rộng theo lưu lượng thực tế (pay-per-use). Lập trình viên có thể sử dụng môi trường `ampx sandbox` để thử nghiệm các thay đổi backend trên đám mây theo thời gian thực mà không làm gián đoạn môi trường chính.

### TÌNH HUỐNG THỰC TẾ:

Một lập trình viên muốn xây dựng ứng dụng Full-Stack hỗ trợ Trợ lý AI có khả năng trò chuyện trực tiếp, tra cứu thông tin bài viết (Post) riêng tư của người dùng, đồng thời trả về kết quả hiển thị dưới dạng thẻ giao diện React (Generative UI) mượt mà.

Kiến trúc triển khai ứng dụng AI Full-Stack với Amplify AI Kit như sau:

> **User Interaction (React UI / `<AIConversation/>`) → AWS AppSync (GraphQL & Auth Check) → AWS Lambda (Orchestrator) → Amazon Bedrock (Claude 3.5 Sonnet / Streaming API) → Amazon DynamoDB (Chat History & App Data)**

![Sơ đồ kiến trúc AI Full-Stack với AWS Amplify AI Kit](/images/blog3architect.png)

Trong kiến trúc này:

* **Frontend & AppSync API Gateway**: Người dùng gửi câu hỏi hoặc yêu cầu từ giao diện React. AWS AppSync tiếp nhận yêu cầu, xác thực danh tính người dùng (Cognito User Pool) và đảm bảo tính bảo mật trước khi chuyển tiếp.
* **AWS Lambda (Orchestrator / Cầu nối)**: Lambda đóng vai trò trung tâm điều phối, lấy lịch sử trò chuyện từ Amazon DynamoDB, xây dựng context và gọi Streaming Converse API của Amazon Bedrock.
* **Amazon Bedrock & Data Tools Execution**: Mô hình LLM (như Anthropic Claude 3.5 Sonnet) phân tích tin nhắn. Nếu cần tra cứu dữ liệu người dùng, Bedrock sẽ gọi lại Data Tool (`PostQuery`) thông qua AppSync để lấy đúng các bản ghi Post mà người dùng đó sở hữu.
* **Realtime Streaming & Generative UI Render**: Phản hồi từ Bedrock được đẩy ngược theo dạng luồng (stream) về Client qua AppSync. Giao diện React hiển thị câu trả lời ngay lập tức cùng với các React Component tương ứng (Generative UI), đồng thời lưu vết hội thoại vào DynamoDB.

### HƯỚNG DẪN THỰC HIỆN VÀ TRIỂN KHAI (GETTING STARTED & IMPLEMENTATION):

#### 1. Thiết lập ban đầu (Setup & Sandbox):
* Bạn cần có một tài khoản AWS đã được bật quyền truy cập vào các Foundation Model (như Anthropic Claude 3.5 Sonnet hoặc Claude 3 Haiku) trong Amazon Bedrock console.
* Trong thư mục dự án Frontend (Next.js hoặc Vite), khởi tạo Amplify backend bằng lệnh:
  ```bash
  npm create amplify@latest
  ```
* Khởi chạy môi trường đám mây cá nhân (Cloud Sandbox) để thử nghiệm real-time:
  ```bash
  npx ampx sandbox
  ```

#### 2. Thêm tính năng AI vào Schema (`amplify/data/resource.ts`):
Trong Amplify Gen 2, bạn định nghĩa các đường dẫn AI (AI Routes) ngay trong schema dữ liệu:

```typescript
import { a, defineData, type ClientSchema } from '@aws-amplify/backend';

const schema = a.schema({
  // AI Route dạng Conversation: Trò chuyện đa lượt real-time
  chat: a.conversation({
    aiModel: a.ai.model('Claude 3 Haiku'),
    systemPrompt: 'You are a helpful assistant',
  })
  .authorization((allow) => allow.owner()),

  // AI Route dạng Generation: Sinh dữ liệu có cấu trúc theo yêu cầu
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

#### 3. Tích hợp Type-Safe Client & React Hooks ở Frontend:
Sử dụng bộ công cụ `@aws-amplify/ui-react-ai` để kết nối với backend với sự hỗ trợ kiểu dữ liệu (type-safe) hoàn hảo:

```typescript
import { generateClient } from "aws-amplify/api";
import { Schema } from "../amplify/data/resource";
import { createAIHooks } from "@aws-amplify/ui-react-ai";

const client = generateClient<Schema>();
const { useAIGeneration, useAIConversation } = createAIHooks(client);

// Component Trò chuyện AI
function Chat() {
  const [
    {
      data: { messages },
      isLoading,
      hasError,
    },
    sendMessage,
  ] = useAIConversation('chat');
  
  // Render UI Chat...
}

// Component Sinh Công thức
function RecipeGenerator() {
   const [{ data, isLoading }, handleGenerate] = useAIGeneration('generateRecipe');
   // Render UI Recipe...
}
```

#### 4. Cơ chế Generative AI as Data (Data Tools):
Để cho phép AI truy vấn dữ liệu trong ứng dụng (ví dụ: tìm kiếm danh sách `Post` riêng tư của người dùng), bạn gắn `dataTool` vào AI route:

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

#### 5. Cấu hình Generative UI với Component `<AIConversation />`:
Cho phép AI phản hồi bằng các React UI Component trực quan:

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

#### 6. Triển khai Production & Dọn dẹp:
* **Triển khai Production:** Đẩy dự án lên Git và kết nối với **AWS Amplify Console**. Mỗi khi có lệnh `git push`, Amplify sẽ tự động kích hoạt pipeline CI/CD để deploy cả Backend và Frontend.
* **Dọn dẹp tài nguyên thử nghiệm:** Nhấn `Ctrl + C` để dừng lệnh sandbox, sau đó chạy lệnh:
  ```bash
  npx ampx sandbox delete
  ```

### KẾT LUẬN:

Điểm ấn tượng nhất ở AWS Amplify AI Kit là khả năng trừu tượng hóa hoàn toàn sự phức tạp của Generative AI thành các khái niệm lập trình web vô cùng quen thuộc. Việc đưa AI Route trở thành một phần tích hợp sâu trong Data Schema giúp lập trình viên Full-Stack thao tác với các mô hình LLM tiên tiến nhất trên Amazon Bedrock dễ dàng như khi viết các câu lệnh CRUD cơ bản.

Sự kết hợp giữa tư duy TypeScript-first của Amplify Gen 2, kiến trúc Serverless (AppSync, Lambda, DynamoDB) và các thành phần UI thông minh (Generative UI) chính là công thức tối ưu giúp các doanh nghiệp và startup đưa các ứng dụng AI đột phá từ ý tưởng ra thị trường chỉ trong thời gian tính bằng phút.

---
**Link tài liệu gốc:**  
[AWS Mobile Blog - Build Fullstack AI Apps in Minutes with the New Amplify AI Kit](https://aws.amazon.com/blogs/mobile/build-fullstack-ai-apps-in-minutes-with-the-new-amplify-ai-kit/)