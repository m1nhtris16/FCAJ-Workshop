---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

# XÂY DỰNG ỨNG DỤNG REAL-TIME VỚI AWS APPSYNC EVENTS: XUẤT BẢN DỮ LIỆU TRỰC TIẾP QUA KẾT NỐI WEBSOCKET

Trong các ứng dụng hiện đại như công cụ cộng tác, bảng điều khiển trực tiếp (live dashboard) hay ứng dụng trò chuyện, tính năng thời gian thực (real-time) đã trở thành tiêu chuẩn bắt buộc. AWS AppSync Events – dịch vụ serverless quản lý hoàn toàn cho WebSocket API – vừa ra mắt cải tiến quan trọng: cho phép xuất bản thông điệp (Publish) trực tiếp qua kết nối WebSocket, thay vì chỉ giới hạn gửi qua endpoint HTTP như trước đây. Nâng cấp này giúp lập trình viên sử dụng một kết nối WebSocket duy nhất cho cả hai chiều gửi và nhận dữ liệu, tinh giản đáng kể kiến trúc phía Client và tối ưu hiệu năng cho hệ thống.

### NHỮNG ĐIỂM NỔI BẬT:

* **Kết nối hai chiều đồng nhất (Single Connection Pub/Sub)**: Lập trình viên chỉ cần duy trì một kết nối WebSocket duy nhất để vừa đăng ký nhận dữ liệu (Subscribe), vừa xuất bản tin nhắn (Publish), loại bỏ sự phức tạp khi phải quản lý nhiều loại kết nối phía Client.
* **Tối ưu hóa chi phí & Giảm overhead kết nối**: Giải quyết triệt để bài toán chi phí khởi tạo kết nối HTTP handshakes liên tục đối với các ứng dụng có tần suất trao đổi dữ liệu dày đặc ("chatty" applications) như ứng dụng Chat, Game hay Collaborative Tools.
* **Linh hoạt theo kiến trúc hệ thống**: Cho phép lựa chọn phương thức tối ưu: dùng WebSocket Publishing phía Client (Web/Mobile) để tương tác mượt mà, hoặc dùng HTTP Endpoint ở phía Backend khi cần đẩy dữ liệu với lưu lượng lớn (high throughput).
* **Tích hợp hạ tầng mã nguồn AWS CDK L2**: Dễ dàng khởi tạo và cấu hình AppSync Event API thông qua các L2 construct chính thức từ AWS Cloud Development Kit (CDK), giúp triển khai hạ tầng tự động và chuẩn hóa.
* **Hạn mức & Quota rõ ràng**: Hỗ trợ tốc độ xuất bản 25 request/giây trên mỗi kết nối WebSocket của client; trong khi HTTP endpoint tiếp tục đáp ứng các kịch bản lưu lượng cực cao (mặc định lên tới 10,000 events/giây).

### TÌNH HUỐNG THỰC TẾ:

Xây dựng một ứng dụng trò chuyện trực tuyến (Chat App) đơn giản, nơi người dùng có thể gửi và nhận tin nhắn tức thì qua giao diện web mà không cần thiết lập thêm bất kỳ cuộc gọi HTTP POST độc lập nào để gửi tin.

Luồng xử lý dữ liệu hai chiều qua kết nối WebSocket duy nhất:

> **User Input (Frontend) → Single WebSocket (type: publish) → AWS AppSync Event API → Event Channel (/default/messages) → Broadcast via WebSocket (type: data) → Real-time UI Update**

Trong kiến trúc này:

