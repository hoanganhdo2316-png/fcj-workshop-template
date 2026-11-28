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

### Cấu hình VPC thực tế của Macket

Macket sử dụng VPC mặc định tại **Asia Pacific (Singapore)** với dải IPv4 `172.31.0.0/16`. Resource map cho thấy **3 public subnet và 3 private subnet** trải trên ba Availability Zone `ap-southeast-1a`, `ap-southeast-1b` và `ap-southeast-1c`. Các private subnet dành cho RDS dùng bảng định tuyến `RDS-Pvt-rt`. Ba public subnet dùng bảng định tuyến có đường tới Internet Gateway và S3 Gateway Endpoint `macket-s3-gateway`.

![Resource map của VPC Macket với các subnet, bảng định tuyến, Internet Gateway và S3 Gateway Endpoint](/images/5-Workshop/5.2-Prerequisite/macket-vpc-resource-map.png)

### Chuẩn bị Actor trên Apify

Chọn [Facebook Groups Scraper của Monkey D. Scraping](https://apify.com/monkey.d.scraping/facebook-groups-scraper), mã Actor `monkey.d.scraping/facebook-groups-scraper`, để thu thập bài đăng từ các nhóm Facebook công khai về MacBook mà bạn được phép sử dụng.

Trong tab **Input**, nhập URL của một nhóm mẫu và đặt giới hạn số bài thấp cho lần chạy đầu. Tại thời điểm chụp, Apify Console hiển thị giá tham chiếu **từ 0,69 USD / 1.000 bài đăng**. Kiểm tra giá áp dụng trong tài khoản trước khi chạy.

![Actor Facebook Groups Scraper của Monkey D. Scraping và giá hiển thị trên Apify Console](/images/5-Workshop/5.2-Prerequisite/macket-apify-actor.png)
