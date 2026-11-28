---
title: "Clean up lab resources"
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

### Objective

Remove temporary resources created solely for the lab.

### Steps

1. Disable the lab schedule before removing its Lambda function or images. Save any logs and screenshots needed for the report.
2. Identify the lab's EC2 instance, RDS database, S3 objects, ECR images, and optional CDN or monitoring resources. Review backups and retention before removing any of them.
3. Remove only the lab resources. Leave the project environment and shared resources in service. Verify that the temporary schedule and chargeable lab resources are gone.
