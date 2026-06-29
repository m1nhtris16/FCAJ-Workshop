---
title: "Worklog Tuần 2"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}} -->


### Mục tiêu tuần 2:

* Nắm vững các dịch vụ tính toán cốt lõi của AWS, tập trung vào khả năng sẵn sàng cao và mở rộng quy mô (Elastic Load Balancing - ELB và Auto Scaling Groups - ASG).
* Hiểu các khái niệm cơ bản về Amazon S3 (Simple Storage Service), các lớp lưu trữ (storage classes), cơ chế bảo mật và quản lý vòng đời (lifecycle management).
* Tìm hiểu các loại lưu trữ khối EBS (Elastic Block Store) và quản lý volume.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu về các loại Elastic Load Balancing (ELB) bao gồm ALB, NLB, GLB <br> - Nắm rõ cơ chế hoạt động của listener, routing rules, target groups và health checks | 22/06/2026 | 22/06/2026 | [Xây dựng ứng dụng web độ sẵn sàng cao](https://docs.aws.amazon.com/elasticloadbalancing/) |
| 3 | - Học về Auto Scaling Groups (ASG) và Launch Templates <br> - Cấu hình các chính sách mở rộng (Target Tracking, Step Scaling) | 23/06/2026 | 23/06/2026 | [Mở rộng quy mô ứng dụng với Auto Scaling](https://000006.awsstudygroup.com/) |
| 4 | - Tìm hiểu về các khái niệm S3: buckets, keys, bảo mật (Bucket Policies, ACLs) <br> - Nghiên cứu các lớp lưu trữ S3 (S3 Storage Classes) và mã hóa dữ liệu (SSE-S3, SSE-KMS) | 24/06/2026 | 24/06/2026 | [Lưu trữ Website tĩnh với Amazon S3](https://000057.awsstudygroup.com/) |
| 5 | - Khám phá các tính năng nâng cao của S3 (Versioning, Lifecycle Rules, Object Lock) <br> - Nghiên cứu các loại ổ đĩa EBS (gp3, io2) và EBS Snapshots | 25/06/2026 | 25/06/2026 | [Bảo mật S3 - Best Practices](https://000069.awsstudygroup.com/) <br> [Các loại ổ đĩa EBS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html) |
| 6 | - **Thực hành:** <br> &emsp; + Tạo một S3 bucket bật versioning, thiết lập vòng đời (lifecycle rules) để chuyển tiếp dữ liệu sang Glacier <br> &emsp; + Thiết lập Launch Template, Target Group, ALB và Auto Scaling Group để kiểm thử tính sẵn sàng cao | 26/06/2026 | 26/06/2026 | [Xây dựng ứng dụng web độ sẵn sàng cao](https://000101.awsstudygroup.comhttps://docs.aws.amazon.com/elasticloadbalancing/) <br> [Lưu trữ Website tĩnh với Amazon S3](https://000057.awsstudygroup.com/) |


### Kết quả đạt được tuần 2:

* **Tính sẵn sàng cao & Cân bằng tải**: Nắm vững các khái niệm ELB (Application Load Balancer), cấu hình thành công target groups và health checks để định tuyến lưu lượng truy cập hiệu quả giữa các EC2 instances.
* **Tự động mở rộng quy mô**: Hiểu rõ cơ chế hoạt động của Auto Scaling Groups (ASG) kết hợp với Launch Templates để tự động điều chỉnh số lượng EC2 instances dựa trên tải thực tế hoặc mức độ sử dụng tài nguyên.
* **Lưu trữ đối tượng mở rộng**: Đạt được sự hiểu biết sâu sắc về Amazon S3, bao gồm phân quyền bucket (Bucket Policies, block public access), mã hóa dữ liệu và các lớp lưu trữ phù hợp để tối ưu chi phí.
* **Quản lý vòng đời lưu trữ & Ổ đĩa khối**: Tạo thành công lifecycle rules trên Amazon S3 để tự động tối ưu hóa chi phí lưu trữ, nghiên cứu sự khác biệt giữa các loại EBS volumes (gp3, io2) để chọn lựa cấu hình tối ưu.
* **Kỹ năng thực hành**: Triển khai thành công mô hình hạ tầng sẵn sàng cao gồm ALB và ASG tự động co giãn EC2 instances qua nhiều Availability Zones (AZs), đồng thời cấu hình thành công S3 bucket bật versioning và chính sách lưu trữ Glacier.
