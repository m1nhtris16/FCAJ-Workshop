---
title: "Event 3"
date: 2026-07-25
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

# Báo cáo tóm tắt: “AWS Agentic AI Build Week - Pitching & Demo Day Showcase”

### Mục Tiêu Của Sự Kiện

- **Trình diễn các sản phẩm AI Agent thực tế**: Đánh giá và tôn vinh các giải pháp được xây dựng trong suốt tuần lễ Hackathon AWS Agentic AI Build Week (AABW).
- **Ứng dụng kiến trúc Agentic AI trên AWS**: Khám phá cách các đội thi áp dụng **Amazon Bedrock AgentCore**, **Strands Agent**, **Model Context Protocol (MCP)**, **Serverless** và các mô hình đa tác tử (Multi-Agent System) để giải quyết các bài toán kinh doanh phức tạp.
- **Tối ưu hóa chi phí & hiệu năng**: Phân tích bài toán chi phí hạ tầng Cloud, độ trễ xử lý (latency) và khả năng mở rộng (scalability) khi đưa các giải pháp Generative AI / Agentic AI vào sản xuất.
- **Giao lưu & Kết nối cộng đồng**: Tạo không gian kết nối giữa các kỹ sư AI, Solution Architects, các đội ngũ phát triển trẻ và chuyên gia AWS.

### Danh Sách Đội Thi & Đề Tài Trình Bày (Showcase)

- **Đội Signal Scout** – *Đề tài:* **Signal Scout** (Hệ thống phát hiện sớm tín hiệu thay đổi chiến lược doanh nghiệp dựa trên Agentic AI)
- **Đội Plan V** – *Đề tài:* **Solution Architect Professional AI Native App** (Ứng dụng AI Native hỗ trợ tự động hóa thiết kế kiến trúc đám mây & báo giá cho Solution Architect)
- **Đội 3KA** – *Đề tài:* **S.H.E.P.H.E.R.D** (Hệ thống giám sát, phân tích lưu lượng đám đông và cảnh báo ùn tắc thời gian thực bằng Computer Vision & Agentic AI)
- **Đội One Team (AWS Track Winner 🏆)** – *Đề tài:* **KFC Bot Agent** (Hệ thống đặt hàng hội thoại đa kênh AI-Powered Conversation Ordering)

---

### Nội Dung Nổi Bật

#### 1. Signal Scout - Hệ thống phân tích tín hiệu chiến lược doanh nghiệp tự động

- **Bài toán kinh doanh**: Việc theo dõi các thay đổi chiến lược của đối thủ hoặc đối tác (tái cấu trúc, thay đổi nhân sự cấp cao, biến động báo cáo tài chính) thường bị phân tán ở nhiều nguồn web công khai, mất nhiều thời gian tổng hợp thủ công.
- **Giải pháp & Tính năng**:
  - Tự động thu thập, kiểm chứng bằng chứng (evidence) và phát hiện sớm các tín hiệu thay đổi chiến lược.
  - Phân tích chỉ số tài chính, vận hành để đưa ra các kịch bản dự báo (scenarios).
  - Trình bày kết quả trực quan trên Executive Dashboard với các kết luận được minh chứng bằng dữ liệu gốc.
- **Đối tác & Công cụ tích hợp**: AWS, Langfuse (LLM Observability & Tracing), TinyFish và Apify (Scraping & Web Crawling).
- **Kiến trúc Agentic AI**:
  - Sử dụng **AgentCore Runtime** & **AgentCore Memory** (Short-Term Memory) kết hợp **Strands Agent**.
  - Mô hình Multi-Agent chia làm 2 Subagent chính: **Crawler Subagent** (giao tiếp với TinyFish/Apify) và **Analysis Subagent** (kết nối Bedrock Guardrails, Lambda và Langfuse).
  - Đề xuất kiến trúc tối ưu chi phí hơn bằng cách tận dụng **AgentCore Gateway** kết hợp với **MCP (Model Context Protocol)** cho WebSearch tool và Browser tool.
- **Đánh giá chi phí**: Tổng chi phí vận hành ước tính từ **$81 đến $359/tháng** (trong đó chi phí AWS chỉ khoảng **$17 - $130/tháng**, giúp doanh nghiệp dễ dàng tiếp cận).

#### 2. Solution Architect Professional AI Native App (Plan V) - Tự động hóa công việc cho Solution Architect

