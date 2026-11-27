---
title: "Week 11 Worklog"
date: 2026-11-05
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

**Period:** 05/11/2026 – 09/11/2026

### Week 11 objectives

- Develop an AI-based classification workflow for collected Facebook posts.
- Distinguish individual sellers from commercial sellers and identify relevant electronics listings.
- Develop backend APIs for accessing and managing collected data.
- Develop the Macket web interface for browsing second-hand electronics listings.
- Integrate data collection, AI processing, database storage, and application components.

### Activities during the week

| No. | Activities | Start Date | Completion Date | Reference Material |
| ---: | --- | --- | --- | --- |
| 1 | Design the AI processing pipeline and classification criteria. Review collected Facebook posts to identify relevant listings, commercial advertisements, and individual seller characteristics. | 05/11/2026 | 05/11/2026 | — |
| 2 | Develop the AI Worker for processing collected posts. Establish classification stages and processing statuses. Review the vector embedding integration and its potential application to product matching. | 06/11/2026 | 06/11/2026 | [PostgreSQL](https://www.postgresql.org/docs/) |
| 3 | Develop FastAPI endpoints for retrieving collected posts and supporting product-based browsing. Integrate PostgreSQL queries with the application and review the **S3** media storage workflow. | 07/11/2026 | 07/11/2026 | [FastAPI Documentation](https://fastapi.tiangolo.com/) |
| 4 | Develop the Macket frontend with a marketplace-style interface. Organize electronics listings by product categories and MacBook models. Integrate frontend components with the backend APIs. | 08/11/2026 | 08/11/2026 | — |
| 5 | Perform integration testing across Apify, the ingestion module, **RDS** PostgreSQL, the AI Worker, and the web application. Review processing logs and identify a workflow issue where newly collected posts remain pending because the AI Worker is not automatically triggered after ingestion. | 09/11/2026 | 09/11/2026 | [**AWS Lambda**](https://docs.aws.amazon.com/lambda/) |

### Weekly results

- Established the AI-based post classification workflow.
- Developed classification criteria for identifying relevant electronics listings and commercial sellers.
- Integrated the backend application with collected Facebook data stored in PostgreSQL.
- Developed the marketplace-style frontend and product-based browsing interface.
- Established the main integration flow between data collection, database storage, AI processing, and the web application.
- Successfully tested the manual data collection and ingestion workflow.
- Identified an automation issue between the ingestion module and the AI Worker that requires further improvement.
