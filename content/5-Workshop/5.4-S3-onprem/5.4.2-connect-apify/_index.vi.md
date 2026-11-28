---
title: "Kết nối Apify với backend"
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

### Mục tiêu

Cấu hình đường nhận kết quả từ Apify và ghi bài đăng vào PostgreSQL.

### Các bước thực hiện

1. Trong Apify, chọn Actor thu thập Facebook Groups. Chuẩn bị URL nhóm công khai được phép sử dụng và giới hạn số bài theo nhóm, ưu tiên số lượng nhỏ cho lab.
2. Trên backend, cấu hình `APIFY_TOKEN` để đọc kết quả từ Apify và `APIFY_WEBHOOK_TOKEN` để xác thực callback. Trong Apify, đặt webhook gửi sự kiện `ACTOR.RUN.SUCCEEDED` qua HTTPS tới `/api/webhooks/apify`, kèm header `X-Webhook-Token` với giá trị tương ứng.
3. Chạy một lượt có kiểm soát. Kiểm tra webhook gửi mã run trong `resource.id`. Backend dùng mã đó để nhập dữ liệu của lượt thu thập vào PostgreSQL. Đối chiếu cùng mã bài và kiểm tra lượt nhập lại có tạo bản ghi trùng hay không.

<!-- ẢNH 06: 06-apify.png — Ảnh lượt Apify hoàn tất và bản ghi RDS tương ứng. Che token, thông tin xác thực và dữ liệu cá nhân. -->

### Kết quả mong đợi

Một lượt Apify tạo được bản ghi có thể đối chiếu trong RDS.
