---
title: "Configure Lambda, Scheduler, and operations"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

1. Build `apify/Dockerfile.lambda`, push to **Amazon ECR**, and create an **AWS Lambda** function from the image. Set `COLLECTOR_CONFIG_URL` and `COLLECTOR_CONFIG_TOKEN`. Invoke it once and match the Apify run using **CloudWatch** logs.
2. Create an **EventBridge Scheduler** schedule targeting Lambda and grant invocation permission. Keep the schedule off while validating cost limits and the manual flow.
3. For the optional CDN layer, point **CloudFront** at Nginx over HTTPS and attach **AWS WAF** in Count mode for initial observation.

<!-- IMAGE 07: Lambda image source, Scheduler target, or run log. -->

## Logs, metrics, and alarms

### Amazon CloudWatch

Open **CloudWatch → Dashboards → Macket-Monitoring** to view **Amazon EC2** CPU, memory, and disk usage, plus **Amazon RDS** CPU, freeable memory, connections, free storage, and read/write latency. The dashboard also shows **AWS Lambda** invocations, errors, and duration. For **Amazon S3** bucket size and object count, go to **Metrics → All metrics → S3**. [Amazon S3 metrics in CloudWatch](https://docs.aws.amazon.com/AmazonS3/latest/userguide/metrics-dimensions.html).

![Macket-Monitoring dashboard in Amazon CloudWatch](/images/5-Workshop/5.5-Policy/macket-cloudwatch-dashboard.png)

**Technical logs.** Go to **Logs → Log groups** and open `/aws/lambda/<function-name>` to view **AWS Lambda** execution logs. Backend and AI Worker logs are available in **CloudWatch Logs** if log forwarding from **Amazon EC2** has been configured. [Lambda logging guide](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-cloudwatchlogs.html).

**Example alarm.** Go to **Alarms → Create alarm**, select `Errors` for the collector function, use `Sum` over five minutes, and set the threshold to `≥ 1` for one out of one datapoint. Choose `Treat missing data as good (not breaching)` so idle periods do not trigger the alarm. For email notifications, select an **Amazon SNS** topic for the `ALARM` state and confirm the email subscription. This alarm monitors function execution errors. Check the crawl run status separately in Apify. [CloudWatch alarms guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Alarms.html).

### Macket Admin logs

**Scan History** shows each crawl's timestamp, run ID, status, collection counts, classification and extraction progress, displayed post counts, AI costs, and any errors.

![Macket Admin Scan History showing the status and progress of individual crawls](/images/5-Workshop/5.5-Policy/macket-admin-scan-history.png)

**AI Classification Logs** records the classification result for each post processed by AI. Search by **Post ID** or filter by **Seller Type** to see the seller category assigned to a post.

![Macket Admin AI Classification Logs with Seller Type and Post ID filters](/images/5-Workshop/5.5-Policy/macket-admin-ai-classification-logs.png)
