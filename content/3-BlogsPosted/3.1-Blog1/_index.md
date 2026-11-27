---
title: "Blog 1"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## If your product reaches millions of users, can your server keep up?

[Read the original post on Facebook](https://www.facebook.com/share/p/1JYhk33Mvh/)

![Load testing illustration showing concurrent users and server metrics](/images/3-BlogsPosted/blog-1-load-testing.png)

*Illustration generated with ChatGPT.*

An **Amazon EC2** instance that runs smoothly with 10 users may struggle when 1,000 people access it at once.

How do you find your server's limits before taking an application into production? Start with **load testing**.

### 1. What is load testing?

Load testing simulates concurrent users sending requests to a server so you can measure how it performs as traffic increases. Testing in advance helps you identify capacity limits before real traffic reaches them.

Track three metrics:

- **Requests per second (RPS)** measures how many requests the server processes each second.
- **Latency** measures the time it takes to respond. P95 is the response time at or below which 95% of requests finish.
- **Error rate** measures the share of requests that fail, for example because of an HTTP 5xx response or a timeout.

Also monitor CPU, memory, network traffic, and database connections.

### 2. How do you run a load test?

Common tools include k6, Locust, and Apache JMeter. With k6, for example, you can start with 100 virtual users sending requests to your API, then increase to 200, 500, and 1,000 within the test limits. At each stage, check whether:

- Response times spike.
- CPU or memory reaches its limit.
- The database runs out of connections.
- The share of failed requests starts to climb.

On AWS, **Amazon CloudWatch** can help you track CPU, network traffic, and other infrastructure metrics during the test.

### 3. Does handling 1,000 requests per second mean you can serve 1,000 users?

One user might generate several requests per second or only a few per minute. An API that reads simple records can also handle far more requests than one that runs complex database queries.

Model real usage patterns and read RPS alongside latency and error rate to understand how much traffic your application can support.

### 4. When should you upgrade the server?

CPU reaching 100% is a reason to investigate. The bottleneck could be an SQL query, a missing database index, a connection pool, or application configuration. Load testing helps you identify the limiting component and decide whether to optimize code, add caching, upgrade **Amazon EC2**, or use **Amazon EC2 Auto Scaling**.

### Conclusion

Load testing answers three questions. How much traffic can the server handle? When does performance begin to decline? What should you optimize first?

**Test whether the system can support 1,000 concurrent users before 1,000 real users arrive.**

*Load-test only systems you own or have permission to test. Set a load limit and a way to stop the test.*

### References

- [Grafana k6 – Load test types](https://grafana.com/docs/k6/latest/testing-guides/test-types/)
- [AWS – Performance Efficiency Pillar](https://docs.aws.amazon.com/wellarchitected/latest/framework/performance-efficiency.html)
- [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)