- **Thách thức thực tế**: Khi khách hàng yêu cầu thiết kế hệ thống AI gấp, các Solution Architect (SA) phải tốn rất nhiều thời gian đọc từng dòng tài liệu yêu cầu (BRD/PRD), vẽ sơ đồ kiến trúc thủ công từ trang giấy trắng, tự viết code IaC (Terraform) và tính toán chi phí dựa trên kinh nghiệm.
- **Giải pháp AI Native App**:
  - Trích xuất yêu cầu từ văn bản ngôn ngữ tự nhiên và tài liệu dự án có cấu trúc.
  - Tự động đề xuất các tùy chọn kiến trúc (hỗ trợ Hybrid-Cloud) tuân thủ tiêu chuẩn của doanh nghiệp.
  - Tự động sinh ra sơ đồ kiến trúc dạng **Draw.io** (có thể chỉnh sửa trực tiếp) và sơ đồ sử dụng bộ biểu tượng chuẩn **AWS Architecture Icons**.
  - Dự toán chi phí AWS theo thời gian thực cho khu vực `ap-southeast-1`.
  - Phát hiện các điểm thiếu sót trong yêu cầu (requirement gaps) và cho phép tinh chỉnh tương tác qua chat sidebar.
- **Quy trình hoạt động & Kiến trúc kỹ thuật**:
  - Khách hàng upload tài liệu / nhắn tin -> Hệ thống truy vấn **Knowledge Base** (Internal Docs, Architecture References) + **Amazon Bedrock** + **Draw.io MCP** + **AWS Pricing MCP** -> Xuất kết quả (Bảng tổng hợp yêu cầu, Sơ đồ kiến trúc, Bảng ước tính chi phí, Mã nguồn Terraform IaC).
  - Hạ tầng AWS Serverless & Container: Triển khai bằng Terraform, S3, CloudFront, Cognito, WAF, Application Load Balancer, VPC (Public/Private Subnets), **ECS Fargate** (Backend & Agent containers), EFS, PostgreSQL và Amazon Bedrock.
- **Tác động mang lại**: Rút ngắn thời gian lập hồ sơ đề xuất kiến trúc từ hàng ngày xuống chỉ còn vài phút; tạo ngay bản phác thảo đầu tiên chính xác thay vì bắt đầu từ trang giấy trắng.

#### 3. S.H.E.P.H.E.R.D (Team 3KA) - Giám sát & phân tích lưu lượng đám đông bằng Computer Vision và AI Agent

- **Bối cảnh dự án**: Ban đầu là dự án tốt nghiệp (Capstone Project), nhóm 3KA đã đưa vào thử nghiệm thực tế tại AABW Hackathon để kiểm chứng ý tưởng trong 24 giờ.
- **Vấn đề cần giải quyết**: Nhân viên vận hành tại các trung tâm thương mại, sự kiện lớn hay sân bay khó có thể giám sát thủ công cùng lúc nhiều lối ra vào, hàng chờ và khu vực tập trung đông người, dễ dẫn đến phản ứng chậm khi xảy ra ùn tắc hoặc sự cố.
- **Kiến trúc giải pháp**:
  - **Tầng xử lý hình ảnh (Computer Vision)**: Tận dụng **YOLO + ByteTrack** để phát hiện và theo dõi chuyển động của con người theo thời gian thực, chạy trên **Amazon SageMaker** và **Kinesis Video Streams**.
  - **Tầng Agentic AI**:
    - *Autonomous Monitor:* Tự động theo dõi chỉ số độ đậm đặc đám đông, phát hiện dấu hiệu ùn tắc, dự báo áp lực quá tải và phát cảnh báo chủ động.
    - *Operator Copilot:* Cho phép nhân viên vận hành truy vấn bằng ngôn ngữ tự nhiên (ví dụ: *"Khu vực gian hàng A có bị ùn tắc không?"*) và nhận câu trả lời kèm hành động đề xuất.
- **Thử thách thực tế trong 24h Hackathon**: Xử lý độ trễ video live stream, duy trì định danh (tracking) giữa các khung hình, tối ưu chi phí AWS, thức đêm 3h sáng để sửa lỗi code, sự cố lỡ push nhầm file `.env` lên GitHub và bài học về tinh thần đồng đội.

#### 4. KFC Bot Agent (One Team - Quán quân AWS Track 🏆) - Hệ thống đặt hàng hội thoại đa kênh

