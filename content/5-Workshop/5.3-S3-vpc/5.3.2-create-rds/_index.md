---
title: "Create RDS PostgreSQL and connect from EC2"
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

### Objective

Provision a private PostgreSQL database for Macket.

### Steps

1. In **Amazon RDS**, create a DB subnet group from the private subnets.
2. Select **Create database**, choose PostgreSQL and **Single-AZ** for the lab, and select the Macket VPC and DB subnet group. Set **Public access** to **No** and select the DB security group.
3. Create the application database and configure backups for the environment. From EC2, connect to the RDS endpoint using a PostgreSQL client with `sslmode=require`. Run `SELECT 1;` to check connectivity.
4. Prepare a schema compatible with the selected source version. For a lab, use `tests/staging_schema.sql` as a test baseline and review which changes in `backend/migrations/` are already present before applying missing migrations in order.

[RDS DB instance creation guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html)

<!-- IMAGE 03: 03-rds.png — Available PostgreSQL instance with Public access set to No, or a successful SELECT 1 from EC2. -->

### Expected result

EC2 can query PostgreSQL over TLS within the VPC.
