---
title: "Triển khai FastAPI, Nginx và HTTPS"
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

### Mục tiêu

Đưa backend và giao diện lên EC2 và cung cấp truy cập HTTPS.

### Các bước thực hiện

1. Đưa mã nguồn lên EC2. Tạo Python virtual environment trong `backend/` và cài dependencies từ `requirements.txt`. Cấu hình `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER` và `DB_PASSWORD` trong tệp môi trường riêng. Nếu dùng Admin để theo dõi lượt quét, cấu hình thêm `ADMIN_API_TOKEN` ở backend.
2. Chạy Uvicorn với entry point `main:app` trên `127.0.0.1:8000`. Tạo dịch vụ systemd cho backend với thư mục làm việc là `backend/` và tệp môi trường ở bước trước. Kiểm tra `/api/health` và `/api/db-health` từ EC2 trước khi cấu hình proxy.
3. Trong `frontend/`, chạy `npm ci` và `npm run build`. Cấu hình Nginx phục vụ thư mục `dist/`, dùng `index.html` cho route giao diện và reverse proxy `/api/` sang Uvicorn. Hạn chế truy cập công khai vào endpoint kiểm tra DB.
4. Trỏ tên miền demo về máy chủ ở nhà cung cấp DNS đang quản lý domain. Cấp chứng chỉ HTTPS cho hostname, kiểm tra cấu hình Nginx và mở trang qua HTTPS.

<!-- ẢNH 04: 04-web-health.png — Giao diện qua HTTPS và phản hồi /api/health. -->

### Kết quả mong đợi

Giao diện mở được qua HTTPS và FastAPI kết nối được RDS.
