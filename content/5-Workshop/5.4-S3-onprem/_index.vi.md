---
title: "Thiết lập luồng dữ liệu"
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
aliases:
  - /5-workshop/5.4-s3-onprem/5.4.1-create-media-storage/
  - /5-workshop/5.4-s3-onprem/5.4.2-connect-apify/
  - /5-workshop/5.4-s3-onprem/5.4.3-configure-ai/
---

1. **S3:** Tạo bucket ảnh tại Singapore với **Block Public Access**. Cấp IAM role của EC2 quyền trên prefix ảnh. Media Worker lưu ảnh vào bucket, còn backend cấp URL ký sẵn để giao diện đọc ảnh.
2. **Apify:** Chọn Actor, nhóm nguồn và giới hạn số bài. Cấu hình `APIFY_TOKEN` và `APIFY_WEBHOOK_TOKEN` trên backend. Trong **Integrations**, thêm **HTTP request**, chọn sự kiện **Run succeeded** (`ACTOR.RUN.SUCCEEDED`) và gửi tới `/api/webhooks/apify` qua HTTPS với header `X-Webhook-Token`.

   Khi lượt thu thập hoàn tất thành công, Apify tự gửi webhook. Backend dùng run ID trong thông báo để lấy kết quả và nhập vào RDS. Nhờ vậy, backend không cần liên tục gửi yêu cầu kiểm tra trạng thái lượt chạy.

   ![Apify Integrations với webhook HTTP được bật cho sự kiện Run succeeded](/images/5-Workshop/5.4-S3-onprem/macket-apify-webhook.png)

3. **AI Worker:** Cấu hình OpenAI API, system prompt và giới hạn xử lý. Dùng runner trong `deploy/macket-ai-pipeline*` cho triển khai container. Stage 1 phân loại bài. Bài bán của cá nhân đủ điều kiện sang Stage 2 để trích xuất dữ liệu sản phẩm trước khi xử lý ảnh.

   ![Hai giai đoạn của AI Worker: phân loại bài đăng và trích xuất thông tin](/images/5-Workshop/5.4-S3-onprem/macket-ai-worker-stages.png)

<!-- ẢNH 05: Bài mẫu trong Apify và bản ghi tương ứng trong RDS. -->
<!-- ẢNH 06: Kết quả AI và ảnh S3 của cùng bài mẫu. -->
