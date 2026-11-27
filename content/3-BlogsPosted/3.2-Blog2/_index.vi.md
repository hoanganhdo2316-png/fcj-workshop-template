---
title: "Blog 2"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

## Availability Zone trong AWS là gì và tại sao cần dùng nhiều AZ?

[Đọc bài viết trên Facebook](https://www.facebook.com/share/p/1AEqmkhJFE/)

![Sơ đồ AWS Region với một VPC và các subnet trải trên nhiều Availability Zone](/images/3-BlogsPosted/blog-2-availability-zones.png)

[Nguồn hình ảnh AWS Documentation](https://docs.aws.amazon.com/images/AWSEC2/latest/UserGuide/images/region-with-azs.png)

Khi mới bắt đầu sử dụng AWS, chúng ta thường nghe đến những khái niệm như Region, Availability Zone (AZ) hay Multi-AZ. Nhưng tại sao AWS lại phải chia hạ tầng thành nhiều khu vực nhỏ như vậy? Chẳng phải chỉ cần thuê một máy chủ **EC2** đủ mạnh là có thể vận hành ứng dụng rồi sao?

Câu trả lời nằm ở một vấn đề rất thực tế: Điều gì xảy ra nếu trung tâm dữ liệu đang chạy ứng dụng của bạn đột ngột gặp sự cố?

### 1. Availability Zone (AZ) là gì?

Để hiểu AZ, trước tiên cần biết AWS tổ chức hạ tầng toàn cầu như thế nào.

AWS chia hệ thống thành nhiều Region (khu vực địa lý), chẳng hạn Singapore (`ap-southeast-1`), Tokyo (`ap-northeast-1`) hay Sydney (`ap-southeast-2`).

Bên trong mỗi Region lại có nhiều Availability Zone (AZ).

Mỗi AZ là một hoặc nhiều trung tâm dữ liệu vật lý, có hệ thống điện, mạng và làm mát được thiết kế độc lập với các AZ khác.

Ví dụ, Region Singapore có các AZ như `ap-southeast-1a`, `ap-southeast-1b` và `ap-southeast-1c`.

Các AZ trong cùng Region được kết nối với nhau bằng mạng tốc độ cao, độ trễ thấp, nhưng vẫn được bố trí tách biệt về mặt vật lý.

Hiểu đơn giản: Region giống như một thành phố, còn AZ giống như những cụm trung tâm dữ liệu độc lập nằm trong thành phố đó.

Nếu một AZ gặp sự cố về điện hoặc hạ tầng, các AZ khác được thiết kế để hạn chế bị ảnh hưởng bởi cùng sự cố.

Đây chính là nền tảng để AWS xây dựng các hệ thống có tính sẵn sàng cao (High Availability).

### 2. Tại sao chúng ta cần nhiều AZ?

Giả sử bạn xây dựng một ứng dụng với kiến trúc đơn giản:

Người dùng → **EC2** (Backend) → **RDS** (Database)

Toàn bộ hệ thống nằm trong một AZ.

Ứng dụng có thể hoạt động hoàn toàn bình thường trong nhiều tháng. Nhưng nếu AZ đó gặp sự cố nghiêm trọng, cả backend lẫn database đều có nguy cơ không thể truy cập.

Đây là vấn đề Single Point of Failure (điểm lỗi đơn): chỉ một thành phần hoặc một vị trí hạ tầng gặp sự cố cũng có thể khiến toàn bộ dịch vụ ngừng hoạt động.

Để giải quyết, chúng ta có thể phân bổ hệ thống sang nhiều AZ.

Ví dụ:

- AZ-A: Chạy **EC2** số 1.
- AZ-B: Chạy **EC2** số 2.
- **Application Load Balancer (ALB)**: Phân phối request đến các **EC2** còn hoạt động.
- **RDS Multi-AZ**: Duy trì một bản dự phòng của database ở AZ khác và hỗ trợ tự động chuyển đổi khi có sự cố.

Khi AZ-A gặp sự cố, ALB có thể tiếp tục chuyển request đến **EC2** ở AZ-B. Nếu database chính cũng gặp lỗi, **RDS Multi-AZ** có thể thực hiện failover sang bản dự phòng.

Nhờ vậy, ứng dụng có thể tiếp tục phục vụ người dùng mà không cần chờ toàn bộ AZ-A được khôi phục.

Tuy nhiên, quá trình chuyển đổi không nhất thiết diễn ra tức thời. Database có thể mất một khoảng thời gian để failover và các kết nối hiện tại có thể cần được thiết lập lại.

### 3. Multi-AZ có phải là AWS tự động tạo nhiều máy chủ?

Đây là một nhầm lẫn khá phổ biến.

Multi-AZ không đơn giản là bật một công tắc để AWS tự nhân đôi toàn bộ hệ thống.

Với **EC2**, nếu bạn chỉ tạo một instance trong AZ-A, AWS sẽ không tự động tạo thêm một **EC2** tương tự ở AZ-B khi AZ-A gặp sự cố.

Muốn backend có khả năng chịu lỗi ở cấp AZ, bạn thường cần triển khai nhiều **EC2** ở các AZ khác nhau, kết hợp với Load Balancer và **Auto Scaling Group**.

Trong khi đó, một số dịch vụ managed của AWS đã tích hợp cơ chế Multi-AZ.

- **Amazon RDS Multi-AZ**: Có thể tự động duy trì database dự phòng và thực hiện failover.
- **Application Load Balancer**: Có thể phân phối lưu lượng đến các target thuộc nhiều AZ.
- **Amazon S3**: Với các lớp lưu trữ đa AZ phổ biến như **S3 Standard**, dữ liệu được thiết kế lưu trữ trên nhiều AZ trong một Region.

Điểm quan trọng là mỗi dịch vụ có cách triển khai và mức độ hỗ trợ Multi-AZ khác nhau. Không phải cứ sử dụng AWS là ứng dụng mặc nhiên có High Availability.

### 4. Multi-AZ khác gì Multi-Region?

Hai khái niệm này đều giúp tăng khả năng chịu lỗi, nhưng giải quyết những cấp độ rủi ro khác nhau.

Multi-AZ là triển khai hệ thống trên nhiều Availability Zone trong cùng một Region.

Ví dụ, ứng dụng có thể chạy trên hai AZ tại Singapore. Khi một AZ gặp sự cố, AZ còn lại có thể tiếp tục xử lý lưu lượng.

Ưu điểm là kết nối giữa các AZ có độ trễ thấp, thuận tiện cho việc đồng bộ dữ liệu và chuyển đổi dự phòng.

Multi-Region là triển khai hệ thống trên nhiều Region khác nhau, chẳng hạn Singapore và Tokyo.

Mục tiêu là giúp hệ thống có khả năng phục hồi ngay cả khi toàn bộ một Region gặp sự cố diện rộng.

Tuy nhiên, Multi-Region phức tạp và thường tốn kém hơn nhiều, vì phải giải quyết bài toán đồng bộ dữ liệu xuyên khu vực, định tuyến lưu lượng, tính nhất quán và khôi phục dịch vụ.

Nói ngắn gọn: Multi-AZ giúp chống chịu sự cố ở cấp AZ, còn Multi-Region hướng đến khả năng chống chịu sự cố ở cấp Region.

### 5. Có phải dự án nào cũng nên sử dụng Multi-AZ?

Không hẳn.

Multi-AZ giúp tăng độ sẵn sàng nhưng cũng đi kèm chi phí bổ sung.

Chẳng hạn, nếu triển khai hai **EC2** thay vì một, bạn phải trả tiền cho cả hai instance. **RDS Multi-AZ** cũng thường đắt hơn cấu hình Single-AZ tương đương. Ngoài ra, truyền dữ liệu giữa các AZ có thể phát sinh phí.

Với một dự án cá nhân, MVP hoặc ứng dụng có ít người dùng, việc chạy một **EC2** và một database Single-AZ có thể là lựa chọn hợp lý để tiết kiệm chi phí.

Ngược lại, với hệ thống thương mại điện tử, tài chính hoặc ứng dụng cần phục vụ liên tục, Multi-AZ thường là một khoản đầu tư đáng cân nhắc.

Điều quan trọng không phải là sử dụng càng nhiều AZ càng tốt, mà là xác định ứng dụng của bạn có thể chấp nhận ngừng hoạt động trong bao lâu.

Nếu hệ thống có thể ngừng vài giờ để khôi phục, Single-AZ kết hợp backup và kế hoạch phục hồi có thể đã đủ.

Nếu chỉ vài phút downtime cũng gây thiệt hại lớn, Multi-AZ trở nên có giá trị hơn nhiều.

### 6. Multi-AZ có đảm bảo hệ thống không bao giờ sập?

Không.

Multi-AZ chủ yếu giúp giảm rủi ro từ sự cố hạ tầng tại một AZ. Nó không tự động giải quyết tất cả nguyên nhân gây downtime.

Ví dụ, nếu lập trình viên triển khai một bản cập nhật lỗi lên toàn bộ **EC2**, ứng dụng vẫn có thể ngừng hoạt động dù đang chạy trên ba AZ.

Tương tự, nếu một câu lệnh SQL vô tình xóa dữ liệu, database dự phòng có thể đồng bộ cả thay đổi sai đó.

Vì vậy, Multi-AZ không thay thế được backup, giám sát, kiểm thử, rollback và kế hoạch Disaster Recovery.

Một kiến trúc có tính sẵn sàng cao cần kết hợp nhiều cơ chế, chứ không chỉ phân tán máy chủ.

### Kết luận

Availability Zone là một trong những khái niệm nền tảng quan trọng nhất khi tìm hiểu kiến trúc AWS.

Thay vì đặt toàn bộ hạ tầng vào một trung tâm dữ liệu, AWS cung cấp nhiều AZ độc lập trong cùng Region, cho phép chúng ta xây dựng hệ thống có khả năng tiếp tục hoạt động khi một AZ gặp sự cố.

Tuy nhiên, AZ là hạ tầng mà AWS cung cấp, còn High Availability là thứ chúng ta phải chủ động thiết kế.

Với những dự án nhỏ, Single-AZ không phải lựa chọn sai. Với hệ thống cần độ tin cậy cao, Multi-AZ là một trong những bước đầu tiên để tiến tới kiến trúc production thực sự.

### Nguồn tham khảo

- [AWS Global Infrastructure – Regions and Availability Zones](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/)
- [AWS Documentation – Regions and Zones](https://docs.aws.amazon.com/global-infrastructure/latest/regions/aws-regions-availability-zones.html)
- [Amazon RDS – Multi-AZ DB Instance Deployments](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZSingleStandby.html)
- [AWS Well-Architected Framework – Reliability Pillar](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/welcome.html)
