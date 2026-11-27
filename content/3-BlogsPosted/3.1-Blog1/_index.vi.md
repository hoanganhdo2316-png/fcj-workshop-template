---
title: "Blog 1"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## Nếu sản phẩm có hàng triệu người dùng, máy chủ có chịu nổi không?

[Đọc bài viết trên Facebook](https://www.facebook.com/share/p/1JYhk33Mvh/)

![Minh họa kiểm thử tải với người dùng đồng thời và các chỉ số máy chủ](/images/3-BlogsPosted/blog-1-load-testing.png)

*Hình ảnh do ChatGPT tạo để minh họa.*

Một máy chủ **Amazon EC2** chạy ổn định với 10 người dùng không có nghĩa là nó vẫn hoạt động tốt khi 1.000 người cùng truy cập.

Vậy làm sao biết giới hạn thực sự của máy chủ trước khi đưa ứng dụng lên production? Câu trả lời là **Load Testing (kiểm thử tải)**.

### 1. Load Testing là gì?

Load Testing mô phỏng nhiều người dùng đồng thời gửi yêu cầu đến máy chủ để đánh giá hiệu năng khi tải tăng. Việc chủ động tạo tải giúp phát hiện giới hạn của hệ thống trước khi lượng truy cập thực tế tăng cao.

Ba chỉ số quan trọng cần theo dõi:

- **RPS (Requests Per Second)** là số yêu cầu máy chủ xử lý mỗi giây.
- **Latency** là thời gian phản hồi. P95 là ngưỡng thời gian mà 95% yêu cầu phản hồi trong hoặc dưới ngưỡng đó.
- **Error Rate** là tỷ lệ yêu cầu thất bại, chẳng hạn do lỗi HTTP 5xx hoặc timeout.

Ngoài ra, cần theo dõi CPU, RAM, lưu lượng mạng và số kết nối cơ sở dữ liệu.

### 2. Làm sao thực hiện Load Test?

Một số công cụ phổ biến gồm k6, Locust và Apache JMeter. Ví dụ, với k6, bạn có thể bắt đầu với 100 người dùng ảo gửi yêu cầu đến API, sau đó tăng lên 200, 500 và 1.000 trong giới hạn kiểm thử đã đặt. Trong mỗi giai đoạn, hãy quan sát:

- Thời gian phản hồi có tăng đột biến không?
- CPU hoặc RAM có chạm ngưỡng không?
- Cơ sở dữ liệu có bị quá tải kết nối không?
- Tỷ lệ yêu cầu thất bại bắt đầu tăng từ lúc nào?

Trên AWS, bạn có thể dùng **Amazon CloudWatch** để theo dõi CPU, lưu lượng mạng và các chỉ số hạ tầng trong quá trình kiểm thử.

### 3. Máy chủ xử lý 1.000 request mỗi giây có phục vụ được 1.000 người dùng không?

Một người dùng có thể gửi nhiều yêu cầu mỗi giây hoặc chỉ vài yêu cầu mỗi phút. Một API đọc dữ liệu đơn giản cũng có thể xử lý nhiều yêu cầu hơn đáng kể so với API cần truy vấn cơ sở dữ liệu phức tạp.

Để đánh giá khả năng chịu tải, hãy mô phỏng hành vi sử dụng thực tế và đọc số RPS cùng với thời gian phản hồi và tỷ lệ lỗi.

### 4. Khi nào nên nâng cấp máy chủ?

CPU chạm 100% là một tín hiệu để điều tra. Nút thắt có thể nằm ở truy vấn SQL, chỉ mục cơ sở dữ liệu, connection pool hoặc cấu hình ứng dụng. Kiểm thử tải giúp tìm thành phần đang giới hạn hiệu năng để quyết định nên tối ưu mã, thêm caching, nâng cấp **Amazon EC2** hay triển khai **Amazon EC2 Auto Scaling**.

### Kết luận

Kiểm thử tải giúp trả lời ba câu hỏi. Máy chủ xử lý được bao nhiêu tải? Khi nào hiệu năng bắt đầu suy giảm? Thành phần nào cần tối ưu trước?

**Đừng đợi đến khi có 1.000 người dùng thật mới kiểm tra xem máy chủ có chịu nổi 1.000 người hay không.**

*Chỉ kiểm thử tải trên hệ thống bạn sở hữu hoặc được phép kiểm thử, với giới hạn tải và phương án dừng phù hợp.*

### Nguồn tham khảo

- [Grafana k6 – Load test types](https://grafana.com/docs/k6/latest/testing-guides/test-types/)
- [AWS – Performance Efficiency Pillar](https://docs.aws.amazon.com/wellarchitected/latest/framework/performance-efficiency.html)
- [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)
