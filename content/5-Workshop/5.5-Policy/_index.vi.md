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
