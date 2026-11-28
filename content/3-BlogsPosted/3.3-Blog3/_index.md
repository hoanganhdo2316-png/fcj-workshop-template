---
title: "Blog 3"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

## Building a social media data pipeline on AWS

[Read the original post on Facebook](https://www.facebook.com/share/p/1A4VmUbcda/)

![AWS architecture diagram for Twitter and Reddit data ingestion and aggregation](/images/3-BlogsPosted/blog-3-social-media-pipeline.png)

[Original AWS architecture diagram (PDF)](https://d1.awsstatic.com/solutions/guidance/architecture-diagrams/social-media-data-pipeline-on-aws.pdf)

Posts and comments can help businesses understand market trends, customer feedback, and interest in products. Collecting that data across platforms requires handling different APIs, access limits, and formats. Without coordination, throttling and processing failures can become harder to manage as data volumes grow.

### 1. Architecture overview

The AWS reference architecture uses Twitter and Reddit as examples. It collects data from multiple sources, handles temporary collection failures, normalizes records, and stores them in **Amazon S3** for reporting and analytics. The design uses serverless components connected through **Amazon EventBridge**, **Amazon SQS**, and **AWS Step Functions**.

### 2. How the pipeline works

1. **Collect regional trends.** Scheduled **Amazon EventBridge** rules invoke **AWS Lambda** to retrieve Twitter trends and store them in **Amazon DynamoDB**. **AWS Systems Manager Parameter Store** supplies configuration inputs such as locations and credentials.
2. **Publish trend events.** Another scheduled **Lambda** function reads the most recent trends from **DynamoDB** and publishes them to an **EventBridge** event bus. Trend discovery and post collection can run independently.
3. **Queue collection requests.** **EventBridge** invokes **AWS Step Functions** to validate and track requests, then place them in **Amazon SQS**. The queue buffers requests so consumers can control the collection rate and retry transient failures.
4. **Retrieve posts.** **Lambda** consumes queued messages, calls the Search API, transforms the results, and publishes events back to **EventBridge**. **DynamoDB** stores pagination tokens so collection can resume where it stopped.
5. **Collect Reddit content.** A separate **Lambda** function pulls popular subreddits and related comments. **DynamoDB** tracks the search state.
6. **Combine the sources.** Twitter and Reddit records reach **EventBridge** in a common format so downstream processing can work with either source.
7. **Validate and transform records.** **Step Functions** runs the validation, transformation, and loading workflow. Its execution history makes it easier to trace failures across steps.
8. **Detect language.** **Amazon Comprehend** identifies the language when the source event has no language metadata.
9. **Store the corpus.** **Amazon S3** holds normalized records for reporting, analytics, or machine learning.
10. **Send metadata to analytics when needed.** The design can stream metadata through **Amazon Kinesis** to **Amazon Redshift** for deeper analysis.

### 3. Roles of the AWS services

| Service | Role |
| --- | --- |
| **Amazon EventBridge** | Schedules work and routes events |
| **AWS Lambda** | Collects and transforms data |
| **Amazon DynamoDB** | Stores trends, pagination tokens, and search state |
| **AWS Systems Manager Parameter Store** | Holds configuration inputs |
| **AWS Step Functions** | Coordinates workflows with multiple steps |
| **Amazon SQS** | Buffers requests and supports retry |
| **Amazon Comprehend** | Detects the language of content |
| **Amazon S3** | Stores the normalized corpus |
| **Amazon Kinesis** | Streams metadata to downstream systems |
| **Amazon Redshift** | Supports queries and analytics in a data warehouse |

### 4. System design considerations

**Control collection under API throttling.** **Amazon SQS** separates incoming requests from execution. Consumers can limit how fast they call the API and retry transient failures.

**Reduce coupling through events.** With **Amazon EventBridge**, each source can publish the same event format. A new collection workflow can feed the downstream pipeline through that shared format.

**Separate workflow state from the corpus.** **Amazon DynamoDB** supports quick lookups of collection state and metadata. **Amazon S3** holds the larger body of collected data.

### 5. System design lessons

The pipeline separates ingestion, event routing, processing, storage, and analytics. When adding a data source, define how its records enter the shared format and how the system handles rate limits, state, and failures.

The Twitter flow reflects the API and access terms in effect when AWS designed the reference architecture. Before using any source, check its current API, permissions, and rate limits. Other platforms can use the same design where their APIs and access permissions allow it.

### References

- [Guidance for Social Media Data Pipeline on AWS](https://docs.aws.amazon.com/solutions/social-media-data-pipeline-on-aws/)
- [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/)
- [Amazon SQS](https://docs.aws.amazon.com/sqs/)
- [AWS Step Functions](https://docs.aws.amazon.com/step-functions/)
- [Amazon DynamoDB](https://docs.aws.amazon.com/dynamodb/)
- [Amazon S3](https://docs.aws.amazon.com/s3/)
