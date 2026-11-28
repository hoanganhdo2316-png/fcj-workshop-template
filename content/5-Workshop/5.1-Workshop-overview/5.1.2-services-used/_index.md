---
title: "Services used"
weight: 2
chapter: false
pre: " <b> 5.1.2. </b> "
---

## AWS services

| Service | Role in Macket |
| --- | --- |
| **Amazon EC2** | Hosts the FastAPI backend and Nginx. |
| **AWS Lambda** | Calls the Apify API. The target architecture describes the `Skip Author` update flow. |
| **Amazon RDS for PostgreSQL** | Stores listings, classification results, and the `Skip Author` list. |
| **Amazon S3** | Stores listing images. |
| **Amazon Route 53** | Manages DNS for the Macket domain in the target architecture. |
| **Amazon CloudFront** | Serves the application through the Nginx origin on **Amazon EC2**, as recorded in the deployment guide. |
| **AWS WAF** | Associated with **CloudFront** in monitoring mode at the time of the documented check. |
| **Amazon VPC** | Separates the public application subnet from the private database subnet. |
| **Amazon Bedrock** | A planned option for seller classification once the project has access to suitable models. |
| **AWS Secrets Manager** | A planned secrets management integration to verify before including it in the demo. |
| **Amazon ECR** | Stores the container image for the **AWS Lambda** collection trigger. |
| **Amazon EventBridge** | A schedule was created to invoke **AWS Lambda**. It was `DISABLED` at the documented check. |
| **AWS CloudTrail** | The deployment guide records AWS management events in Event History. |
| **AWS IAM** | Manages permissions for the application and services to access AWS resources. |
| **Amazon CloudWatch** | Monitors **EC2** and **RDS** metrics, including EC2 memory and disk usage through the CloudWatch Agent. |

## External services

| Service | Role in Macket |
| --- | --- |
| OpenAI API | Classifies sellers using the system prompt and returns a JSON label for each post. |
| Apify | Collects posts from configured public Facebook groups and sends a webhook notification when results are ready. |
| Zalo OAuth | The planned sign-in method for Macket users. |
