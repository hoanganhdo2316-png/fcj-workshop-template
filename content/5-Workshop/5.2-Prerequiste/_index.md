---
title: "Prerequisites"
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
aliases:
  - /5-workshop/5.2-prerequiste/5.2.1-prepare-environment/
  - /5-workshop/5.2-prerequiste/5.2.2-prepare-source-data/
---

### Prepare the environment
1. Choose **Asia Pacific (Singapore)** (`ap-southeast-1`). Create a VPC, a public EC2 subnet, and at least two private subnets in different Availability Zones for the RDS subnet group.
2. Attach an internet gateway and add a default route to the public subnet. Use separate security groups. Allow RDS TCP 5432 only from the EC2 group and restrict SSH, if used, to the administrative IP.
3. Prepare the [Macket source](https://github.com/hoanganhdo2316-png/macket-aws), a domain, Apify and OpenAI API accounts, and an authorized sample post.

### Macket's deployed VPC configuration

Macket uses the default VPC in **Asia Pacific (Singapore)** with the IPv4 range `172.31.0.0/16`. The resource map shows **three public and three private subnets** across `ap-southeast-1a`, `ap-southeast-1b`, and `ap-southeast-1c`. The private subnets for RDS use the `RDS-Pvt-rt` route table. The public subnets share a route table with routes to the internet gateway and the `macket-s3-gateway` S3 gateway endpoint.

![Macket VPC resource map showing subnets, route tables, the internet gateway, and the S3 gateway endpoint](/images/5-Workshop/5.2-Prerequisite/macket-vpc-resource-map.png)

### Prepare the Apify Actor

Choose [Facebook Groups Scraper by Monkey D. Scraping](https://apify.com/monkey.d.scraping/facebook-groups-scraper), Actor identifier `monkey.d.scraping/facebook-groups-scraper`, to collect posts from public MacBook groups you are authorized to use.

In the **Input** tab, enter one sample group URL and set a low post limit for the first run. When captured, the Apify Console showed a reference price **from USD 0.69 per 1,000 group posts**. Check the price that applies to your account before starting the run.

![Monkey D. Scraping’s Facebook Groups Scraper and its displayed price in the Apify Console](/images/5-Workshop/5.2-Prerequisite/macket-apify-actor.png)
