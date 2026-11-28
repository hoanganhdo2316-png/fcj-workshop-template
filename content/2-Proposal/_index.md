---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## Macket

An AI-powered platform that helps electronics stores and resellers source used MacBooks on Facebook.

[Macket source code on GitHub](https://github.com/hoanganhdo2316-png/macket-aws)

## 1. Executive summary

Stores and resellers looking to buy used electronics directly from individuals spend time sorting through listings from other commercial sellers. Macket focuses on used Apple MacBooks and gives buyers one place to find posts that are more likely to come from individual owners.

The platform collects posts from Facebook groups, detects duplicates, and uses an LLM to classify sellers. Buyers can filter the results and open the original post to contact the seller.

Macket is designed for the AWS Singapore Region. The project uses the OpenAI API for classification today and plans to evaluate **Amazon Bedrock** once it has access to suitable models.

## 2. Problem statement

**Current challenges**

- Listings are scattered across Facebook groups. Buyers have to check several groups manually, and well-priced devices can sell before they find the post.
- Posts use free text for the model, specifications, price, and condition. The same listing can also appear in more than one group.
- Commercial listings can look like posts from individual owners, making it difficult to identify suitable sources.

**Proposed solution**

- Collect posts on a schedule and detect duplicates before processing.
- Apply seller classification criteria through a system prompt. Match known reseller IDs first to avoid classifying the same accounts repeatedly.
- Show listings in a searchable, filterable interface with links to the original posts.

## 3. Solution architecture

The target architecture runs in AWS Asia Pacific (Singapore), `ap-southeast-1`. It includes resources already deployed and components planned for later phases.

![Macket AWS solution architecture diagram](/images/2-Proposal/macket-aws-architecture.png)

**Main components**

- **Amazon Route 53**, **Amazon CloudFront** and **AWS WAF** handle DNS, content delivery and caching, and HTTP request filtering in the target architecture.
- **Amazon VPC** separates the public application subnet from the private database subnet. An internet gateway provides internet access for the application server, and security groups restrict database access to the application layer.
- **Amazon EC2** hosts the frontend, FastAPI backend, Nginx reverse proxy, AI worker, and search and user-management logic. The initial design uses one instance to control costs.
- **Amazon RDS for PostgreSQL** stores listings, products, sources, seller IDs, user accounts, and classifications. **Amazon S3** stores images and other unstructured data. The **S3 gateway endpoint** lets eligible EC2 traffic reach S3 over the AWS network.
- **Amazon EventBridge Scheduler** and **AWS Lambda** are designed to schedule and orchestrate collection. Lambda requests an Apify run, and the collection service sends posts to the backend API.
- **AWS IAM** and **AWS Secrets Manager** manage permissions and secrets. **Amazon CloudWatch** and **AWS CloudTrail** cover operational monitoring and AWS API auditing in the target architecture. Zalo OAuth is the planned sign-in method.

After collection, the backend checks for duplicate posts and known reseller IDs. The AI worker sends posts that still need classification to the model and receives a JSON result. The system stores the result in **Amazon RDS**, records newly identified reseller IDs for reuse, and serves matching listings through the web interface.

## 4. Technical implementation

- **Application and data.** The deployed foundation includes **Amazon EC2**, FastAPI, Nginx, domain access over HTTPS, and **Amazon RDS for PostgreSQL**. Backend connectivity to the database and the route table for the **S3 gateway endpoint** were verified.
- **Collection and classification.** The project tested collection through Apify. The AI worker sends post captions and classification criteria to the OpenAI API. The system prompt uses a small set of real examples from individual sellers and resellers. Classification accuracy still needs to be tested on new posts.
- **Secrets.** **AWS Secrets Manager** holds the OpenAI and Apify API keys and the database password.
- **Deployment and operations.** The deployment design uses Docker, GitHub Actions, and **Amazon ECR** for image builds, versioning, and rollout to **Amazon EC2** or **AWS Lambda**. Full CI/CD, scheduled collection, automated end-to-end AI processing, centralized monitoring, and advanced security controls remain in development.

## 5. Roadmap and milestones

**Phase 1 · Infrastructure foundation**

- Set up **EC2**, **RDS**, **VPC**, and **S3**.
- Configure the domain, HTTPS, and backend API.
- Set up public and private database subnets and security groups.
- Integrate the **S3 gateway endpoint**.
- Test connectivity between components.

**Phase 2 · Automated data collection**

- Complete the post collection module.
- Integrate the external collection service.
- Implement duplicate detection.
- Integrate **EventBridge Scheduler** and **AWS Lambda**.
- Complete post and image storage.

**Phase 3 · AI processing and classification**

- Integrate the LLM through the OpenAI API and consider **Amazon Bedrock** when suitable model access is available.
- Define seller classification criteria in the system prompt.
- Complete the AI worker for automated post analysis.
- Test classification accuracy with real data.
- Refine the system prompt and optimize model costs.

**Phase 4 · Application development**

- Complete the user interface.
- Develop listing and seller filters.
- Integrate Zalo OAuth.
- Refine the search and results display.
- Test functionality across the system.

**Phase 5 · Security and operational optimization**

- Complete CI/CD with GitHub Actions and **Amazon ECR**.
- Integrate **CloudFront** and **AWS WAF**.
- Refine access management with **IAM** and **Secrets Manager**.
- Set up monitoring with **CloudWatch** and **CloudTrail**.
- Test load and assess scalability.
- Review and optimize AWS operating costs.

## 6. Budget estimate

The initial design limits operating costs while allowing for future growth. This preliminary estimate assumes the Singapore Region, On-Demand pricing, one **EC2** instance, and one Single-AZ **RDS** instance.

| Service | Estimated monthly cost (USD) |
| --- | ---: |
| **Amazon EC2** and **EBS** | 13–23 |
| **Amazon RDS** for PostgreSQL | 20–27 |
| **Amazon S3** | 1–3 |
| **Amazon ECR** | 0.5–2 |
| **Amazon CloudFront** | 0–3 |
| **AWS WAF** | 6–10 |
| **Amazon Route 53** | 0.5–1 |
| **AWS Lambda** and **EventBridge** | 0–1 |
| **AWS Secrets Manager** | 1–3 |
| **Amazon CloudWatch** | 1–4 |
| **AWS CloudTrail** | 0–2 |
| Public IPv4 | 3.65 |
| **S3 gateway endpoint** | 0 |
| Estimated total | 46.65–82.65 |

The estimate excludes **Amazon Bedrock**, the OpenAI API, the external collection service, taxes, and domain registration. Actual costs depend on resource configuration and usage.

### Costs recorded in AWS Cost Explorer

The screenshot taken on **28 November 2026** shows a total of **USD 25.66** for the selected report scope. **Amazon RDS** was the largest line item (USD 13.6153), followed by **Amazon EC2 Compute** (USD 5.6670). The screenshots do not show the selected date range, so these figures are presented separately from the monthly estimate above.

![AWS costs by service totaling USD 25.66](/images/2-Proposal/macket-cost-explorer-overview.png)

![Service line items shown in AWS Cost Explorer](/images/2-Proposal/macket-cost-explorer-by-service.png)

**Cost optimization**

- Use one **EC2** instance in the initial phase.
- Use Single-AZ **RDS** while the project has no high-availability requirement.
- Use **Lambda** for short orchestration tasks.
- Use an **S3 gateway endpoint**.
- Classify new posts only when needed and reuse known reseller IDs.
- Refine the system prompt to reduce unnecessary tokens.
- Choose a model appropriate for classification.
- Defer paid **NAT gateways** and **interface endpoints** until needed.
- Review running costs regularly.

## 7. Risk assessment

| Risk | Impact | Mitigation |
| --- | --- | --- |
| Facebook policies or data structures change | Collection interruptions | Isolate the collection module and use permitted data sources |
| Dependence on the external collection service | New listings fail to appear | Add retries and logging, and standardize the integration interface |
| LLM misclassifies sellers | Buyers miss suitable listings | Evaluate sample results and refine the system prompt |
| Duplicate data | Higher storage and AI processing costs | Detect duplicates before processing |
| API keys or credentials leak | Unauthorized access and charges | Use **IAM** roles, **Secrets Manager**, and least privilege |
| Unauthorized database access | Data loss or disclosure | Use a private subnet, security groups, encrypted connections, and backups |
| **EC2** or **RDS** failure | Service interruption | Schedule backups, monitor resources, and define recovery procedures |
| AI and collection costs increase | Budget overrun | Limit the number of posts and control usage |
| Privacy and content usage rights | Legal risk | Respect data access permissions, limit stored personal information, and link to original posts |

## 8. Expected outcomes

### 8.1. Users

Macket aims to help stores and resellers spend less time sourcing used MacBooks and manually monitoring Facebook groups, while improving access to listings from people selling their own devices.

### 8.2. System

- Collect and refresh listings on a schedule.
- Classify individual sellers and resellers with an LLM.
- Adjust classification criteria through the system prompt.
- Offer one interface for searching and filtering posts.
- Separate compute, database, and object storage resources.
- Automate application deployment.
- Monitor operations, control access, and optimize running costs.

After validating the system in the used MacBook market, the project could expand to other categories of electronics.

### 8.3. Professional development

Through Macket, I apply AWS knowledge to a practical problem, including network infrastructure, application deployment, database management, AI integration, and operations automation. The project helps me practice balancing business requirements, security, performance, scalability, and cost when designing a system.
