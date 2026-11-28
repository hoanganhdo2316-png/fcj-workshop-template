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
