# 🚀 FCAJ Workshop - AWS Internship Report

[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Live%20Demo-orange?style=for-the-badge&logo=github)](https://m1nhtris16.github.io/fcaj-workshop/)
[![Hugo](https://img.shields.io/badge/Hugo-v0.134.3-FF4088?style=for-the-badge&logo=hugo)](https://gohugo.io/)
[![AWS](https://img.shields.io/badge/Amazon%20Web%20Services-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=FF9900)](https://aws.amazon.com/)

Báo cáo tổng kết quá trình thực tập và tài liệu học tập Hands-on Workshop trong chương trình **Workforce Bootcamp - First Cloud AI Journey (FCAJ)** tại **Amazon Web Services (AWS) Viet Nam**.

🌐 **Website Live Demo:** [https://m1nhtris16.github.io/fcaj-workshop/](https://m1nhtris16.github.io/fcaj-workshop/)

---

## 👨‍🎓 Thông tin thực tập sinh (Student Profile)

| Tiêu chí | Thông tin |
| :--- | :--- |
| **Họ và tên:** | Lê Minh Trí |
| **Email:** | `tri.leminhcsk23@hcmut.edu.vn` |
| **Trường:** | Trường Đại học Bách Khoa - ĐHQG TP.HCM (HCMUT) |
| **Chuyên ngành:** | Khoa học Máy tính (Computer Science) |
| **Công ty thực tập:** | Amazon Web Services Viet Nam Company Limited |
| **Vị trí thực tập:** | Workforce Bootcamp - First Cloud AI Journey |
| **Thời gian thực tập:** | 15/06/2026 – 14/08/2026 |

---

## 📚 Nội dung báo cáo (Table of Contents)

Tài liệu được xây dựng song ngữ (Anh - Việt) và chia thành các mục chính:

1. **📋 Worklog (Nhật ký thực tập):** Theo dõi tiến độ công việc và bài học theo từng tuần.
2. **💡 Project Proposal (Đề xuất dự án):** Kiến trúc giải pháp và đề xuất hệ thống.
3. **✍️ Blogs Posted:** Tổng hợp các bài viết chia sẻ kiến thức chuyên môn về AWS Cloud.
4. **🎪 Events Participated:** Nhật ký tham gia các sự kiện, Build Week & Hackathon 24h.
5. **🛠 Hands-on Workshop:** Hướng dẫn chi tiết các bài Lab kỹ thuật về AWS:
   - VPC Gateway Endpoints & Interface Endpoints (S3, PrivateLink).
   - Phân quyền & IAM Bucket Policies (VPC Restriction, Deny/Allow rules).
   - Mô phỏng Route 53 DNS Simulation & Hybrid Cloud Connectivity.
   - Cleanup & Quản lý tài nguyên AWS.
6. **🎯 Self-evaluation:** Tự đánh giá kết quả đạt được sau kỳ thực tập.
7. **💬 Sharing and Feedback:** Nhận xét và góp ý từ Mentors & Chuyên gia AWS.
8. **📖 References:** Tài liệu tham khảo và mã nguồn dự án.

---

## 📁 Cấu trúc thư mục (Repository Structure)

```text
FCAJ-Workshop/
├── .github/
│   └── workflows/
│       └── hugo.yml         # GitHub Actions CI/CD deployment workflow
├── content/                 # Nội dung bài viết Markdown (English & Tiếng Việt)
│   ├── 1-Worklog/
│   ├── 2-Proposal/
│   ├── 3-BlogsPosted/
│   ├── 4-EventParticipated/
│   ├── 5-Workshop/
│   ├── 6-Self-evaluation/
│   ├── 7-Feedback/
│   └── 8-References/
├── layouts/                 # Custom HTML Layouts & Components
├── static/                  # Tài nguyên tĩnh (Hình ảnh, CSS custom, Fonts)
│   ├── css/
│   └── images/
├── themes/
│   └── hugo-theme-learn/    # Theme Hugo Learn (Submodule)
├── config.toml              # Cấu hình dự án Hugo
└── README.md
```

---

## 💻 Hướng dẫn chạy dự án cục bộ (Local Development)

### Yêu cầu tiên quyết (Prerequisites)
- [Git](https://git-scm.com/)
- [Hugo Extended](https://gohugo.io/installation/) (`v0.134.3` hoặc mới hơn)

### Các bước cài đặt & khởi chạy

1. **Clone repository về máy (kèm submodules):**
   ```bash
   git clone --recursive https://github.com/m1nhtris16/fcaj-workshop.git
   cd fcaj-workshop
   ```

2. **Khởi chạy Local Hugo Server:**
   ```bash
   hugo server
   # Hoặc nếu sử dụng file thực thi trong dự án:
   .\bin\hugo.exe server
   ```

3. **Mở trình duyệt truy cập:**  
   [http://localhost:1313/fcaj-workshop/](http://localhost:1313/fcaj-workshop/)

---

## 🚀 Tự động Deploy lên GitHub Pages (CI/CD Workflow)

Dự án được cấu hình tự động Deploy lên GitHub Pages bằng **GitHub Actions** ([.github/workflows/hugo.yml](file:///.github/workflows/hugo.yml)):
- Mỗi khi push code lên nhánh `main`, GitHub Actions sẽ tự động checkout code, cài đặt Hugo Extended, build website tĩnh và đẩy lên môi trường GitHub Pages.

---

## 🙏 Lời cảm ơn (Acknowledgements)

Trân trọng cảm ơn các Mentors, Speakers và Đội ngũ hỗ trợ từ **Amazon Web Services (AWS) Việt Nam** cùng cộng đồng **AWS Study Group** đã đồng hành và hướng dẫn trong suốt thời gian diễn ra chương trình Bootcamp.
