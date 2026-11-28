---
draft: true
title: "Connect Apify to the backend"
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

### Objective

Configure the ingestion path from Apify to PostgreSQL.

### Steps

1. In Apify, select the Facebook Groups collection Actor. Prepare a permitted public group URL and set a small post limit for the lab.
2. Set `APIFY_TOKEN` so the backend can retrieve Apify results, and set `APIFY_WEBHOOK_TOKEN` to authenticate the callback. Configure Apify to send `ACTOR.RUN.SUCCEEDED` over HTTPS to `/api/webhooks/apify`, with the matching `X-Webhook-Token` header.
3. Run a bounded collection. Verify that the webhook sends a run ID in `resource.id`. The backend uses that ID to import results into PostgreSQL. Match the sample post and check whether a repeat import creates a duplicate.

<!-- IMAGE 06: 06-apify.png — A completed Apify run and its matching RDS record with credentials and personal data masked. -->

### Expected result

An Apify run produces a traceable record in RDS.
