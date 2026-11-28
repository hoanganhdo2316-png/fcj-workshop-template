---
draft: true
title: "Chuẩn bị môi trường AWS"
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

### Mục tiêu

Tạo mạng để ứng dụng có thể nhận kết nối và cơ sở dữ liệu nằm trong subnet riêng.

### Các bước thực hiện

1. Mở AWS Console, chọn **Asia Pacific (Singapore)**, mã `ap-southeast-1`. Dùng Region này cho EC2, RDS, S3, ECR, Lambda và Scheduler của bài thực hành.
2. Trong **Amazon VPC**, tạo VPC có DNS resolution và DNS hostnames. Tạo một public subnet cho máy chủ và ít nhất hai private subnet ở hai Availability Zone khác nhau cho DB subnet group.
3. Gắn Internet Gateway vào VPC và thêm default route đến gateway trong bảng định tuyến của public subnet. Các subnet DB giữ riêng tư.
4. Tạo hai security group. Nhóm máy chủ cho phép HTTPS và SSH từ IP quản trị nếu dùng SSH. Nhóm DB chỉ nhận TCP `5432` từ security group của máy chủ.

<!-- ẢNH 01: 01-vpc.png — Sơ đồ VPC hoặc danh sách subnet kèm bảng route public. -->

### Kết quả mong đợi

VPC và subnet sẵn sàng cho EC2 và RDS.
