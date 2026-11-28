---
title: "Verify the deployment"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

### Objective

Check whether the environment built in the preceding steps processes and displays one sample listing.

### Steps

1. Confirm the site opens over HTTPS and the backend can query RDS.
2. Start one bounded collection run. Match its run ID to the stored post in PostgreSQL.
3. Check its classification. For an eligible post, compare the extracted product details and S3 object with the source. Open the storefront and look for the product with the corresponding model filter.
4. Record the stage reached and any remaining error. Admin Scan History can help trace a run if available in the deployed version.

<!-- IMAGE 10: 10-storefront.png — the sample listing or the observed result for its model filter. -->

### Expected result

The sample has a traceable record from collection to the last completed stage. A storefront screenshot shows the result when the item is eligible for display.
