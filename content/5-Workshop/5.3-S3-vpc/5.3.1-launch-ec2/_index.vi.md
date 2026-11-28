---
draft: true
title: "Tạo máy chủ EC2 tại Singapore"
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

### Mục tiêu

Dựng máy chủ Ubuntu để chạy backend và giao diện Macket.

### Các bước thực hiện

1. Trong **Amazon EC2** tại Singapore, chọn **Launch instances**. Chọn Ubuntu Server 24.04 LTS, kiến trúc x86-64 và dung lượng phù hợp cho môi trường thực hành. Có thể bắt đầu với instance dòng micro để kiểm tra hạ tầng.
2. Chọn VPC và public subnet đã tạo, gắn security group ứng dụng. Chọn key pair nếu dùng SSH. Gắn IAM instance role để cấp quyền tới bucket ảnh sau khi tạo bucket.
3. Khởi tạo instance và đợi status checks đạt. Kết nối qua SSH từ IP quản trị đã cho phép, hoặc dùng Session Manager nếu đã cấu hình.
4. Trên Ubuntu, cài Git, Python venv, PostgreSQL client và Nginx. Cài môi trường build frontend hoặc dùng bản build từ máy phát triển.

[Hướng dẫn tạo và kết nối EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)

<!-- ẢNH 02: 02-ec2.png — EC2 ở Singapore, trạng thái Running và subnet đã chọn. -->

### Kết quả mong đợi

Kết nối được vào máy chủ Ubuntu để triển khai ứng dụng.
