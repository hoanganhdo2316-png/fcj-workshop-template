---
title: "Blog 3"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

## Xây dựng pipeline thu thập dữ liệu mạng xã hội trên AWS

[Đọc bài viết trên Facebook](https://www.facebook.com/share/p/1A4VmUbcda/)

![Sơ đồ thu thập dữ liệu Twitter và Reddit, hợp nhất và lưu trữ trên AWS](/images/3-BlogsPosted/blog-3-social-media-pipeline.png)

[Sơ đồ kiến trúc gốc của AWS (PDF)](https://d1.awsstatic.com/solutions/guidance/architecture-diagrams/social-media-data-pipeline-on-aws.pdf)

Các nền tảng mạng xã hội tạo ra lượng lớn dữ liệu mỗi ngày. Đối với doanh nghiệp, những bài đăng và bình luận này có thể cung cấp thông tin về xu hướng thị trường, phản hồi khách hàng và mức độ quan tâm đối với sản phẩm.

Tuy nhiên, việc thu thập dữ liệu từ nhiều nền tảng không đơn giản. Mỗi nền tảng có API, giới hạn truy cập và định dạng dữ liệu riêng. Nếu xây dựng một hệ thống thu thập dữ liệu trực tiếp mà không có cơ chế điều phối, ứng dụng dễ gặp tình trạng API throttling, lỗi xử lý hoặc khó mở rộng khi lượng dữ liệu tăng lên.

Guidance for Social Media Data Pipeline on AWS giới thiệu một kiến trúc serverless, event-driven để tự động thu thập, chuẩn hóa và lưu trữ dữ liệu mạng xã hội trên AWS.

### 1. Tổng quan kiến trúc

Mục tiêu của kiến trúc là xây dựng một pipeline có khả năng:

- Thu thập dữ liệu từ nhiều nguồn mạng xã hội.
- Xử lý các giới hạn API và lỗi trong quá trình thu thập.
- Chuẩn hóa dữ liệu về một định dạng chung.
- Lưu trữ dữ liệu tập trung trên **Amazon S3**.
- Cung cấp dữ liệu cho các hệ thống phân tích và báo cáo.

Kiến trúc tham khảo sử dụng Twitter và Reddit làm nguồn dữ liệu. Các dịch vụ AWS được tổ chức theo mô hình event-driven, trong đó các thành phần giao tiếp với nhau thông qua **EventBridge**, **SQS** và **Step Functions**.

### 2. Quy trình hoạt động

#### Bước 1 – Thu thập xu hướng từ mạng xã hội

**Amazon EventBridge** kích hoạt **AWS Lambda** theo lịch để lấy các xu hướng (trends) từ Twitter theo từng khu vực địa lý.

Kết quả được lưu vào **Amazon DynamoDB**. **AWS Systems Manager Parameter Store** lưu các thông tin cấu hình cần thiết như khu vực và credentials.

#### Bước 2 – Phát sự kiện để xử lý

Các **Lambda** functions chạy theo lịch tiếp tục lấy danh sách trends từ **DynamoDB** và gửi chúng đến **EventBridge** Event Bus.

**EventBridge** đóng vai trò trung gian phân phối sự kiện, giúp tách biệt quá trình phát hiện xu hướng khỏi các bước thu thập nội dung chi tiết.

#### Bước 3 – Điều phối quá trình thu thập

Khi nhận được sự kiện, **EventBridge** kích hoạt **AWS Step Functions**.

**Step Functions** kiểm tra và theo dõi yêu cầu, sau đó đưa message vào **Amazon SQS** để chuẩn bị thu thập tweets.

**SQS** đặc biệt quan trọng ở bước này vì Twitter API có giới hạn số lượng request. Hàng đợi đệm yêu cầu để bên xử lý có thể giới hạn tốc độ gọi API và thử lại khi gặp lỗi tạm thời.

#### Bước 4 – Lấy dữ liệu bài đăng

**AWS Lambda** lấy các message từ **SQS**, gọi Search API để thu thập tweets, sau đó chuyển đổi dữ liệu và phát sự kiện trở lại **EventBridge**.

**Amazon DynamoDB** lưu các pagination token và thông tin liên quan để hệ thống có thể tiếp tục lấy dữ liệu từ vị trí trước đó mà không phải bắt đầu lại toàn bộ quá trình.

#### Bước 5 – Thu thập dữ liệu từ Reddit

Một **Lambda** function khác được **EventBridge** kích hoạt để lấy các subreddit phổ biến và bình luận tương ứng từ Reddit.

**DynamoDB** lưu trạng thái tìm kiếm (search state), giúp hệ thống theo dõi tiến trình thu thập dữ liệu.

#### Bước 6 – Hợp nhất dữ liệu từ nhiều nguồn

Sau khi thu thập, các sự kiện từ Twitter và Reddit đều được gửi về **EventBridge** dưới một định dạng chung.

Đây là bước quan trọng vì dữ liệu từ các nền tảng khác nhau có thể có cấu trúc và cách biểu diễn khác nhau. Việc chuẩn hóa giúp những bước xử lý phía sau không cần phụ thuộc vào nguồn dữ liệu ban đầu.

#### Bước 7 – Kiểm tra và chuyển đổi dữ liệu

Mỗi message được **EventBridge** tiếp nhận sẽ kích hoạt một workflow trên **Step Functions**.

Workflow thực hiện các tác vụ:

- Validate: kiểm tra tính hợp lệ của dữ liệu.
- Transform: chuyển đổi dữ liệu về cấu trúc mong muốn.
- Load: đưa dữ liệu đến vị trí lưu trữ tương ứng.

Việc sử dụng **Step Functions** giúp quy trình xử lý nhiều bước dễ theo dõi và quản lý lỗi hơn.

#### Bước 8 – Nhận diện ngôn ngữ

**Amazon Comprehend** được sử dụng để nhận diện ngôn ngữ của nội dung nếu message chưa có thông tin này.

Metadata về ngôn ngữ giúp hệ thống phân loại dữ liệu tốt hơn và hỗ trợ những hoạt động phân tích tiếp theo.

#### Bước 9 – Lưu trữ dữ liệu trên Amazon S3

**Amazon S3** đóng vai trò kho lưu trữ trung tâm (corpus) của pipeline.

Dữ liệu sau khi thu thập và chuẩn hóa được lưu tại đây, tạo nền tảng cho các hệ thống phân tích dữ liệu, machine learning hoặc báo cáo trong tương lai.

#### Bước 10 – Mở rộng sang hệ thống phân tích

Đối với những workload cần phân tích sâu hơn, kiến trúc có thể sử dụng **Amazon Kinesis** để chuyển metadata đến các hệ thống khác như **Amazon Redshift**.

Đây là thành phần mở rộng tùy chọn, không bắt buộc đối với pipeline thu thập và lưu trữ cơ bản.

### 3. Vai trò của các dịch vụ AWS

- **EventBridge**: Lên lịch và định tuyến sự kiện
- **Lambda**: Thu thập và xử lý dữ liệu
- **DynamoDB**: Lưu trends, pagination token và search state
- **Parameter Store**: Lưu cấu hình và credentials
- **Step Functions**: Điều phối workflow nhiều bước
- **SQS**: Xếp hàng và kiểm soát tốc độ xử lý
- **Comprehend**: Nhận diện ngôn ngữ
- **S3**: Lưu trữ dữ liệu tập trung
- **Kinesis**: Truyền metadata đến hệ thống phân tích
- **Redshift**: Phân tích dữ liệu bằng data warehouse

### 4. Một số vấn đề System Design đáng chú ý

#### Xử lý API throttling bằng SQS

Một trong những khó khăn lớn nhất khi thu thập dữ liệu từ mạng xã hội là giới hạn request của API. Thay vì để **Lambda** gọi API liên tục, **SQS** đóng vai trò buffer giữa yêu cầu thu thập và quá trình thực thi.

Bên xử lý có thể giới hạn tốc độ gọi API và thử lại khi gặp lỗi tạm thời.

#### Sử dụng EventBridge để giảm coupling

Nếu mỗi thành phần phải gọi trực tiếp thành phần tiếp theo, việc thay đổi hoặc bổ sung nguồn dữ liệu sẽ khiến hệ thống phức tạp hơn.

**EventBridge** cho phép các thành phần phát và nhận sự kiện độc lập. Khi muốn bổ sung một nguồn dữ liệu mới, hệ thống có thể thêm một ingestion workflow mà không cần thay đổi toàn bộ pipeline.

#### Tách biệt dữ liệu nghiệp vụ và dữ liệu thu thập

**DynamoDB** được sử dụng để lưu trạng thái và metadata phục vụ quá trình thu thập, trong khi **S3** lưu trữ corpus dữ liệu.

Sự phân chia này giúp mỗi dịch vụ đảm nhiệm đúng mục đích: **DynamoDB** hỗ trợ truy cập nhanh vào trạng thái, còn **S3** phù hợp với việc lưu trữ khối lượng dữ liệu lớn với chi phí hợp lý.

### 5. Bài học về System Design

Guidance for Social Media Data Pipeline on AWS minh họa cách xây dựng một hệ thống thu thập dữ liệu phân tán bằng kiến trúc serverless và event-driven.

Thay vì tạo một ứng dụng duy nhất thực hiện toàn bộ công việc, pipeline được chia thành những thành phần độc lập:

Data Sources → Ingestion → Event Routing → Processing → Data Lake → Analytics.

Trong đó:

- **EventBridge** đảm nhiệm việc phát và định tuyến sự kiện.
- **SQS** giúp kiểm soát tốc độ xử lý và giảm ảnh hưởng của API throttling.
- **Step Functions** quản lý các workflow nhiều bước.
- **Lambda** thực hiện các tác vụ thu thập và chuyển đổi dữ liệu.
- **DynamoDB** lưu trạng thái thu thập.
- **S3** cung cấp kho dữ liệu tập trung.

Một điểm cần lưu ý khi áp dụng kiến trúc này hiện nay là phần tích hợp Twitter trong tài liệu tham khảo được xây dựng theo API và điều kiện truy cập tại thời điểm thiết kế. Khi triển khai thực tế, cần kiểm tra lại API, quyền truy cập và giới hạn hiện hành của từng nền tảng. Kiến trúc cũng có thể được điều chỉnh để tích hợp Facebook, Instagram hoặc các nguồn dữ liệu khác nếu có API và quyền truy cập phù hợp.

Bài học quan trọng: Một pipeline dữ liệu tốt không chỉ cần thu thập được dữ liệu, mà còn phải kiểm soát được tốc độ, trạng thái, lỗi và khả năng mở rộng khi số lượng nguồn dữ liệu tăng lên.

### Tài liệu tham khảo

- [Guidance for Social Media Data Pipeline on AWS](https://docs.aws.amazon.com/solutions/social-media-data-pipeline-on-aws/)
- [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/)
- [Amazon SQS](https://docs.aws.amazon.com/sqs/)
- [AWS Step Functions](https://docs.aws.amazon.com/step-functions/)
- [Amazon DynamoDB](https://docs.aws.amazon.com/dynamodb/)
- [Amazon S3](https://docs.aws.amazon.com/s3/)
