---
title: "Kiểm tra và dự toán chi phí"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

### Kiểm tra sau triển khai

Mở trang qua HTTPS và kiểm tra `/api/health`. Chạy một lượt thu thập có giới hạn, đối chiếu bài mẫu trong RDS, kết quả AI, ảnh S3 và giao diện.

<!-- ẢNH 08: Bài mẫu trên giao diện hoặc trạng thái thực tế. -->

### Dự toán chi phí

Dự toán trong Proposal cho cấu hình tham chiếu là **46,65–82,65 USD/tháng**. Khi dựng lab, theo dõi riêng chi phí Apify và OpenAI API. [Xem bảng dự toán trong Proposal](../../2-proposal/#6-dự-toán-chi-phí)

Bảng AWS Cost Explorer cho kỳ **01–30/11/2026** hiển thị **25,80 USD** tại thời điểm chụp ngày **28/11/2026**. Hai khoản lớn nhất là **RDS** (13,68 USD) và **EC2 Instances** (5,69 USD). Đây là số liệu của báo cáo có bộ lọc đang áp dụng.

![Bảng chi phí AWS và kỳ báo cáo tháng 11/2026](/images/2-Proposal/macket-cost-explorer-nov-2026.png)
