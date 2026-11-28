---
draft: true
title: "Cấu hình AI Worker"
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

### Mục tiêu

Thiết lập phân loại người bán và trích xuất dữ liệu sản phẩm.

### Các bước thực hiện

1. Thiết lập khóa OpenAI API qua cấu hình riêng của ứng dụng và chuẩn bị system prompt phân loại. Dùng một bài mẫu có nhãn mong đợi để đối chiếu.
2. Cấu hình chạy `backend/ai_pipeline.py` và Media Worker trong môi trường backend. Nếu dùng bộ triển khai container của repo, xem các tệp `deploy/macket-ai-pipeline*` để thiết lập runner và systemd timer trên EC2. Đặt giới hạn request và ngân sách trước khi bật xử lý theo lô.
3. Cho Stage 1 xác định `post_type` và `seller_type`. Chuyển bài bán của cá nhân đủ điều kiện sang Stage 2 để trích xuất mẫu máy, cấu hình, giá và tình trạng. Sau đó đối chiếu kết quả và ảnh với bản ghi gốc trước khi kiểm tra hiển thị.

<!-- ẢNH 07: 07-ai.png — Kết quả Stage 1 và Stage 2 của cùng bài mẫu, hoặc trạng thái xử lý thực tế. -->

### Kết quả mong đợi

Bài mẫu có kết quả phân loại và, nếu đủ điều kiện, dữ liệu sản phẩm được trích xuất.
