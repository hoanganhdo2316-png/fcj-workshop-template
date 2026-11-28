---
title: "Configure Lambda, Scheduler, and operations"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

### Objective

Set up the collection trigger and add operational visibility once a manual run works.

### Steps

1. Build the collector trigger with `apify/Dockerfile.lambda`, push the image to **Amazon ECR** in Singapore, and create an **AWS Lambda** function from that image. Use an execution role scoped to the function's permissions and logging.
2. Configure the function's collector configuration URL and token through `COLLECTOR_CONFIG_URL` and `COLLECTOR_CONFIG_TOKEN`. Check one invocation in the test environment and match its result with the Apify run.
3. In **Amazon EventBridge Scheduler**, create a schedule targeting the function. Give the schedule permission to invoke the function and keep the schedule disabled while validating the manual flow and cost limits. The supplied guide records a disabled schedule at the time of its check.
4. Review **Amazon CloudWatch** for EC2, RDS, and Lambda metrics and logs. To reproduce the CDN layer, configure the Nginx HTTPS origin in **Amazon CloudFront**, associate **AWS WAF** in Count mode for initial observation, and update DNS through your domain provider. Verify that dynamic API requests and authentication behave correctly.

[Lambda container images](https://docs.aws.amazon.com/lambda/latest/dg/images-create.html) · [EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-schedule.html)

<!-- IMAGE 08: 08-lambda-scheduler.png — Lambda image source or the Scheduler target and its actual state. -->
<!-- IMAGE 09 (optional): 09-monitoring.png — CloudWatch metrics or logs for the test run. -->

### Expected result

One Lambda invocation can be matched to an Apify run. The schedule and monitoring can be checked independently.
