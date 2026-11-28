---
title: "Deployment checks and cost estimate"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

### Deployment checks

Open the site over HTTPS and check `/api/health`. Start one bounded collection run and trace the sample through RDS, the AI results, the S3 image, and the storefront.

<!-- IMAGE 08: Sample listing on the storefront or its observed status. -->

### Cost estimate

The Proposal estimates **USD 46.65–82.65 per month** for the reference configuration. Track Apify and OpenAI API costs separately when running the lab. [See the Proposal estimate](../../2-proposal/#6-budget-estimate)

The AWS Cost Explorer table for **1–30 November 2026** showed **USD 25.80** when captured on **28 November 2026**. The largest line items were **RDS** (USD 13.68) and **EC2 Instances** (USD 5.69). The figures reflect the report's applied filters.

![AWS cost table and November 2026 date range](/images/2-Proposal/macket-cost-explorer-nov-2026.png)
