---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## Macket

Nền tảng dùng AI để giúp cửa hàng và reseller tìm nguồn MacBook đã qua sử dụng trên Facebook.

[Mã nguồn Macket trên GitHub](https://github.com/hoanganhdo2316-png/macket-aws)

## 1. Tóm tắt dự án

Cửa hàng và reseller muốn thu mua thiết bị trực tiếp từ người dùng cá nhân thường phải đọc nhiều bài đăng của những người bán thương mại khác. Macket tập trung vào thị trường Apple MacBook đã qua sử dụng và giúp người thu mua tìm các bài đăng có khả năng đến từ người bán máy chính chủ.

Hệ thống thu thập bài đăng từ các nhóm Facebook, phát hiện nội dung trùng lặp và dùng mô hình ngôn ngữ lớn (LLM) để phân loại người bán. Người thu mua có thể lọc kết quả và mở bài đăng gốc để liên hệ.

Macket được thiết kế cho AWS Region Singapore. Dự án hiện dùng OpenAI API để phân loại và sẽ xem xét **Amazon Bedrock** khi có quyền truy cập mô hình phù hợp.

## 2. Vấn đề cần giải quyết

**Khó khăn hiện tại**

- Bài đăng phân tán trong nhiều nhóm Facebook. Người thu mua phải theo dõi từng nhóm, trong khi những máy có giá tốt có thể đã được bán trước khi họ thấy bài đăng.
- Model, cấu hình, giá và tình trạng máy thường được viết dưới dạng văn bản tự do. Cùng một bài đăng có thể xuất hiện ở nhiều nhóm.
- Bài đăng của reseller có thể giống bài thanh lý của cá nhân, khiến người thu mua khó xác định nguồn hàng phù hợp.

**Giải pháp đề xuất**

- Thu thập bài đăng theo lịch và phát hiện dữ liệu trùng lặp trước khi xử lý.
- Dùng system prompt để xác định tiêu chí phân loại người bán. Đối chiếu UID trước để tránh dùng LLM phân loại lại các reseller đã biết.
- Cung cấp giao diện tìm kiếm, lọc và mở bài đăng gốc.

## 3. Kiến trúc giải pháp

Kiến trúc mục tiêu sử dụng AWS Region Asia Pacific (Singapore), `ap-southeast-1`. Kiến trúc bao gồm hạ tầng đã triển khai và các thành phần dự kiến cho những giai đoạn tiếp theo.

![Sơ đồ kiến trúc giải pháp Macket trên AWS](/images/2-Proposal/macket-aws-architecture.png)

**Các thành phần chính**

- **Amazon Route 53**, **Amazon CloudFront** và **AWS WAF** phụ trách DNS, phân phối và lưu đệm nội dung, đồng thời kiểm tra yêu cầu HTTP trong kiến trúc mục tiêu.
- **Amazon VPC** phân tách subnet công khai của ứng dụng và subnet riêng của cơ sở dữ liệu. Internet gateway cung cấp kết nối Internet cho máy chủ ứng dụng. Security group giới hạn quyền truy cập cơ sở dữ liệu ở lớp ứng dụng.
- **Amazon EC2** chạy frontend, backend FastAPI, Nginx reverse proxy, AI Worker, logic tìm kiếm và quản lý người dùng. Giai đoạn đầu dùng một instance để kiểm soát chi phí.
- **Amazon RDS for PostgreSQL** lưu bài đăng, thông tin sản phẩm, nguồn thu thập, UID người bán, tài khoản và kết quả phân loại. **Amazon S3** lưu ảnh và dữ liệu phi cấu trúc. **S3 gateway endpoint** cho phép lưu lượng phù hợp từ **EC2** đến **S3** đi qua mạng AWS.
- **Amazon EventBridge Scheduler** và **AWS Lambda** được thiết kế để lên lịch và điều phối việc thu thập. **Lambda** gửi yêu cầu chạy tác vụ cho Apify, sau đó dịch vụ thu thập chuyển bài đăng về backend API.
- **AWS IAM** và **AWS Secrets Manager** quản lý quyền truy cập và thông tin bí mật. **Amazon CloudWatch** và **AWS CloudTrail** đảm nhiệm giám sát vận hành và ghi nhận hoạt động API trong kiến trúc mục tiêu. Zalo OAuth là phương thức đăng nhập dự kiến.

Sau khi nhận dữ liệu, backend kiểm tra bài đăng trùng lặp và đối chiếu UID với danh sách reseller đã biết. AI Worker gửi những bài còn cần phân loại đến mô hình và nhận kết quả JSON. Hệ thống lưu kết quả trong **Amazon RDS**, ghi nhận UID của reseller mới để tái sử dụng và hiển thị các bài phù hợp trên web.

## 4. Triển khai kỹ thuật

- **Ứng dụng và dữ liệu.** Hạ tầng đã triển khai gồm **Amazon EC2**, FastAPI, Nginx, tên miền có HTTPS và **Amazon RDS for PostgreSQL**. Dự án đã kiểm tra kết nối từ backend đến cơ sở dữ liệu và bảng định tuyến của **S3 gateway endpoint**.
- **Thu thập và phân loại.** Dự án đã kiểm thử thu thập bài đăng qua Apify. AI Worker gửi caption và tiêu chí phân loại đến OpenAI API. System prompt dùng một tập nhỏ ví dụ thực tế từ người bán cá nhân và reseller. Độ chính xác cần được đánh giá trên bài đăng mới.
- **Thông tin bí mật.** **AWS Secrets Manager** lưu API key của OpenAI và Apify cùng mật khẩu cơ sở dữ liệu.
- **Triển khai và vận hành.** Phương án triển khai sử dụng Docker, GitHub Actions và **Amazon ECR** để đóng gói, quản lý phiên bản và triển khai lên **Amazon EC2** hoặc **AWS Lambda**. CI/CD hoàn chỉnh, thu thập theo lịch, tự động hóa toàn bộ luồng AI, giám sát tập trung và bảo mật nâng cao vẫn đang được hoàn thiện.

## 5. Lộ trình phát triển

**Giai đoạn 1 · Hạ tầng nền tảng**

- Thiết lập **EC2**, **RDS**, **VPC** và **S3**.
- Cấu hình tên miền, HTTPS và Backend API.
- Thiết lập Public Subnet, Private DB Subnet và Security Group.
- Tích hợp **S3 Gateway Endpoint**.
- Kiểm thử kết nối giữa các thành phần.
**Giai đoạn 2 · Tự động hóa thu thập dữ liệu**

- Hoàn thiện module thu thập bài đăng.
- Tích hợp dịch vụ crawling bên ngoài.
- Xây dựng cơ chế phát hiện dữ liệu trùng lặp.
- Tích hợp **EventBridge Scheduler** và **AWS Lambda**.
- Hoàn thiện quá trình lưu trữ bài đăng và hình ảnh.
**Giai đoạn 3 · Xử lý và phân loại bằng AI**

- Tích hợp LLM qua OpenAI API và xem xét chuyển sang **Amazon Bedrock** khi có quyền truy cập mô hình phù hợp.
- Xây dựng System Prompt quy định các tiêu chí phân loại người bán.
- Hoàn thiện AI Worker tự động phân tích nội dung bài đăng.
- Kiểm thử độ chính xác của kết quả phân loại trên dữ liệu thực tế.
- Tối ưu System Prompt và chi phí sử dụng mô hình AI.
**Giai đoạn 4 · Phát triển ứng dụng**

- Hoàn thiện giao diện người dùng.
- Phát triển các bộ lọc bài đăng và người bán.
- Tích hợp Zalo OAuth.
- Tối ưu trải nghiệm tìm kiếm và hiển thị kết quả.
- Thực hiện kiểm thử chức năng toàn hệ thống.
**Giai đoạn 5 · Bảo mật và tối ưu vận hành**

- Hoàn thiện quy trình CI/CD với GitHub Actions và **Amazon ECR**.
- Tích hợp **CloudFront** và **AWS WAF**.
- Hoàn thiện quản lý quyền truy cập với **IAM** và **Secrets Manager**.
- Thiết lập giám sát thông qua **CloudWatch** và **CloudTrail**.
- Thực hiện kiểm thử tải và đánh giá khả năng mở rộng.
- Đánh giá và tối ưu chi phí vận hành AWS.
## 6. Dự toán chi phí

Kiến trúc Macket được thiết kế theo hướng tiết kiệm chi phí trong giai đoạn phát triển ban đầu, đồng thời duy trì khả năng mở rộng khi số lượng người dùng tăng lên.

Dự toán sơ bộ dưới đây sử dụng Region Singapore, mô hình On-Demand, một **EC2** instance và một **RDS** instance Single-AZ.

| Dịch vụ | Chi phí dự kiến/tháng (USD) |
| --- | ---: |
| **Amazon EC2** và **EBS** | 13–23 |
| **Amazon RDS** PostgreSQL | 20–27 |
| **Amazon S3** | 1–3 |
| **Amazon ECR** | 0.5–2 |
| **Amazon CloudFront** | 0–3 |
| **AWS WAF** | 6–10 |
| **Amazon Route 53** | 0.5–1 |
| **AWS Lambda** và **EventBridge** | 0–1 |
| **AWS Secrets Manager** | 1–3 |
| **Amazon CloudWatch** | 1–4 |
| **AWS CloudTrail** | 0–2 |
| Public IPv4 | 3.65 |
| **S3 Gateway Endpoint** | 0 |
| Tổng chi phí dự kiến | 46.65–82.65 |

Dự toán chưa bao gồm chi phí **Amazon Bedrock**, OpenAI API, dịch vụ crawling bên ngoài, thuế và đăng ký tên miền. Chi phí thực tế phụ thuộc vào cấu hình tài nguyên và mức độ sử dụng.

### Chi phí ghi nhận trên AWS Cost Explorer

Ảnh chụp ngày **28/11/2026** cho thấy tổng chi phí **25,66 USD** trong phạm vi báo cáo đang chọn. **Amazon RDS** chiếm phần lớn nhất (13,6153 USD), tiếp theo là **Amazon EC2 Compute** (5,6670 USD). Khoảng ngày của bộ lọc không hiển thị trong ảnh, nên số liệu này được trình bày riêng với dự toán theo tháng ở trên.

![Biểu đồ chi phí AWS theo dịch vụ, tổng 25,66 USD](/images/2-Proposal/macket-cost-explorer-overview.png)

![Các khoản chi phí theo dịch vụ hiển thị trong AWS Cost Explorer](/images/2-Proposal/macket-cost-explorer-by-service.png)

**Chiến lược tối ưu chi phí**

- Sử dụng một **EC2** instance trong giai đoạn đầu.
- Sử dụng **RDS** Single-AZ khi chưa có yêu cầu tính sẵn sàng cao.
- Sử dụng **Lambda** cho các tác vụ điều phối ngắn hạn thay vì duy trì máy chủ riêng.
- Sử dụng **S3 Gateway Endpoint** miễn phí.
- Chỉ gọi LLM cho bài đăng mới cần phân loại, tận dụng danh sách UID của reseller đã ghi nhận.
- Tối ưu System Prompt để giảm lượng token không cần thiết.
- Lựa chọn mô hình AI phù hợp với tác vụ phân loại.
- Không triển khai **NAT Gateway** và các **Interface Endpoint** có phí khi chưa có nhu cầu thực tế.
- Theo dõi chi phí vận hành định kỳ.
## 7. Đánh giá rủi ro

| Rủi ro | Ảnh hưởng | Biện pháp giảm thiểu |
| --- | --- | --- |
| Thay đổi chính sách hoặc cấu trúc dữ liệu Facebook | Gián đoạn thu thập dữ liệu | Tách riêng module crawling, sử dụng nguồn dữ liệu được phép truy cập |
| Phụ thuộc vào dịch vụ crawling bên ngoài | Không cập nhật được bài đăng mới | Thiết lập cơ chế retry, ghi log và chuẩn hóa giao diện tích hợp |
| LLM phân loại người bán không chính xác | Bỏ sót nguồn hàng tiềm năng | Đánh giá kết quả trên tập dữ liệu mẫu và cải thiện System Prompt |
| Dữ liệu trùng lặp | Tăng chi phí lưu trữ và xử lý AI | Phát hiện trùng lặp trước khi xử lý |
| Rò rỉ API Key hoặc thông tin xác thực | Truy cập trái phép, phát sinh chi phí | **IAM** Role, **Secrets Manager** và nguyên tắc cấp quyền tối thiểu |
| Truy cập trái phép vào cơ sở dữ liệu | Mất hoặc rò rỉ dữ liệu | Private Subnet, Security Group, mã hóa kết nối và sao lưu |
| **EC2** hoặc **RDS** gặp sự cố | Gián đoạn dịch vụ | Sao lưu định kỳ, giám sát và xây dựng quy trình phục hồi |
| Chi phí AI và crawling tăng nhanh | Vượt ngân sách | Giới hạn số lượng bài đăng và kiểm soát mức sử dụng |
| Quyền riêng tư và quyền sử dụng nội dung | Rủi ro pháp lý | Tuân thủ quyền truy cập dữ liệu, hạn chế lưu thông tin cá nhân và dẫn nguồn bài đăng gốc |

## 8. Kết quả kỳ vọng

### 8.1. Đối với người dùng

Macket hướng đến việc giúp các cửa hàng và reseller giảm đáng kể thời gian tìm kiếm nguồn hàng, hạn chế việc phải theo dõi thủ công nhiều hội nhóm Facebook và tăng khả năng tiếp cận những người dùng có nhu cầu bán MacBook chính chủ.

### 8.2. Đối với hệ thống

- Tự động thu thập và cập nhật bài đăng định kỳ.
- Phân loại người bán cá nhân và reseller bằng LLM.
- Cho phép điều chỉnh các tiêu chí phân loại thông qua System Prompt.
- Cung cấp giao diện tập trung để tìm kiếm và sàng lọc bài đăng.
- Phân tách tài nguyên tính toán, cơ sở dữ liệu và lưu trữ.
- Tự động hóa quá trình triển khai ứng dụng.
- Giám sát hoạt động, kiểm soát quyền truy cập và tối ưu chi phí vận hành.

Sau khi hoàn thiện và kiểm chứng hiệu quả trên thị trường MacBook đã qua sử dụng, kiến trúc của Macket có thể được mở rộng sang những nhóm sản phẩm công nghệ khác.

### 8.3. Đối với quá trình học tập và phát triển chuyên môn

Thông qua dự án Macket, tôi có cơ hội áp dụng kiến thức về AWS vào một bài toán thực tế, từ xây dựng hạ tầng mạng, triển khai ứng dụng, quản lý cơ sở dữ liệu đến tích hợp trí tuệ nhân tạo và tự động hóa vận hành.

Dự án giúp tôi phát triển tư duy thiết kế hệ thống, cân bằng giữa yêu cầu nghiệp vụ, bảo mật, hiệu suất, khả năng mở rộng và chi phí, thay vì chỉ tập trung vào phát triển các tính năng phần mềm.
