---
draft: true
title: "Launch EC2 in Singapore"
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

### Objective

Provision an Ubuntu host for the Macket backend and storefront.

### Steps

1. In **Amazon EC2** in Singapore, select **Launch instances**. Choose Ubuntu Server 24.04 LTS, x86-64, and a size suitable for the lab. A micro instance can serve as the initial infrastructure check.
2. Select the VPC and public subnet and attach the application security group. Choose a key pair if using SSH. Attach an IAM instance role so you can grant media-bucket permissions after creating the bucket.
3. Launch the instance and wait for status checks to pass. Connect from the permitted administrative IP over SSH or use Session Manager if configured.
4. Install Git, Python venv, the PostgreSQL client, and Nginx. Prepare the frontend build environment or build the assets on your development machine.

[EC2 launch and connection guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)

<!-- IMAGE 02: 02-ec2.png — EC2 in Singapore, showing Running status and the selected subnet. -->

### Expected result

The Ubuntu host is reachable for application deployment.
