---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->

# XÂY DỰNG ỨNG DỤNG OFFLINE-FIRST VỚI OPTIMISTIC UI: KẾT HỢP AWS AMPLIFY, APPSYNC, TANSTACK QUERY VÀ MONGODB ATLAS

Trong phát triển ứng dụng web và di động hiện đại, trải nghiệm người dùng (UX) phụ thuộc rất lớn vào tốc độ phản hồi của giao diện. Tuy nhiên, kết nối mạng không ổn định hoặc độ trễ từ máy chủ (latency) thường khiến ứng dụng phải hiển thị màn hình chờ (loading screen) liên tục, gây gián đoạn trải nghiệm. Bài viết này hướng dẫn cách xây dựng một ứng dụng theo tư duy Offline-First kết hợp với Optimistic UI (cập nhật giao diện lạc quan), sử dụng AWS Amplify Gen 2, AWS AppSync, TanStack Query và MongoDB Atlas. Giải pháp giúp dữ liệu hiển thị tức thì ngay cả khi mất kết nối mạng và tự động đồng bộ khi có kết nối trở lại.

### NHỮNG ĐIỂM NỔI BẬT:

* **Tư duy Offline-First & Optimistic UI**: Thay vì chờ phản hồi khứ hồi từ server (roundtrip), ứng dụng cập nhật bộ nhớ tạm (local cache) ngay lập tức khi người dùng thao tác. Nếu API gặp lỗi, hệ thống tự động hoàn tác (rollback) về trạng thái dữ liệu ban đầu.
* **Quản lý trạng thái bất đồng bộ với TanStack Query**: Tận dụng cơ chế caching và network mode của TanStack Query để quản lý luồng dữ liệu, tự động tạm dừng (pause) hoặc thử lại (retry) các yêu cầu khi kết nối mạng bị gián đoạn.
* **Kiến trúc Full-Stack Serverless linh hoạt**: Kết hợp AWS AppSync (GraphQL API), AWS Lambda (Resolver), Amazon Cognito (Xác thực người dùng) và AWS Amplify Gen 2 giúp tự động hóa hạ tầng và triển khai CI/CD dễ dàng qua Git-based workflow.
* **Tích hợp Cơ sở dữ liệu MongoDB Atlas**: Sử dụng MongoDB Atlas làm cơ sở dữ liệu NoSQL đám mây linh hoạt, kết nối liền mạch với hạ tầng AWS thông qua Lambda Resolver.
* **Cơ chế xử lý xung đột đơn giản**: Áp dụng phương pháp xử lý xung đột "đến trước phục vụ trước" (first-come first-served) dựa trên thứ tự ghi thực tế tại MongoDB Atlas, phù hợp cho các ứng dụng ít có xung đột dữ liệu đồng thời.

### TÌNH HUỐNG THỰC TẾ:

Một ứng dụng quản lý công việc (To-Do List) yêu cầu người dùng có thể thêm, sửa, xóa tác vụ liên tục ngay cả khi đang di chuyển qua các vùng sóng yếu hoặc hoàn toàn ngoại tuyến. Hệ thống cần phản hồi ngay tức thì trên giao diện mà không được để người dùng phải chờ đợi hiệu ứng tải (loading).

Kiến trúc triển khai ứng dụng Offline-First & Optimistic UI như sau:

> **User Action (React UI) → TanStack Query Cache (Instant UI Update) → AWS Amplify / AppSync (GraphQL API) → AWS Lambda Resolver → MongoDB Atlas (Database)**

![Sơ đồ kiến trúc Offline-First & Optimistic UI](/images/blog1architect.png)

Trong kiến trúc này:

* **User Action & TanStack Local Cache (onMutate)**: Khi người dùng thêm hoặc sửa tác vụ, TanStack Query lập tức hủy các truy vấn đang chạy (`cancelQueries`), lưu lại snapshot dữ liệu cũ và cập nhật dữ liệu mới trực tiếp vào cache để hiển thị ngay trên màn hình.
* **AWS AppSync & Lambda Resolver**: Đồng thời, ứng dụng gửi thao tác Mutation thông qua AWS AppSync GraphQL API. AWS Lambda đóng vai trò Resolver tiếp nhận request và thực hiện thao tác tương ứng trên MongoDB Atlas.
* **Error Handling & Rollback (onError)**: Nếu thao tác lưu dữ liệu thất bại (do lỗi server hoặc logic), TanStack Query sẽ sử dụng snapshot đã lưu trước đó để hoàn tác (rollback) cache về trạng thái ban đầu, đảm bảo tính nhất quán dữ liệu.
* **Network Queueing & Auto-Sync (onSettled / onSuccess)**: Khi thiết bị mất mạng, TanStack Query tạm dừng gửi mutation và đưa vào hàng đợi. Ngay khi có kết nối trở lại, các thao tác được gửi đi và hàm `invalidateQueries` được kích hoạt để đồng bộ lại dữ liệu mới nhất từ MongoDB Atlas.

