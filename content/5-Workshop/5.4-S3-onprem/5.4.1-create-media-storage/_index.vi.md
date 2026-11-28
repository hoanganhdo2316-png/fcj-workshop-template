---
draft: true
title: "Tạo kho ảnh S3 riêng tư"
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

### Mục tiêu

Thiết lập lưu ảnh riêng tư và cấp quyền truy cập theo vai trò của máy chủ.

### Các bước thực hiện

1. Trong **Amazon S3**, tạo bucket ảnh ở Singapore và giữ **Block Public Access** bật. Cấp cho IAM role của EC2 quyền đọc và ghi trong phạm vi bucket và prefix ảnh.
2. Cấu hình backend và Media Worker trỏ tới bucket. Sau khi nhận bài đủ điều kiện, worker lưu ảnh vào S3 và ghi key ảnh vào PostgreSQL.
3. Backend trả URL ký sẵn để trình duyệt đọc ảnh. Kiểm tra ảnh hiển thị trên giao diện và biến cấu hình đang trỏ đúng bucket. Nếu chọn thêm S3 gateway endpoint theo sơ đồ mục tiêu, gắn endpoint vào bảng định tuyến dùng bởi EC2 và xác nhận lại kết nối.

<!-- ẢNH 05: 05-s3.png — Bucket giữ Block Public Access bật và đối tượng ảnh của bài mẫu. -->

### Kết quả mong đợi

Worker có thể lưu ảnh và giao diện đọc được ảnh qua URL ký sẵn.
