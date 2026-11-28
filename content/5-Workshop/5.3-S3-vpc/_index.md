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

![EC2 launch settings showing Ubuntu Server 24.04 LTS x86-64 and the t3.micro instance type](/images/5-Workshop/5.3-S3-vpc/macket-ec2-launch.png)

*Example settings before launch: Ubuntu Server 24.04 LTS x86-64, `t3.micro`, and 8 GiB of storage.*

> **Production note.** If you expect to serve many real users, choose an **EC2** instance type with more RAM. Macket's **Amazon CloudWatch** metrics showed that `t3.micro` often had very little memory available and could easily reach its memory limit.

### Step 2. Create RDS PostgreSQL
Macket uses PostgreSQL to store structured data such as listings and accounts in tables while storing and querying flexible data such as Apify image lists and AI extraction results in `jsonb`. The backend uses `psycopg` and PostgreSQL `jsonb` queries, so switching engines would require changes to the ingestion and query code. [PostgreSQL documentation for `jsonb`](https://www.postgresql.org/docs/current/datatype-json.html).

![Amazon RDS Create database screen with PostgreSQL selected](/images/5-Workshop/5.3-S3-vpc/macket-rds-postgresql-create.png)

*The screenshot shows the PostgreSQL engine selection in **Amazon RDS**. For the workshop, choose **Full configuration** to set the networking options below.*

Create a DB subnet group from the private subnets. In **RDS → Create database**, select PostgreSQL and **Full configuration**, then choose Single-AZ and set **Public access: No**. Select the DB subnet group and database security group. Connect from **EC2** with `sslmode=require`, run `SELECT 1;`, and prepare the schema for the selected source version.

<!-- IMAGE 03: Private RDS or SELECT 1 from EC2. -->

### Step 3. Deploy the application
Install `backend/requirements.txt`, set the database variables in a private environment file, and run Uvicorn `main:app` on `127.0.0.1:8000`. Build the frontend with `npm ci` and `npm run build`. Serve `dist/` through Nginx and proxy `/api/` to Uvicorn. Configure DNS and HTTPS, then check `/api/health`.

<!-- IMAGE 04: Site over HTTPS and health check. -->
