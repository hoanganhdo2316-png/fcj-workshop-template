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

<!-- IMAGE 01: VPC, subnets, and the public route table. -->
