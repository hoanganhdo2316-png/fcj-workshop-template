---
title: "Kiến trúc giải pháp"
weight: 3
chapter: false
pre: " <b> 5.1.3. </b> "
---

### Luồng xử lý trong kiến trúc mục tiêu

Sơ đồ mô tả phương án tổng thể. Phần thực hành bắt đầu bằng mạng, máy chủ EC2 và RDS tại Singapore, sau đó kết nối từng thành phần của pipeline.

1. **Khởi chạy thu thập.** **Amazon EventBridge Scheduler** kích hoạt một hàm **AWS Lambda** để gọi API của Apify.
2. **Thu thập bài đăng.** Apify lấy bài viết Facebook từ danh sách URL của các nhóm công khai về mua bán MacBook.
3. **Nhận và lưu kết quả.** Apify báo kết quả qua webhook. Backend nhập bài đăng vào **Amazon RDS for PostgreSQL**. Media Worker lưu ảnh cần hiển thị vào **Amazon S3**.
4. **Phân loại và trích xuất.** AI Worker gọi OpenAI API với system prompt và dữ liệu bài đăng. JSON phân loại ghi loại bài và loại người bán. Bài bán của cá nhân đủ điều kiện sang bước trích xuất thông tin sản phẩm.
5. **Loại reseller đã nhận diện.** Danh sách `Skip Author` trong cơ sở dữ liệu giúp các lượt sau bỏ qua tài khoản reseller đã biết trước khi gọi AI.
6. **Hiển thị bài hợp lệ.** FastAPI truy xuất những sản phẩm đủ điều kiện hiển thị trên giao diện Macket.

![Sơ đồ kiến trúc giải pháp Macket trên AWS](/images/2-Proposal/macket-aws-architecture.png)

Xem thêm [Proposal Macket](../../../2-proposal/) và phần [chuẩn bị môi trường AWS](../../5.2-prerequiste/).
