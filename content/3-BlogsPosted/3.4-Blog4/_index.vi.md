---
title: "Blog 4"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# CÁCH MẠNG HÓA TRẢI NGHIỆM TƯƠNG TÁC CỦA NGHƯỜI HÂM MỘ: BÌNH LUẬN TRỰC TIẾP BẰNG GENERATIVE AI CHO BUNDESLIGA

Trong ngành phát sóng thể thao và truyền thông hiện đại, việc thu hút lượng người hâm mộ toàn cầu đòi hỏi phải cung cấp các cập nhật trận đấu theo thời gian thực được cá nhân hóa theo từng ngôn ngữ và phong cách văn viết địa phương. Tuy nhiên, các kênh truyền thống thường thiếu các bản phát sóng địa phương hóa hoặc không đáp ứng được phong cách tường thuật yêu thích của từng nhóm khán giả. Bài viết này khám phá giải pháp bình luận trực tiếp (live ticker) sử dụng Generative AI được phát triển bởi AWS hợp tác với Sportec Solutions AG cho giải bóng đá Bundesliga (Đức). Giải pháp tự động chuyển đổi dữ liệu sự kiện thô từ sân thi đấu thành các đoạn bình luận phong phú, đa ngôn ngữ và đa phong cách theo thời gian thực.

### ĐIỂM NỔI BẬT CHÍNH:

* **Đa ngôn ngữ & Phong cách Cá nhân hóa**: Tự động tạo các đoạn bình luận trận đấu đồng thời bằng nhiều ngôn ngữ và tông giọng khác nhau như "Nhà báo thể thao", "Thân mật", hoặc phong cách "Gen Z / Bro".
* **Xử lý Dữ liệu Sự kiện Chuyên sâu**: Xử lý khoảng 1.600 điểm dữ liệu sự kiện trực tiếp trên sân cho mỗi trận đấu Bundesliga (ví dụ: loại cú sút, tốc độ cầu thủ, số lượng hậu vệ và chỉ số áp lực).
* **Kiến trúc AWS Serverless Hoàn toàn**: Sử dụng AWS Lambda, Amazon ECS Fargate, AWS AppSync và Amazon Bedrock để tự động mở rộng quy mô mượt mà trong suốt trận đấu và giảm về 0 vào những ngày không có trận đấu để tối ưu chi phí.
* **Đồng bộ Luồng với Độ trễ Thấp**: Đạt độ trễ xử lý toàn trình chỉ từ 7 đến 12 giây từ khi hành động xảy ra trên sân cho đến khi hiển thị trên giao diện người dùng, nằm trọn trong khoảng độ trễ phát sóng video tiêu chuẩn.

### KỊCH BẢN THỰC TẾ:

Người hâm mộ bóng đá quốc tế theo dõi các đội bóng Bundesliga ở nước ngoài cần cập nhật trực tiếp theo từng phút bằng ngôn ngữ mẹ đẻ và phong cách yêu thích, ngay cả khi không có kênh phát sóng khu vực hoặc bình luận bằng tiếng địa phương.

### TỔNG QUAN KIẾN TRÚC:

```text
Dữ liệu sự kiện trực tiếp ➔ Bundesliga Datahub / Amazon ECS Fargate ➔ AWS Lambda & Amazon Bedrock ➔ AWS AppSync (GraphQL API) ➔ Amazon DynamoDB & Frontend UI
```

![Sơ đồ kiến trúc Bundesliga Generative AI](/images/blog4-architect.jpg)

Trong kiến trúc này:

* **Thu thập & Trích xuất Sự kiện (Amazon ECS Fargate)**: Dữ liệu sự kiện trực tiếp từ sân đấu (~1.600 sự kiện/trận) được nạp qua Bundesliga Datahub và xử lý trên ECS Fargate để trích xuất các thuộc tính trận đấu quan trọng (chỉ số cầu thủ, áp lực, đánh giá cơ hội).
* **Kỹ thuật Prompt & Tạo văn bản GenAI (AWS Lambda & Amazon Bedrock)**: AWS Lambda định dạng dữ liệu trích xuất thành các prompt có cấu trúc chỉ định ngôn ngữ và phong cách văn viết mục tiêu, sau đó gọi API đến Amazon Bedrock để tạo đoạn bình luận hấp dẫn.
* **API Real-time & Lưu trữ (AWS AppSync & Amazon DynamoDB)**: AWS AppSync nhận đoạn bình luận và phân phối tức thì qua GraphQL subscriptions đến ứng dụng người dùng (Web UI), đồng thời tự động lưu trữ vào Amazon DynamoDB để đảm bảo tính toàn vẹn dữ liệu.

### CÁC BƯỚC TRIỂN KHAI:

1. **Bắt dữ liệu sự kiện**: Các hành động trên sân kích hoạt dữ liệu sự kiện có cấu trúc (sút, truyền, phạm lỗi) được gửi qua mạng Bundesliga Datahub.
2. **Trích xuất ngữ cảnh**: ECS Fargate phân tích dữ liệu JSON thô để trích xuất các chỉ số ngữ cảnh chi tiết (điều kiện sút, khoảng cách thủ môn).
3. **Nạp LLM & Tạo phong cách**: AWS Lambda chuyển tiếp dữ liệu kèm theo prompt định hình phong cách đến các mô hình nền tảng Amazon Bedrock.
4. **Phân phối thời gian thực**: AWS AppSync phát sóng các đoạn bình luận trực tiếp đến ứng dụng web qua GraphQL subscriptions trong vòng 7–12 giây.

### KẾT LUẬN:

Bằng cách kết hợp nguồn dữ liệu thể thao phong phú với Amazon Bedrock và các dịch vụ AWS Serverless, Bundesliga và Sportec Solutions đã tạo ra một giải pháp tự động hóa giúp kết nối khán giả thể thao quốc tế trên quy mô lớn. Nền tảng kiến trúc này mở ra tiền đề cho các đổi mới trong tương lai, bao gồm tạo giọng nói bình luận AI truyền cảm và tự động tạo tài sản hình ảnh cho các buổi phát sóng thể thao toàn cầu.

---
**Link bài viết gốc:**  
[AWS Media Blog - Revolutionizing Fan Engagement: Bundesliga Generative AI-Powered Live Commentary](https://aws.amazon.com/blogs/media/revolutionizing-fan-engagementcer-bundesliga-generative-ai-powered-live-commentary/)
