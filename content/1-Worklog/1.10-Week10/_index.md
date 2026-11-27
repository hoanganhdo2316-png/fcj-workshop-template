---
title: "Week 10 Worklog"
date: 2026-10-31
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

**Period:** 31/10/2026 – 04/11/2026

### Week 10 objectives

- Research and evaluate available solutions for collecting Facebook group posts.
- Develop an automated data collection module for second-hand electronics listings.
- Optimize data collection costs and reduce unnecessary collection of commercial seller posts.
- Establish a structured data ingestion workflow between the external scraping service and PostgreSQL.
- Prepare the collector for containerized deployment and scheduled execution.

### Activities during the week

| No. | Activities | Start Date | Completion Date | Reference Material |
| ---: | --- | --- | --- | --- |
| 1 | Research open-source Facebook crawling libraries and test an initial crawler implementation. Encounter dependency and JSON parsing errors during installation and evaluate alternative solutions. | 31/10/2026 | 31/10/2026 | [GitHub](https://github.com/) |
| 2 | Evaluate Apify Facebook Groups Scraper Actors. Test the initial Actor and identify limitations related to scraping costs and the high proportion of commercial seller posts. | 01/11/2026 | 01/11/2026 | [Apify Store](https://apify.com/store) |
| 3 | Select a more cost-effective Apify Actor. Identify and organize high-activity Facebook groups. Configure collection parameters, sorting, post freshness, and seller exclusion rules. | 02/11/2026 | 02/11/2026 | [Apify Documentation](https://docs.apify.com/) |
| 4 | Develop the Python collector module to initiate scraping runs, retrieve datasets, normalize collected posts, and handle author information, image URLs, and post metadata. Implement duplicate detection and database ingestion logic. | 03/11/2026 | 03/11/2026 | [Apify API](https://docs.apify.com/api/v2) |
| 5 | Configure the Docker development environment and prepare the collector for containerized deployment. Develop the **AWS Lambda**-based execution workflow with a target collection interval of ten minutes. Review execution logs and error-handling mechanisms. | 04/11/2026 | 04/11/2026 | [Docker](https://docs.docker.com/), [**AWS Lambda**](https://docs.aws.amazon.com/lambda/) |

### Weekly results

- Evaluated multiple Facebook data collection solutions and identified the limitations of the initial open-source approach.
- Selected a more reliable and cost-effective external scraping solution.
- Reduced the estimated scraping cost from approximately $5 to $0.20 per 1,000 posts by switching Apify Actors.
- Established a list of active Facebook groups relevant to second-hand electronics.
- Developed the collector module and structured data ingestion workflow.
- Implemented post deduplication and commercial seller exclusion logic.
- Prepared the data collection module for containerized and scheduled execution.