### HƯỚNG DẪN TRIỂN KHAI ỨNG DỤNG (DEPLOYMENT GUIDE):

Để triển khai ứng dụng vào tài khoản AWS của bạn, hãy thực hiện theo các bước dưới đây. Sau khi triển khai, bạn có thể tạo người dùng, xác thực và tạo các tác vụ to-do.

1. **Thiết lập cụm MongoDB Atlas (Set up MongoDB Atlas Cluster):**
   * Truy cập MongoDB Atlas để khởi tạo cluster, Database, User và cấu hình quyền truy cập mạng (Network Access).

2. **Thiết lập người dùng (Set up & Configure User):**
   * Cấu hình thông tin người dùng và quyền truy cập database tương ứng.

3. **Sao chép GitHub Repository (Clone Repository):**
   * Clone mẫu ứng dụng bằng lệnh:
     ```bash
     git clone https://github.com/mongodb-partners/amplify-mongodb-tanstack-offline
     ```

4. **Cấu hình thông tin xác thực AWS CLI (Tùy chọn khi Debug Local):**
   * Nếu muốn kiểm tra ứng dụng ở môi trường local sandbox, hãy cấu hình credentials AWS tạm thời:
     ```bash
     export AWS_ACCESS_KEY_ID=<your-access-key-id>
     export AWS_SECRET_ACCESS_KEY=<your-secret-access-key>
     export AWS_SESSION_TOKEN=<your-session-token>
     ```

5. **Triển khai ứng dụng Todo trên AWS Amplify Console:**
   * Mở **AWS Amplify Console** và chọn tùy chọn **GitHub**.
   * Kết nối và cấp quyền truy cập GitHub Repository (`amplify-mongodb-tanstack-offline`).
   * Chọn Repository và nhánh (Branch) tương ứng, sau đó nhấn **Next**.
   * Giữ các tùy chọn còn lại ở mặc định và nhấn **Deploy**.

6. **Cấu hình biến môi trường (Environment Variables):**
   * Sau khi triển khai thành công, cấu hình các biến môi trường kết nối (MongoDB connection string, AppSync keys, v.v.).

7. **Truy cập ứng dụng và kiểm tra:**
   * Mở ứng dụng qua URL do AWS Amplify cung cấp và tiến hành kiểm tra các tính năng tạo, sửa, xóa To-Do.
   * Kiểm tra dữ liệu thực tế được ghi nhận đồng bộ trên MongoDB Atlas.

### KẾT LUẬN:

Điều mình thấy ấn tượng ở giải pháp này là cách tác giả kết hợp khéo léo sức mạnh của client-side state management (TanStack Query) với bộ dịch vụ Serverless của AWS và MongoDB Atlas để tối ưu hóa trải nghiệm người dùng cuối.

Bằng cách đưa tư duy Offline-First và Optimistic UI vào thiết kế, ứng dụng hoàn toàn loại bỏ được sự phụ thuộc vào độ trễ mạng trong các thao tác cơ bản, mang lại cảm giác mượt mà tuyệt đối như một ứng dụng native. Đây là một mẫu kiến trúc cực kỳ thực tế và hữu ích cho các dự án Web/Mobile App hiện đại, nơi mà trải nghiệm người dùng và tính sẵn sàng của dữ liệu luôn là ưu tiên hàng đầu.

---
**Link tài liệu gốc:**  
[AWS Mobile Blog - Offline Caching with AWS Amplify, TanStack, AppSync, and MongoDB Atlas](https://aws.amazon.com/blogs/mobile/offline-caching-with-aws-amplify-tanstack-appsync-and-mongodb-atlas/?fbclid=IwY2xjawTQRbBwZG9mBWV4dG4DYWVtAjEwAGJyaWQRMXA4TTZOYVBMTTBOczdNTTBzcnRjBmFwcF9pZBAyMjIwMzkxNzg4MjAwODkyAAEesu05YrTOVk9zmUd67tN6XtZMZw5fg4SRk9wIz4mZ3UxFB2eo7Cggm8k9i6o_aem_kteT-zlGgPrmMNZY1cqaiQ)