- **Điểm khởi nguồn (The Trigger)**: Bài học từ việc McDonald's phải dừng thử nghiệm AI drive-thru tại hơn 100 cửa hàng ở Mỹ cho thấy: Đặt hàng bằng AI không chỉ là trò chuyện thông thường mà là **một bài toán hệ thống phức tạp** (phải hiểu đúng món, số lượng, tùy chọn đi kèm, quy tắc voucher, trạng thái giỏ hàng và xử lý lỗi).
- **Nỗi đau khách hàng (The Problem)**: Khách hàng đang nhắn tin trò chuyện trên Zalo/Messenger nếu phải chuyển sang app khác để đặt đồ ăn sẽ bị mất đà (lost momentum), tăng ma sát người dùng. Trong khi đó, đội ngũ hỗ trợ bằng người không thể mở rộng (scale) khi lượng truy cập tăng đột biến.
- **Giải pháp KFC Bot Agent**:
  - Cho phép đặt hàng trực tiếp ngay trong khung chat (Zalo OA, Messenger, WhatsApp,...) mà không cần tải app, không cần tạo tài khoản mới.
  - Triết lý cốt lõi: **"A Chatbot Replies. An Agent Acts."** (Chatbot chỉ trả lời - Agent hành động thực sự).
  - Quy trình 5 bước tác tử: **Goal** (Hiểu ý định) -> **Plan** (Lập kế hoạch các bước) -> **Tools** (Truy vấn dữ liệu doanh nghiệp tin cậy) -> **Act** (Cập nhật giỏ hàng & áp dụng ưu đãi) -> **Verify** (Xác nhận lại với giỏ hàng thực tế). Mô hình AI đóng vai trò hiểu ngữ nghĩa, nhưng Tools mới là nơi quyết định dữ liệu thực tế.
- **Tư duy kiến trúc "Design Once | Deploy Everywhere"**:
  - Tách biệt **Channel Adapters** (Zalo, WhatsApp, Telegram, Instagram) giúp hệ thống dễ dàng thêm kênh mới hoặc thêm tính năng mới mà không cần viết lại toàn bộ core.
  - **Tầng Ingestion**: WAF, API Gateway, Lambda Webhook Handler, SQS.
  - **Tầng AgentCore Runtime**: Bedrock AgentCore, Agent Orchestration, Tool Use, Guardrails.
  - **Tầng Tool Layer & Memory**: AgentCore Gateway, Lambda Workers, DynamoDB (Session/State, Products, Orders), OpenSearch Service (Vector Store & Full-text Search).
  - **Tầng Observability & Security**: CloudWatch, X-Ray, CloudTrail, GuardDuty, AWS Secrets Manager, IAM.
- **Bốn chỉ số ấn tượng (Four Numbers Worth Writing Down)**:
  - **$0.006 / đơn hàng**: Chi phí hạ tầng cực kỳ tối ưu (tính toán cho 500 đơn/ngày).
  - **$88 / tháng**: Tổng chi phí hạ tầng Cloud (Bedrock chiếm 75%).
  - **3 - 5 giây**: Độ trễ end-to-end từ lúc người dùng gửi tin nhắn đến khi nhận phản hồi.
  - **-60% lượng code hạ tầng**: Nhờ sử dụng Amazon Bedrock AgentCore thay thế cho các lớp hạ tầng truyền thống.

---

### Những Gì Học Được

#### 1. Tư Duy Thiết Kế Agentic AI
- **Chuyển dịch từ Chatbot sang Agent**: Chatbot truyền thống chỉ phản hồi văn bản, còn AI Agent thực sự có khả năng lập kế hoạch (Plan), sử dụng công cụ (Tools), thực thi hành động (Act) và kiểm chứng kết quả (Verify).
- **Mô hình "The model understands. The tools decide what is real"**: Không phụ thuộc hoàn toàn vào LLM để đưa ra quyết định kinh doanh; sử dụng LLM cho khả năng hiểu ngôn ngữ tự nhiên và để các công cụ (Tools/APIs) kiểm soát tính chính xác của dữ liệu.
- **Tư duy "Design Once | Deploy Everywhere"**: Xây dựng kiến trúc dạng mô-đun với các lớp Channel Adapters và AgentCore Gateway giúp mở rộng kênh giao tiếp và tính năng mới dễ dàng mà không làm ảnh hưởng đến lõi hệ thống.

