---
title: "Create private S3 media storage"
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

### Objective

Set up private media storage and grant access through the server role.

### Steps

1. In **Amazon S3**, create a media bucket in Singapore and keep **Block Public Access** enabled. Scope EC2's IAM role to read and write the required bucket and media prefix.
2. Configure the backend and Media Worker to use the bucket. For eligible posts, the worker uploads the image to S3 and records its key in PostgreSQL.
3. The backend returns a presigned browser URL. Check that the image renders and that the configuration points to the intended bucket. If you add the target architecture's optional S3 gateway endpoint, associate it with the EC2 route table and verify connectivity again.

<!-- IMAGE 05: 05-s3.png — Block Public Access enabled and the sample image object. -->

### Expected result

The worker can store images and the storefront can read them through presigned URLs.
