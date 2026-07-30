---
title: "Workshop"
date: 2026-07-30
weight: 5
chapter: false
pre: " <b> 5. </b> "
includeInReport: false
---

# CODEXECUTE WORKSHOP: HỆ THỐNG CHẤM BÀI TRỰC TUYẾN & NỀN TẢNG THUẬT TOÁN TỰ ĐỘNG TRÊN AWS SERVERLESS

#### Tổng quan

Workshop **CodExecute** hướng dẫn từng bước thiết kế, triển khai và vận hành hệ thống chấm bài tự động trực tuyến (Online Judge) hoàn toàn trên môi trường **Cloud-Native AWS Serverless**.

Bài lab kết hợp các dịch vụ Serverless cốt lõi bao gồm **AWS Lambda** (chạy API backend &amp; Sandbox chấm bài cách ly), **Amazon API Gateway** (REST API entrypoint), **Amazon SQS** (Hàng chờ điều tiết nộp bài), **Amazon DynamoDB** (Lưu trữ metadata &amp; bài tập), **Amazon S3** (Lưu trữ bộ Testcases &amp; Frontend static hosting) và **Amazon CloudFront** (CDN phân phối ứng dụng).

#### Các nội dung trong Workshop

<style>
#body a.toc-link, 
#body a.toc-link:hover, 
#body a.toc-link:focus, 
#body a.toc-link:active {
  border: 1px solid #e2e8f0 !important;
  border-bottom: 1px solid #e2e8f0 !important;
  text-decoration: none !important;
  background-image: none !important;
  box-shadow: 0 1px 3px rgba(0,0,0,0.04) !important;
}
#body a.toc-link:after, 
#body a.toc-link:before {
  display: none !important;
  content: "" !important;
}
#body a.toc-link:hover {
  border-color: #FF9900 !important;
  background-color: #FFFDF7 !important;
}
</style>

<div style="display: flex; flex-direction: column; gap: 10px; margin: 20px 0;">

  <a class="toc-link" href="5.1-Workshop-overview/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">BƯỚC 01</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Giới thiệu &amp; Tổng quan</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Tổng quan nền tảng CodExecute, sơ đồ kiến trúc hệ thống và các dịch vụ AWS sử dụng</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.2-Prerequiste/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">BƯỚC 02</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Các bước chuẩn bị</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Cài đặt môi trường phát triển, cấu hình AWS CLI credentials và khởi tạo project local</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.3-fe/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">BƯỚC 03</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Triển khai Frontend và cấu hình CloudFront CDN</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Tải ứng dụng React Frontend lên Amazon S3 và cấu hình phân phối CloudFront với OAC</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.4-be/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">BƯỚC 04</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Triển khai Lambda qua Docker &amp; ECR</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Đóng gói container image cho API và Sandbox Worker, đẩy lên ECR và khởi tạo hàm AWS Lambda</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.5-DynamoDB/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">BƯỚC 05</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Thiết lập Amazon DynamoDB</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Khởi tạo các bảng DynamoDB cho Users, Problems, Submissions, TestCases và định nghĩa Khóa chính</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.6-APIGateway/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">BƯỚC 06</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Cấu hình API Gateway cho Lambda API</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Thiết lập HTTP API Gateway proxy routing, cấu hình CORS và liên kết với Lambda API Backend</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.7-SQS/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">BƯỚC 07</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Thiết lập Amazon SQS</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Tạo hàng chờ điều tiết nộp bài bất đồng bộ và cấu hình Event Source Mapping kích hoạt Lambda Worker</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.8-CloudWatch/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">BƯỚC 08</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Giám sát với CloudWatch</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Theo dõi log thực thi Lambda, chỉ số SQS queue metrics và dashboard giám sát hệ thống</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.9-SNS/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">BƯỚC 09</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Cài đặt SNS &amp; Lambda Alarms</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Cấu hình Amazon SNS Topic thông báo và tạo CloudWatch Alarms phát hiện lỗi tự động</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="5.10-Cleanup/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #ef4444; background: #FEF2F2; padding: 5px 10px; border-radius: 6px; border: 1px solid #FEE2E2;">BƯỚC 10</span>
      <div>
        <div style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Dọn dẹp tài nguyên</div>
        <div style="font-size: 0.82rem; color: #64748b; margin-top: 2px;">Xóa sạch toàn bộ tài nguyên hạ tầng AWS Serverless để tránh phát sinh chi phí ngoài ý muốn</div>
      </div>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

</div>