* **Khởi tạo AppSync Event API (AWS CDK)**: Định nghĩa Event API với cơ chế xác thực API Key và cấu hình một Channel Namespace tên là `default`.
* **Thiết lập kết nối WebSocket duy nhất**: Client mở kết nối `wss://...` tới AppSync Realtime Domain, đồng thời đăng ký lắng nghe sự kiện trên kênh `/default/*` bằng thông điệp `type: "subscribe"`.
* **Xuất bản dữ liệu trực tiếp qua WebSocket (type: "publish")**: Khi người dùng bấm gửi tin nhắn, Client gửi khung dữ liệu JSON chứa nội dung, kênh đích (`/default/messages`) và header xác thực qua chính kết nối WebSocket đang mở.
* **Phát sóng real-time (Real-time Broadcast)**: AppSync tiếp nhận thông điệp, xác thực quyền và lập tức phân phối sự kiện tới toàn bộ các Client đang đăng ký kênh `/default/*` để cập nhật giao diện tức thì.

### HƯỚNG DẪN THỰC HIỆN VÀ TRIỂN KHAI (GETTING STARTED & IMPLEMENTATION):

#### 1. Thử nghiệm trên AWS AppSync Console:
Bạn có thể dễ dàng bắt đầu với AppSync Events trực tiếp từ console. Khi tạo một API, hệ thống tự động cung cấp kênh namespace mặc định kèm API key. Trên trình biên tập Pub/Sub Editor, chọn nút **Publish** và chọn tùy chọn **WebSocket** để thử nghiệm xuất bản thông điệp trực tiếp.

**Cấu trúc dữ liệu thông điệp (Message Format):**
Để xuất bản sự kiện qua WebSocket, Client gửi một thông điệp định dạng JSON chứa `type: "publish"`, mã định danh `id`, kênh đích `channel`, mảng danh sách sự kiện `events` (tối đa 5 sự kiện) và thông tin xác thực `authorization`:

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
Sau khi gửi, bạn sẽ nhận được phản hồi `publish_success` chi tiết cho từng sự kiện hoặc `publish_error` nếu có lỗi xảy ra.

---

#### 2. Tích hợp vào ứng dụng Chat Real-Time:

##### Bước 1: Cài đặt và khởi tạo dự án AWS CDK
Cài đặt CDK CLI (nếu chưa có) và khởi tạo dự án CDK:
```bash
npm install -g aws-cdk
mkdir -p events-app/cdk-events-publish
cd events-app/cdk-events-publish
cdk init app --language javascript
```

##### Bước 2: Định nghĩa Hạ tầng AppSync Event API với AWS CDK L2 Constructs
Cập nhật nội dung tệp `lib/cdk-events-publish-stack.js`:
```javascript
const { Stack, CfnOutput } = require('aws-cdk-lib');
const { EventApi, AppSyncAuthorizationType } = require('aws-cdk-lib/aws-appsync');

class CdkEventsPublishStack extends Stack {
  constructor(scope, id, props) {
    super(scope, id, props);
    const apiKeyProvider = { authorizationType: AppSyncAuthorizationType.API_KEY };

    // Khởi tạo Event API có tên `my-event-api` sử dụng xác thực API Key
    const api = new EventApi(this, 'api', {
      apiName: 'my-event-api',
      authorizationConfig: { authProviders: [apiKeyProvider] }
    });

    // Thêm channel namespace tên là `default`
    api.addChannelNamespace('default');

    // Xuất các giá trị đầu ra (Outputs)
    new CfnOutput(this, 'apiKey', { value: api.apiKeys['Default'].attrApiKey });
    new CfnOutput(this, 'httpDomain', { value: api.httpDns });
    new CfnOutput(this, 'realtimeDomain', { value: api.realtimeDns });
  }
}

module.exports = { CdkEventsPublishStack }
```

##### Bước 3: Triển khai CDK Stack
Triển khai hạ tầng lên AWS và xuất kết quả ra file `output.json`:
```bash
npm run cdk deploy -- -O output.json
```
Kết quả trả về sẽ có dạng:
```text
Outputs:
CdkStack.apiKey = da2-12345678901234567890123456-example
CdkStack.httpDomain = a12345678901234567890123456.appsync-api.us-east-2.amazonaws.com
CdkStack.realtimeDomain = a12345678901234567890123456.appsync-realtime-api.us-east-2.amazonaws.com
```

