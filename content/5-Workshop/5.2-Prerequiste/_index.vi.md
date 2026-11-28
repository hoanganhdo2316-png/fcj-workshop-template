---
title: "Chuẩn bị"
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
aliases:
  - /5-workshop/5.2-prerequiste/5.2.1-prepare-environment/
  - /5-workshop/5.2-prerequiste/5.2.2-prepare-source-data/
---

### Chuẩn bị môi trường
1. Chọn **Asia Pacific (Singapore)** (`ap-southeast-1`). Tạo VPC, một public subnet cho EC2 và ít nhất hai private subnet ở các Availability Zone khác nhau cho nhóm subnet của RDS.
2. Gắn Internet Gateway vào VPC và thêm default route cho public subnet. Tạo security group riêng cho EC2 và RDS. RDS chỉ nhận TCP 5432 từ security group EC2. Nếu dùng SSH, giới hạn cổng 22 theo IP quản trị.
3. Chuẩn bị [mã nguồn Macket](https://github.com/hoanganhdo2316-png/macket-aws), tên miền, tài khoản Apify, OpenAI API và một bài đăng được phép dùng để kiểm thử.

<!-- ẢNH 01: VPC, các subnet và bảng định tuyến public. -->
