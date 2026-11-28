---
title: "Cấu hình Lambda, Scheduler và vận hành"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

### Mục tiêu

Thiết lập hàm kích hoạt thu thập và giám sát sau khi luồng chạy thủ công đã hoạt động.

### Các bước thực hiện

1. Build hàm kích hoạt thu thập từ `apify/Dockerfile.lambda`, đẩy image lên **Amazon ECR** tại Singapore, rồi tạo **AWS Lambda** từ image đó. Gắn execution role với các quyền cần cho hàm và ghi log.
2. Cấu hình URL và token truy cập cấu hình collector qua `COLLECTOR_CONFIG_URL` và `COLLECTOR_CONFIG_TOKEN`. Chạy thử một lần trong môi trường thực hành và đối chiếu với lượt Apify.
3. Trong **Amazon EventBridge Scheduler**, tạo lịch trỏ tới hàm và cấp quyền gọi Lambda cho lịch. Giữ lịch ở trạng thái tắt khi đang xác nhận luồng thủ công và giới hạn chi phí. Tài liệu dự án ghi nhận lịch ở trạng thái tắt tại thời điểm kiểm tra.
4. Dùng **Amazon CloudWatch** xem chỉ số EC2, RDS và log Lambda. Nếu dựng lớp CDN, trỏ **Amazon CloudFront** về origin Nginx qua HTTPS, gắn **AWS WAF** ở chế độ Count để quan sát ban đầu và cập nhật DNS tại nhà cung cấp tên miền. Kiểm tra API động và luồng xác thực qua CDN.

[Lambda container image](https://docs.aws.amazon.com/lambda/latest/dg/images-create.html) · [EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-schedule.html)

<!-- ẢNH 08: 08-lambda-scheduler.png — cấu hình nguồn image Lambda hoặc target và trạng thái thực của Scheduler. -->
<!-- ẢNH 09 (tùy chọn): 09-monitoring.png — chỉ số CloudWatch hoặc log của lượt chạy thử. -->

### Kết quả mong đợi

Đối chiếu được một lần gọi Lambda với lượt Apify tương ứng. Có thể kiểm tra riêng cấu hình lịch và màn hình giám sát.
