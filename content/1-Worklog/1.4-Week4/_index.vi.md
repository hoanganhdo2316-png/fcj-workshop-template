---
title: "Worklog Tuần 4"
date: 2025-10-13
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

**Thời gian:** 13/10/2025 – 19/10/2025

### Mục tiêu tuần 4

- Hiểu serverless compute và kiến trúc hướng sự kiện.
- Tìm hiểu cách event kích hoạt hoặc điều phối công việc giữa các service.

### Nội dung học tập trong tuần

| STT | Nội dung thực hiện | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---: | --- | --- | --- | --- |
| 1 | Tìm hiểu **Lambda** function, handler, runtime limit, execution role và các kiểu invocation. | 13/10/2025 | 14/10/2025 | [**AWS Lambda**](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) |
| 2 | Tìm hiểu **EventBridge** event bus, rule, event pattern, schedule và target. | 15/10/2025 | 16/10/2025 | [**Amazon EventBridge**](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html) |
| 3 | So sánh request đồng bộ với luồng xử lý bất đồng bộ theo event. | 17/10/2025 | 19/10/2025 | [**Lambda** invocation](https://docs.aws.amazon.com/lambda/latest/dg/lambda-invocation.html), [**Amazon EventBridge**](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html) |

### Kết quả đạt được trong tuần

- Hiểu khi nào **Lambda** phù hợp cho xử lý ngắn hạn, hướng sự kiện.
- Hiểu **EventBridge** giúp tách rời nơi phát sinh event và nơi xử lý event.
