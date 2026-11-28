---
draft: true
title: "Tạo RDS PostgreSQL và kết nối EC2"
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

### Mục tiêu

Tạo PostgreSQL riêng tư để lưu dữ liệu của Macket.

### Các bước thực hiện

1. Trong **Amazon RDS**, tạo DB subnet group từ các private subnet đã chuẩn bị.
2. Chọn **Create database**, engine PostgreSQL, cấu hình **Single-AZ** cho lab, VPC Macket và DB subnet group vừa tạo. Đặt **Public access** thành **No** và chọn security group DB.
3. Tạo cơ sở dữ liệu ứng dụng và cấu hình sao lưu theo môi trường. Lấy endpoint từ trang RDS và thử kết nối từ EC2 bằng PostgreSQL client với `sslmode=require`. Chạy `SELECT 1;` để kiểm tra.
4. Chuẩn bị schema tương thích với phiên bản mã nguồn. Với môi trường lab có thể dùng `tests/staging_schema.sql` làm schema kiểm thử, rồi đối chiếu các migration trong `backend/migrations/` với những phần schema đã có trước khi áp dụng theo thứ tự.

[Hướng dẫn tạo DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html)

<!-- ẢNH 03: 03-rds.png — RDS PostgreSQL Available, Public access No hoặc kết quả SELECT 1 từ EC2. -->

### Kết quả mong đợi

EC2 truy vấn được PostgreSQL từ trong VPC qua TLS.
