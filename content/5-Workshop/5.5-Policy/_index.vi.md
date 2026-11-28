---
title: "Cấu hình Lambda, Scheduler và vận hành"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

1. Build image từ `apify/Dockerfile.lambda`, đẩy lên **Amazon ECR**, rồi tạo **AWS Lambda** từ image. Cấu hình `COLLECTOR_CONFIG_URL` và `COLLECTOR_CONFIG_TOKEN`. Chạy thử một lần và đối chiếu lượt Apify qua log **CloudWatch**.
2. Tạo **EventBridge Scheduler** trỏ tới Lambda và cấp quyền gọi hàm. Giữ lịch tắt trong lúc kiểm tra giới hạn chi phí và luồng thủ công.
3. Nếu dựng lớp CDN, cấu hình **CloudFront** trỏ tới Nginx qua HTTPS và gắn **AWS WAF** ở chế độ Count để quan sát ban đầu.

<!-- ẢNH 07: Nguồn image Lambda, target Scheduler hoặc log lượt chạy. -->

## Log, Metrics và cảnh báo

### Amazon CloudWatch

Mở **CloudWatch → Dashboards → Macket-Monitoring** để xem CPU, RAM và mức sử dụng đĩa của **Amazon EC2**, cùng CPU, bộ nhớ khả dụng, số kết nối, dung lượng trống và độ trễ đọc/ghi của **Amazon RDS**. Dashboard cũng hiển thị số lượt gọi, lỗi và thời gian chạy của **AWS Lambda**. Để xem dung lượng bucket và số đối tượng của **Amazon S3**, vào **Metrics → All metrics → S3**. [Chỉ số Amazon S3 trong CloudWatch](https://docs.aws.amazon.com/AmazonS3/latest/userguide/metrics-dimensions.html).

![Dashboard Macket-Monitoring trong Amazon CloudWatch](/images/5-Workshop/5.5-Policy/macket-cloudwatch-dashboard.png)

**Log kỹ thuật.** Vào **Logs → Log groups** và mở `/aws/lambda/<function-name>` để kiểm tra log thực thi của **AWS Lambda**. Log backend và AI Worker có trong **CloudWatch Logs** nếu đã cấu hình chuyển log từ **Amazon EC2**. [Hướng dẫn ghi log cho Lambda](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-cloudwatchlogs.html).

**Cảnh báo mẫu.** Vào **Alarms → Create alarm**, chọn `Errors` của hàm thu thập, dùng thống kê `Sum` trong 5 phút và đặt ngưỡng `≥ 1` với điều kiện 1/1 điểm dữ liệu. Chọn `Treat missing data as good (not breaching)` để khoảng thời gian không có lượt chạy không tự kích hoạt cảnh báo. Để nhận email, chọn một chủ đề **Amazon SNS** làm đích thông báo khi vào trạng thái `ALARM` và xác nhận đăng ký email. Cảnh báo này theo dõi lỗi thực thi hàm. Kiểm tra trạng thái lượt crawl riêng trong Apify. [Hướng dẫn tạo cảnh báo CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Alarms.html).

### Nhật ký trên Macket Admin

**Scan History** cho biết thời gian, mã và trạng thái của từng lượt crawl, số bài thu thập, tiến độ phân loại và trích xuất, số bài hiển thị, chi phí AI và lỗi nếu có.

![Scan History trên Macket Admin với trạng thái và tiến độ từng lượt crawl](/images/5-Workshop/5.5-Policy/macket-admin-scan-history.png)

**AI Classification Logs** ghi kết quả phân loại của từng bài đã qua AI. Tìm theo **Post ID** hoặc lọc theo **Seller Type** để xem bài được gán loại người bán nào.

![AI Classification Logs trên Macket Admin với bộ lọc loại người bán và ô tìm mã bài](/images/5-Workshop/5.5-Policy/macket-admin-ai-classification-logs.png)
