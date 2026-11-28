---
title: "Dựng EC2, RDS và ứng dụng"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
aliases:
  - /5-workshop/5.3-s3-vpc/5.3.1-launch-ec2/
  - /5-workshop/5.3-s3-vpc/5.3.2-create-rds/
  - /5-workshop/5.3-s3-vpc/5.3.3-deploy-web/
---

### Bước 1. Tạo EC2
Trong **EC2 → Launch instances**, chọn Ubuntu Server 24.04 LTS x86-64 ở Singapore. Gắn public subnet, security group ứng dụng và IAM role của máy chủ. Khi status checks đạt, kết nối để cài Git, Python, PostgreSQL client và Nginx.

![Màn hình cấu hình khởi tạo EC2 với Ubuntu Server 24.04 LTS x86-64 và loại instance t3.micro](/images/5-Workshop/5.3-S3-vpc/macket-ec2-launch.png)

*Cấu hình mẫu trước khi khởi tạo: Ubuntu Server 24.04 LTS x86-64, `t3.micro` và 8 GiB lưu trữ.*

### Bước 2. Tạo RDS PostgreSQL
Tạo DB subnet group từ các private subnet. Trong **RDS → Create database**, chọn PostgreSQL, Single-AZ và **Public access: No**. Gắn security group DB. Từ EC2, kết nối bằng `sslmode=require`, chạy `SELECT 1;` và chuẩn bị schema theo phiên bản mã nguồn.

<!-- ẢNH 03: RDS riêng tư hoặc SELECT 1 từ EC2. -->

### Bước 3. Triển khai ứng dụng
Cài `backend/requirements.txt`, đặt các biến DB trong tệp môi trường riêng và chạy FastAPI với Uvicorn `main:app` trên `127.0.0.1:8000`. Build frontend bằng `npm ci` và `npm run build`. Dùng Nginx phục vụ `dist/` và proxy `/api/` tới Uvicorn. Trỏ DNS, bật HTTPS và kiểm tra `/api/health`.

<!-- ẢNH 04: Giao diện HTTPS và health check. -->
