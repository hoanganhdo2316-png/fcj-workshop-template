---
draft: true
title: "Prepare the AWS environment"
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

### Objective

Create a network with a reachable application subnet and private database subnets.

### Steps

1. In the AWS Console, choose **Asia Pacific (Singapore)**, `ap-southeast-1`, for EC2, RDS, S3, ECR, Lambda, and Scheduler.
2. In **Amazon VPC**, create a VPC with DNS resolution and DNS hostnames enabled. Create one public subnet for the application and at least two private subnets in different Availability Zones for the DB subnet group.
3. Attach an internet gateway and add a default route to it in the public subnet’s route table. Keep the database subnets private.
4. Create separate security groups. Allow HTTPS and, if using SSH, restrict it to your administrative IP. Allow TCP `5432` on the DB security group only from the application security group.

<!-- IMAGE 01: 01-vpc.png — VPC resource map or subnet list with the public route table. -->

### Expected result

The VPC and subnets are ready for EC2 and RDS.
