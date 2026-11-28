---
title: "Kiểm tra sau triển khai"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

### Mục tiêu

Xác nhận môi trường vừa dựng có thể xử lý và hiển thị một bài đăng mẫu.

### Các bước thực hiện

1. Kiểm tra trang mở qua HTTPS và backend truy vấn được RDS.
2. Chạy một lượt thu thập có giới hạn. Đối chiếu mã lượt quét với bản ghi đã lưu trong PostgreSQL.
3. Kiểm tra kết quả phân loại. Với bài đủ điều kiện, đối chiếu dữ liệu trích xuất và ảnh trên S3 với bài gốc. Mở giao diện và tìm sản phẩm theo mẫu MacBook tương ứng.
4. Ghi lại bước bài mẫu đã đi tới và lỗi còn gặp. Có thể dùng Lịch sử quét trong Admin để tra lượt nếu bản triển khai có chức năng đó.

<!-- ẢNH 10: 10-storefront.png — bài mẫu trên giao diện hoặc kết quả thực tế khi lọc theo mẫu máy. -->

### Kết quả mong đợi

Có thể theo dõi bài mẫu từ lúc thu thập đến bước hoàn thành cuối cùng. Khi bài đủ điều kiện hiển thị, chụp kết quả trên giao diện khách hàng.
