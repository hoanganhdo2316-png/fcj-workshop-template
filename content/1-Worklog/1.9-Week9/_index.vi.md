---
title: "Worklog Tuần 9"
date: 2026-10-25
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

**Thời gian:** 25/10/2026 – 30/10/2026

### Mục tiêu tuần 9

- Phân tích yêu cầu và xác định phạm vi của Macket, nền tảng tự động thu thập và lọc bài đăng bán đồ điện tử đã qua sử dụng trên Facebook.
- Thiết kế kiến trúc đám mây AWS, cân nhắc khả năng mở rộng và chi phí hạ tầng.
- Triển khai và cấu hình hạ tầng nền tảng gồm **Amazon VPC**, **Amazon EC2**, **Amazon RDS** for PostgreSQL và **Amazon S3**.
- Thiết lập môi trường phát triển backend và kiểm tra kết nối giữa các tài nguyên đám mây.
- Cấu hình truy cập qua tên miền và bảo mật kết nối bằng HTTPS.

### Nội dung thực hiện trong tuần

| STT | Nội dung thực hiện | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---: | --- | --- | --- | --- |
| 1 | Phân tích yêu cầu dự án, xác định các thành phần chính và xây dựng luồng thu thập, xử lý dữ liệu. | 25/10/2026 | 25/10/2026 | — |
| 2 | Thiết kế kiến trúc trên AWS, chọn **EC2**, **RDS**, **S3** và **Lambda**, sau đó rà soát phương án hạ tầng và ước tính chi phí vận hành. | 26/10/2026 | 26/10/2026 | [AWS Pricing Calculator](https://calculator.aws/) |
| 3 | Cấu hình **Amazon VPC**, subnet, bảng định tuyến và security group. Khởi tạo máy chủ **EC2** chạy Ubuntu và thiết lập truy cập từ xa qua SSH. | 27/10/2026 | 27/10/2026 | [**Amazon VPC**](https://docs.aws.amazon.com/vpc/) |
| 4 | Triển khai **Amazon RDS** for PostgreSQL, cấu hình quyền truy cập cơ sở dữ liệu và thiết lập kết nối mã hóa giữa **EC2** và **RDS**. | 28/10/2026 | 28/10/2026 | [**Amazon RDS**](https://docs.aws.amazon.com/rds/) |
| 5 | Khởi tạo backend FastAPI, cấu hình SQLAlchemy và trình điều khiển PostgreSQL, đồng thời xây dựng endpoint kiểm tra trạng thái. Phân tích và khắc phục lỗi tích hợp cơ sở dữ liệu và thử nghiệm embedding. | 29/10/2026 | 29/10/2026 | [FastAPI Documentation](https://fastapi.tiangolo.com/) |
| 6 | Cấu hình **Amazon S3** để lưu trữ tệp đa phương tiện. Thiết lập Nginx làm reverse proxy và chạy backend dưới dạng dịch vụ systemd. | 30/10/2026 | 30/10/2026 | [**Amazon S3**](https://docs.aws.amazon.com/s3/) |
| 7 | Trỏ tên miền `macket.io.vn` về máy chủ và bật HTTPS bằng Certbot. | 30/10/2026 | 30/10/2026 | [Certbot](https://certbot.eff.org/) |

### Kết quả đạt được trong tuần

- Xác định yêu cầu và thiết kế kiến trúc AWS ban đầu cho Macket.
- Thiết lập hạ tầng mạng, máy chủ, cơ sở dữ liệu và lưu trữ.
- Cấu hình **EC2** và kết nối thành công với **Amazon RDS** for PostgreSQL.
- Khởi tạo backend FastAPI và kiểm tra các endpoint theo dõi trạng thái ứng dụng và cơ sở dữ liệu.
- Khắc phục lỗi tích hợp cơ sở dữ liệu ban đầu và thử nghiệm thành công việc lưu vector embedding.
- Cấu hình tên miền, Nginx reverse proxy và truy cập HTTPS.
- Truy cập được giao diện cơ bản của Macket qua tên miền `macket.io.vn`.
- Thiết lập môi trường đám mây để tiếp tục phát triển ứng dụng.
