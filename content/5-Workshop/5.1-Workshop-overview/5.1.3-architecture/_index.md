---
title: "Solution architecture"
weight: 3
chapter: false
pre: " <b> 5.1.3. </b> "
---

### Listing lifecycle in the target architecture

The diagram describes the overall design. The lab starts with the network, EC2 host, and RDS database in Singapore, then connects the pipeline components one by one.

1. **Start collection.** **Amazon EventBridge Scheduler** invokes an **AWS Lambda** function that calls the Apify API.
2. **Collect posts.** Apify collects Facebook posts from the configured URLs of public MacBook buy-and-sell groups.
3. **Receive and store results.** Apify signals completion through a webhook. The backend imports posts into **Amazon RDS for PostgreSQL**. The Media Worker stores images intended for display in **Amazon S3**.
4. **Classify and extract.** The AI Worker calls the OpenAI API with the system prompt and post data. Classification JSON identifies post and seller types. Eligible individual sales proceed to product extraction.
5. **Skip known resellers.** The `Skip Author` list in the database allows later runs to exclude known reseller accounts before calling the AI model.
6. **Display eligible listings.** FastAPI retrieves products eligible to appear in the Macket storefront.

![Macket AWS solution architecture diagram](/images/2-Proposal/macket-aws-architecture.png)

See the [Macket Proposal](../../../2-proposal/) and [AWS environment setup](../../5.2-prerequiste/).
