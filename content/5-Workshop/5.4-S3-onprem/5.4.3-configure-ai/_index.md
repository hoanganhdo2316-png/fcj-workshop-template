---
title: "Configure the AI Worker"
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

### Objective

Configure seller classification and product extraction.

### Steps

1. Configure the OpenAI API key through the application's private configuration and prepare the classification system prompt. Use a sample with an expected label.
2. Configure `backend/ai_pipeline.py` and the Media Worker in the backend environment. For the repo's container-based deployment, review `deploy/macket-ai-pipeline*` to configure the runner and systemd timer on EC2. Set request and budget limits before enabling batch processing.
3. Use Stage 1 to determine `post_type` and `seller_type`. Send eligible individual sales to Stage 2 to extract the model, specifications, price, and condition. Match the results and images to the original record before checking the storefront.

<!-- IMAGE 07: 07-ai.png — Stage 1 and Stage 2 results for the same sample, or its observed processing status. -->

### Expected result

The sample has a classification result and, if eligible, extracted product data.
