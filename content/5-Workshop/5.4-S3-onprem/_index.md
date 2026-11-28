---
title: "Set up the data pipeline"
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
aliases:
  - /5-workshop/5.4-s3-onprem/5.4.1-create-media-storage/
  - /5-workshop/5.4-s3-onprem/5.4.2-connect-apify/
  - /5-workshop/5.4-s3-onprem/5.4.3-configure-ai/
---

1. **S3:** Create a media bucket in Singapore with **Block Public Access** enabled. Grant the EC2 role access to the media prefix. The Media Worker uploads images and the backend issues presigned URLs.
2. **Apify:** Choose an Actor, source group, and post limit. Configure `APIFY_TOKEN` and `APIFY_WEBHOOK_TOKEN` on the backend. Send `ACTOR.RUN.SUCCEEDED` over HTTPS to `/api/webhooks/apify` with the `X-Webhook-Token` header. Match the run ID to the imported RDS record.
3. **AI Worker:** Configure the OpenAI API, system prompt, and processing limits. Use the runner under `deploy/macket-ai-pipeline*` for container deployment. Stage 1 classifies posts. Eligible individual sales proceed to Stage 2 for product extraction before media processing.

<!-- IMAGE 05: Apify sample and matching RDS record. -->
<!-- IMAGE 06: AI result and S3 image for the same sample. -->
