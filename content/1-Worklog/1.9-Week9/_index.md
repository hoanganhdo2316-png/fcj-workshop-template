---
title: "Week 9 Worklog"
date: 2026-10-25
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

**Period:** 25/10/2026 – 30/10/2026

### Week 9 objectives

- Analyze the requirements and define the scope of the Macket project, an automated platform for collecting and filtering second-hand electronics listings from Facebook.
- Design a cloud architecture using AWS services with consideration for scalability and infrastructure costs.
- Deploy and configure the foundational AWS infrastructure, including **Amazon VPC**, **EC2**, **RDS** PostgreSQL, and **S3**.
- Establish the backend development environment and verify connectivity between cloud resources.
- Configure domain access and secure communication using HTTPS.

### Activities during the week

| No. | Activities | Start Date | Completion Date | Reference Material |
| ---: | --- | --- | --- | --- |
| 1 | Analyze the project requirements, identify the main system components, and define the data collection and processing workflow. | 25/10/2026 | 25/10/2026 | — |
| 2 | Design the AWS cloud architecture. Select appropriate AWS services, including **EC2**, **RDS**, **S3**, and **Lambda**. Review the proposed infrastructure and estimate operating costs. | 26/10/2026 | 26/10/2026 | [AWS Pricing Calculator](https://calculator.aws/) |
| 3 | Configure the **Amazon VPC** environment, subnets, route tables, and Security Groups. Launch an **EC2** instance running Ubuntu and establish remote access through SSH. | 27/10/2026 | 27/10/2026 | [**Amazon VPC**](https://docs.aws.amazon.com/vpc/) |
| 4 | Deploy **Amazon RDS** PostgreSQL, configure database access permissions, and establish connectivity between **EC2** and **RDS** using an encrypted connection. | 28/10/2026 | 28/10/2026 | [**Amazon RDS**](https://docs.aws.amazon.com/rds/) |
| 5 | Initialize the FastAPI backend, configure SQLAlchemy and PostgreSQL drivers, and implement health-check endpoints. Investigate and resolve database integration and embedding test errors. | 29/10/2026 | 29/10/2026 | [FastAPI Documentation](https://fastapi.tiangolo.com/) |
| 6 | Configure **Amazon S3** for media storage. Set up Nginx as a reverse proxy and run the backend as a systemd service. | 30/10/2026 | 30/10/2026 | [**Amazon S3**](https://docs.aws.amazon.com/s3/) |
| 7 | Point the `macket.io.vn` domain to the server and enable HTTPS using Certbot. | 30/10/2026 | 30/10/2026 | [Certbot](https://certbot.eff.org/) |

### Weekly results

- Defined the project requirements and designed the initial AWS cloud architecture for Macket.
- Established the foundational networking, computing, database, and storage infrastructure.
- Successfully configured **EC2** and established connectivity with **RDS** PostgreSQL.
- Initialized the FastAPI backend and verified the application and database health-check endpoints.
- Resolved initial database integration errors and successfully tested vector embedding storage.
- Configured the project domain, Nginx reverse proxy, and HTTPS access.
- The basic Macket interface was accessible at `macket.io.vn`.
- Established a functional cloud environment for further application development.
