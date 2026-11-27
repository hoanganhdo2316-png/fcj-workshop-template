---
title: "Worklog Tuần 11"
date: 2026-11-05
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

**Thời gian:** 05/11/2026 – 09/11/2026

### Mục tiêu tuần 11

- Xây dựng luồng phân loại bằng AI cho các bài đăng thu thập từ Facebook.
- Phân biệt người bán cá nhân và người bán thương mại, đồng thời nhận diện bài đăng đồ điện tử phù hợp.
- Phát triển API backend để truy cập và quản lý dữ liệu thu thập.
- Xây dựng giao diện web Macket để duyệt bài đăng bán đồ điện tử đã qua sử dụng.
- Tích hợp các thành phần thu thập, xử lý AI, lưu trữ và ứng dụng.

### Nội dung thực hiện trong tuần

| STT | Nội dung thực hiện | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---: | --- | --- | --- | --- |
| 1 | Thiết kế luồng xử lý AI và tiêu chí phân loại. Rà soát các bài đăng đã thu thập để nhận diện bài đăng phù hợp, quảng cáo thương mại và đặc điểm của người bán cá nhân. | 05/11/2026 | 05/11/2026 | — |
| 2 | Phát triển AI Worker, xác định các bước phân loại và trạng thái xử lý. Rà soát tích hợp vector embedding và khả năng ứng dụng vào đối sánh sản phẩm. | 06/11/2026 | 06/11/2026 | [PostgreSQL](https://www.postgresql.org/docs/) |
| 3 | Xây dựng các endpoint FastAPI để truy xuất bài đăng và duyệt theo sản phẩm. Tích hợp truy vấn PostgreSQL và rà soát luồng lưu trữ tệp đa phương tiện trên **Amazon S3**. | 07/11/2026 | 07/11/2026 | [FastAPI Documentation](https://fastapi.tiangolo.com/) |
| 4 | Xây dựng frontend Macket theo dạng sàn giao dịch. Sắp xếp bài đăng theo danh mục thiết bị điện tử và mẫu MacBook, sau đó tích hợp giao diện với API backend. | 08/11/2026 | 08/11/2026 | — |
| 5 | Kiểm thử tích hợp giữa Apify, mô-đun nhập dữ liệu, **Amazon RDS** for PostgreSQL, AI Worker và ứng dụng web. Qua nhật ký xử lý, phát hiện bài đăng mới vẫn ở trạng thái chờ vì AI Worker chưa được kích hoạt tự động sau khi nhập dữ liệu. | 09/11/2026 | 09/11/2026 | [**AWS Lambda**](https://docs.aws.amazon.com/lambda/) |

### Kết quả đạt được trong tuần

- Xây dựng luồng phân loại bài đăng bằng AI.
- Hoàn thiện tiêu chí nhận diện bài đăng đồ điện tử phù hợp và người bán thương mại.
- Tích hợp backend với dữ liệu Facebook lưu trong PostgreSQL.
- Phát triển giao diện dạng sàn giao dịch và chức năng duyệt theo sản phẩm.
- Tích hợp luồng thu thập, lưu trữ, xử lý AI và ứng dụng web.
- Kiểm thử thành công luồng thu thập và nhập dữ liệu thủ công.
- Xác định vấn đề tự động kích hoạt giữa mô-đun nhập dữ liệu và AI Worker cần tiếp tục xử lý.