##### Bước 4: Khởi tạo ứng dụng Frontend (Vite Vanilla JS)
Từ thư mục `events-app`, tạo ứng dụng web frontend:
```bash
npm create vite@latest app -- --template vanilla
cd app
npm install
ln -s ../../cdk-events-publish/output.json src
```

##### Bước 5: Cấu hình mã nguồn Frontend (`src/main.js`)
Thay thế toàn bộ nội dung `src/main.js` để kết nối WebSocket, đăng ký nhận tin nhắn trên `/default/*` và xuất bản tin nhắn trực tiếp qua kết nối WebSocket khi bấm Gửi:
```javascript
import './style.css'
import output from './output.json'

// Sử dụng thông tin từ output CDK Stack
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

// Cấu hình Protocol Header cho kết nối WebSocket
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
    // Khi nhận được sự kiện `data`, thêm tin nhắn vào danh sách
    if (data.type === 'data') {
      const event = JSON.parse(data.event)
      const div = document.createElement('div')
      div.className = 'msg'
      div.innerHTML = `↑ ${event.time} | ↓ ${new Date().toISOString().split('T')[1]} | ${event.message}`
      messages.prepend(div)
    }
  }
})

// Đăng ký (Subscribe) kênh `/default/*`
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

// Khi submit form, xuất bản (Publish) trực tiếp sự kiện qua WebSocket tới `/default/messages`
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

##### Bước 6: Chạy thử ứng dụng
Trong thư mục `app`, khởi chạy máy chủ phát triển local:
```bash
npm run dev
```
Mở đường dẫn local trên nhiều trình duyệt khác nhau để kiểm tra tính năng gửi và nhận tin nhắn thời gian thực qua duy nhất 1 kết nối WebSocket.

---

#### 3. Dọn dẹp tài nguyên (Cleaning Up):
Khi hoàn thành thử nghiệm, hủy các tài nguyên đã tạo bằng lệnh CDK:
```bash
cd ../cdk-events-publish
npm run cdk destroy
```

### KẾT LUẬN:

Điểm ấn tượng nhất ở nâng cấp này của AWS AppSync Events là sự tinh giản trải nghiệm phát triển (Developer Experience). Việc loại bỏ rào cản phải duy trì song song cả HTTP endpoint (cho chiều gửi) và WebSocket (cho chiều nhận) giúp luồng xử lý ở phía Client trở nên gọn gàng, ít lỗi và mượt mà hơn rất nhiều.

Sự kết hợp giữa Serverless WebSocket và cơ chế Pub/Sub quản lý hoàn toàn từ AWS giúp các đội ngũ kỹ thuật dễ dàng đưa các tính năng thời gian thực vào sản phẩm chỉ trong thời gian ngắn mà không phải bận tâm về việc vận hành hay mở rộng hạ tầng cụm WebSocket phức tạp.

---
**Link tài liệu gốc:**  
[AWS Mobile Blog - Building Real-Time Apps with AWS AppSync Events WebSocket Publishing](https://aws.amazon.com/blogs/mobile/building-real-time-apps-with-aws-appsync-events-websocket-publishing/?content_source=fb&fb_content_id=Q9-wBQFwx5Yd66n6jtrlxpOWP3d7Ai1Z7Qhlr_NhxwMZQM8H3rgdqa3L5yjLiJ_zJw&channel_type=fb&fbclid=IwY2xjawTQRshhZmRrCVJvUEdfVmNxaXBkb2YFZXh0bgNhZW0CMTEAc3J0YwZhcHBfaWQQMjIyMDM5MTc4ODIwMDg5MgABHopepW3Rg8DlwlNQTVRRSIliLvuErLGNkjdiOoVSIM-HwlueXudnHeYSKZoQ_aem_exW5yGY1VG2nNo6i6_eNQw)
