---
title: "Dịch vụ sử dụng"
weight: 2
chapter: false
pre: " <b> 5.1.2. </b> "
---

## Dịch vụ AWS

| Dịch vụ | Vai trò trong Macket |
| --- | --- |
| **Amazon EC2** | Chạy backend FastAPI và Nginx. |
| **AWS Lambda** | Tài nguyên gọi API Apify. Luồng cập nhật `Skip Author` được mô tả trong kiến trúc mục tiêu. |
| **Amazon RDS for PostgreSQL** | Lưu bài đăng, kết quả phân loại và danh sách `Skip Author`. |
| **Amazon S3** | Lưu ảnh của bài đăng. |
| **Amazon Route 53** | Quản lý DNS cho tên miền Macket trong kiến trúc mục tiêu. |
| **Amazon CloudFront** | Phân phối ứng dụng qua origin Nginx trên **Amazon EC2**, theo tài liệu triển khai. |
| **AWS WAF** | Đã gắn với **CloudFront** ở chế độ giám sát tại thời điểm kiểm tra trong tài liệu. |
| **Amazon VPC** | Tách subnet công khai của ứng dụng khỏi subnet riêng của cơ sở dữ liệu. |
| **Amazon Bedrock** | Phương án chạy mô hình phân loại dự kiến khi dự án có quyền truy cập mô hình phù hợp. |
| **AWS Secrets Manager** | Phương án quản lý thông tin bí mật cần xác minh trước khi đưa vào phạm vi demo. |
| **Amazon ECR** | Lưu ảnh container cho hàm thu thập **AWS Lambda**. |
| **Amazon EventBridge** | Đã tạo lịch gọi **AWS Lambda**. Lịch ở trạng thái `DISABLED` tại lần kiểm tra được ghi trong tài liệu. |
| **AWS CloudTrail** | Tài liệu ghi nhận đã xem được sự kiện quản lý AWS trong Event History. |
| **AWS IAM** | Quản lý quyền truy cập của ứng dụng và các dịch vụ đối với tài nguyên AWS. |
| **Amazon CloudWatch** | Dashboard theo dõi chỉ số **EC2** và **RDS**, gồm RAM và dung lượng đĩa EC2 qua CloudWatch Agent. |

## Dịch vụ bên ngoài

| Dịch vụ | Vai trò trong Macket |
| --- | --- |
| OpenAI API | Phân loại người bán theo system prompt và trả nhãn cho từng bài dưới dạng JSON. |
| Apify | Thu thập bài đăng từ các nhóm Facebook công khai đã cấu hình và gửi thông báo kết quả qua webhook. |
| Zalo OAuth | Phương thức đăng nhập dự kiến cho người dùng Macket. |