#### 2. Kiến Trúc Kỹ Thuật & Tối Ưu Chi Phí
- Tận dụng **Amazon Bedrock AgentCore**, **Strands Agent** và **MCP (Model Context Protocol)** giúp giảm đến 60% lượng code hạ tầng và đơn giản hóa việc tích hợp công cụ ngoại vi (Draw.io, Pricing API, Web Search).
- Bài toán tối ưu chi phí Cloud thực tế: Việc kết hợp Serverless (Lambda, DynamoDB, S3) với Bedrock AgentCore giúp duy trì chi phí ở mức cực thấp ($0.006/đơn hàng hoặc ~$35 - $88/tháng cho các hệ thống vừa và nhỏ).

#### 3. Bài Học Thực Tế & Tinh Thần Đồng Đội Từ Hackathon
- **"Clear direction beats too many options"**: Hướng đi rõ ràng quan trọng hơn việc có quá nhiều sự lựa chọn phức tạp.
- **"Execution matters more than perfection"**: Khả năng hoàn thành sản phẩm thực tế (MVP) quan trọng hơn việc cố gắng xây dựng một ý tưởng hoàn hảo nhưng dở dang.
- **"Small, finished work beats big, broken ideas"**: Một giải pháp nhỏ gọn nhưng chạy mượt mà luôn chiến thắng các ý tưởng lớn nhưng bị lỗi.

---

### Ứng Dụng Vào Công Việc

- **Áp dụng mô hình AI Agent vào dự án thực tế**: Chuyển đổi tư duy thiết kế các ứng dụng xử lý hội thoại (Chatbot) sang mô hình Agentic AI có tích hợp Tool Use và Guardrails trên AWS Bedrock.
- **Chuẩn hóa kiến trúc đa kênh (Multi-channel Architecture)**: Áp dụng pattern Adapter và Event-driven để thiết kế các hệ thống backend có khả năng mở rộng linh hoạt.
- **Tối ưu hóa quy trình làm việc cá nhân**: Sử dụng các công cụ AI hỗ trợ sinh diagram, IaC (Terraform) và báo giá tự động để giảm bớt các công việc lặp đi lặp lại.
- **Rèn luyện kỹ năng làm việc nhóm & Pitching**: Học hỏi cách trình bày kịch bản Demo trực quan, tập trung vào việc giải quyết nỗi đau của khách hàng (Problem - Solution - Impact) và đưa ra các con số chứng minh tính khảthi.

---

### Trải Nghiệm Trong Sự Kiện

Buổi **AWS Agentic AI Build Week Demo Day (25/07/2026)** là một sự kiện bùng nổ cảm xúc và tràn đầy cảm hứng công nghệ. Những điểm nhấn đáng nhớ bao gồm:

- **Sự đa dạng của các giải pháp**: Từ các công cụ hỗ trợ doanh nghiệp (Signal Scout), tự động hóa công việc kỹ thuật (Plan V), xử lý thị giác máy tính theo thời gian thực (S.H.E.P.H.E.R.D) cho đến ứng dụng thương mại đa kênh thực tế (KFC Bot Agent).
- **Tinh thần chiến đấu 24h của các đội thi**: Cảm nhận được năng lượng tích cực từ những câu chuyện "thức xuyên đêm đến 3h sáng", những ly Redbull, các sự kiện dở khóc dở cười như push nhầm file `.env` lên GitHub hay những chuyến đi dạo giải tỏa căng thẳng lúc nửa đêm.
- **Chất lượng chuyên môn cao từ ban giám khảo & cố vấn AWS**: Nhận được những góc nhìn phản biện sắc bén về bài toán chi phí, bảo mật (WAF, Guardrails) và khả năng ứng dụng thực tế trong doanh nghiệp.
- **Không khí ăn mừng và kết nối**: Khoảnh khắc vinh danh đội chiến thắng (One Team với KFC Bot Agent) và buổi tiệc networking giao lưu giữa các thí sinh, diễn giả và chuyên gia AWS đã khép lại tuần lễ Build Week một cách trọn vẹn và đầy ý nghĩa.

#### Một số hình ảnh minh chứng khi tham gia sự kiện

![Hình ảnh minh chứng 1](/images/4-EventParticipated/4.3-Event3/event3_evidence1.jpg)
*Hình ảnh minh chứng 1: Selfie tại sự kiện*

![Hình ảnh minh chứng 2](/images/4-EventParticipated/4.3-Event3/event3_evidence2.jpg)
*Hình ảnh minh chứng 2: Showcase và Demo dự án*

