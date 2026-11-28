---
title: "Set up EC2, RDS, and the application"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
aliases:
  - /5-workshop/5.3-s3-vpc/5.3.1-launch-ec2/
  - /5-workshop/5.3-s3-vpc/5.3.2-create-rds/
  - /5-workshop/5.3-s3-vpc/5.3.3-deploy-web/
---

### Step 1. Launch EC2
In **EC2 → Launch instances**, choose Ubuntu Server 24.04 LTS x86-64 in Singapore. Attach the public subnet, application security group, and instance IAM role. Wait for the status checks, then connect and install Git, Python, a PostgreSQL client, and Nginx.

<!-- IMAGE 02: Running EC2 instance in Singapore. -->

### Step 2. Create RDS PostgreSQL
Create a DB subnet group from the private subnets. In **RDS → Create database**, choose PostgreSQL, Single-AZ, and **Public access: No**. Attach the DB security group. Connect from EC2 with `sslmode=require`, run `SELECT 1;`, and prepare the schema for the selected source version.

<!-- IMAGE 03: Private RDS or SELECT 1 from EC2. -->

### Step 3. Deploy the application
Install `backend/requirements.txt`, set the database variables in a private environment file, and run Uvicorn `main:app` on `127.0.0.1:8000`. Build the frontend with `npm ci` and `npm run build`. Serve `dist/` through Nginx and proxy `/api/` to Uvicorn. Configure DNS and HTTPS, then check `/api/health`.

<!-- IMAGE 04: Site over HTTPS and health check. -->
