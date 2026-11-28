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
2. **Apify:** Choose an Actor, source group, and post limit. Configure `APIFY_TOKEN` and `APIFY_WEBHOOK_TOKEN` on the backend. Under **Integrations**, add an **HTTP request** for the **Run succeeded** event (`ACTOR.RUN.SUCCEEDED`). Send it over HTTPS to `/api/webhooks/apify` with the `X-Webhook-Token` header.

   When a collection run succeeds, Apify automatically sends the webhook. The backend uses the run ID in the notification to retrieve the results and import them into RDS. This removes the need to poll Apify for run status.

   ![Apify Integrations showing an enabled HTTP webhook for the Run succeeded event](/images/5-Workshop/5.4-S3-onprem/macket-apify-webhook.png)

3. **AI Worker:** Configure the OpenAI API, system prompt, and processing limits. Use the runner under `deploy/macket-ai-pipeline*` for container deployment. Stage 1 classifies posts. Eligible individual sales proceed to Stage 2 for product extraction before media processing.

   ![The two stages of the AI Worker: post classification and information extraction](/images/5-Workshop/5.4-S3-onprem/macket-ai-worker-stages.png)

<!-- IMAGE 05: Apify sample and matching RDS record. -->
<!-- IMAGE 06: AI result and S3 image for the same sample. -->
