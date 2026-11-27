---
title: "Worklog Tuần 12"
date: 2026-11-10
weight: 12
chapter: false
pre: " <b> 1.12 </b> "
---

**Thời gian:** 10/11/2026 – 15/11/2026

### Mục tiêu tuần 12

- Rà soát tình trạng ứng dụng Macket và hạ tầng AWS.
- Cải thiện kết nối mạng, bảo mật và hiệu quả chi phí hạ tầng.
- Rà soát các vấn đề tích hợp còn mở và ghi lại hướng cải thiện.
- Hoàn thiện sơ đồ kiến trúc đám mây và tài liệu dự án.

### Nội dung thực hiện trong tuần

| STT | Nội dung thực hiện | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---: | --- | --- | --- | --- |
| 1 | Rà soát tài nguyên AWS đã triển khai và kiểm tra kết nối giữa **EC2**, **Amazon RDS** for PostgreSQL và **S3**. Kiểm tra security group và quyền truy cập tài nguyên. | 10/11/2026 | 10/11/2026 | [**Amazon VPC**](https://docs.aws.amazon.com/vpc/) |
| 2 | Cấu hình gateway endpoint cho **Amazon S3** trong **Amazon VPC** để cải thiện kết nối giữa **EC2** và **S3**. Rà soát các bảng định tuyến liên quan và kiểm tra đường đi của yêu cầu **S3**. | 11/11/2026 | 11/11/2026 | [Gateway Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html) |
| 3 | Rà soát nhật ký ứng dụng, kết quả nhập dữ liệu và vấn đề tích hợp AI Worker còn mở. Đánh giá chi phí hạ tầng và xác định hướng tăng độ tin cậy và tự động hóa. | 12/11/2026 | 12/11/2026 | [AWS Pricing Calculator](https://calculator.aws/) |
| 4 | Cập nhật sơ đồ kiến trúc đám mây theo tài nguyên đã triển khai, cấu hình subnet và kết nối giữa các dịch vụ. | 13/11/2026 | 13/11/2026 | — |
| 5 | Tổng kết kết quả triển khai, ghi lại các giới hạn kỹ thuật và hướng cải thiện, đồng thời chuẩn bị báo cáo thực tập cuối kỳ. | 14/11/2026 | 15/11/2026 | — |

### Kết quả đạt được trong tuần

- Rà soát tình trạng vận hành của ứng dụng Macket và hạ tầng AWS.
- Cấu hình gateway endpoint cho **Amazon S3** để tài nguyên trong **Amazon VPC** có thể kết nối riêng tư với **S3**.
- Rà soát cấu hình mạng, quyền truy cập tài nguyên và chi phí hạ tầng.
- Ghi lại các vấn đề tích hợp còn mở và hướng xử lý.
- Cập nhật sơ đồ kiến trúc đám mây theo trạng thái triển khai thực tế.
- Tổng kết kết quả dự án và chuẩn bị tài liệu kỹ thuật cuối kỳ.
