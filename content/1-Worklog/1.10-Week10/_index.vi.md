---
title: "Worklog Tuần 10"
date: 2026-10-31
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

**Thời gian:** 31/10/2026 – 04/11/2026

### Mục tiêu tuần 10

- Nghiên cứu và đánh giá các giải pháp thu thập bài đăng từ các nhóm Facebook.
- Phát triển mô-đun tự động thu thập bài đăng bán đồ điện tử đã qua sử dụng.
- Tối ưu chi phí thu thập và giảm số bài đăng không cần thiết từ người bán thương mại.
- Xây dựng luồng nhập dữ liệu từ dịch vụ thu thập bên ngoài vào PostgreSQL.
- Chuẩn bị mô-đun thu thập để triển khai bằng container và chạy theo lịch.

### Nội dung thực hiện trong tuần

| STT | Nội dung thực hiện | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---: | --- | --- | --- | --- |
| 1 | Nghiên cứu các thư viện mã nguồn mở để thu thập bài đăng Facebook. Thử nghiệm crawler ban đầu, gặp lỗi phụ thuộc và phân tích JSON, rồi đánh giá các phương án thay thế. | 31/10/2026 | 31/10/2026 | [GitHub](https://github.com/) |
| 2 | Đánh giá các Apify Actor dành cho nhóm Facebook. Thử nghiệm Actor ban đầu và ghi nhận hạn chế về chi phí cùng tỷ lệ cao bài đăng từ người bán thương mại. | 01/11/2026 | 01/11/2026 | [Apify Store](https://apify.com/store) |
| 3 | Chọn Apify Actor có chi phí phù hợp hơn, lập danh sách các nhóm Facebook hoạt động tích cực và cấu hình tham số thu thập, sắp xếp, độ mới của bài đăng và quy tắc loại trừ người bán. | 02/11/2026 | 02/11/2026 | [Apify Documentation](https://docs.apify.com/) |
| 4 | Phát triển mô-đun Python để khởi chạy tác vụ thu thập, lấy dữ liệu, chuẩn hóa bài đăng và xử lý thông tin tác giả, URL ảnh và siêu dữ liệu. Bổ sung phát hiện trùng lặp và nhập dữ liệu vào cơ sở dữ liệu. | 03/11/2026 | 03/11/2026 | [Apify API](https://docs.apify.com/api/v2) |
| 5 | Cấu hình môi trường phát triển Docker và chuẩn bị đóng gói mô-đun thu thập bằng container. Xây dựng luồng chạy trên **AWS Lambda** với chu kỳ mục tiêu 10 phút, đồng thời rà soát nhật ký thực thi và cơ chế xử lý lỗi. | 04/11/2026 | 04/11/2026 | [Docker](https://docs.docker.com/), [**AWS Lambda**](https://docs.aws.amazon.com/lambda/) |

### Kết quả đạt được trong tuần

- Đánh giá nhiều phương án thu thập dữ liệu Facebook và xác định hạn chế của phương án mã nguồn mở ban đầu.
- Chọn giải pháp thu thập bên ngoài có độ tin cậy và chi phí phù hợp hơn.
- Giảm chi phí thu thập ước tính từ khoảng 5 USD xuống 0,20 USD trên mỗi 1.000 bài đăng bằng cách đổi Apify Actor.
- Lập danh sách các nhóm Facebook hoạt động tích cực về đồ điện tử đã qua sử dụng.
- Xây dựng mô-đun thu thập và luồng nhập dữ liệu có cấu trúc.
- Triển khai phát hiện bài đăng trùng lặp và quy tắc loại trừ người bán thương mại.
- Chuẩn bị mô-đun để triển khai bằng container và chạy theo lịch.
