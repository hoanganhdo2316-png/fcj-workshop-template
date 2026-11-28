---
draft: true
title: "Deploy FastAPI, Nginx, and HTTPS"
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

### Objective

Deploy the backend and storefront on EC2 and serve them over HTTPS.

### Steps

1. Copy the source to EC2. Create a Python virtual environment in `backend/` and install `requirements.txt`. Set `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, and `DB_PASSWORD` in a private environment file. If using Admin to follow collection runs, also configure `ADMIN_API_TOKEN` on the backend.
2. Run Uvicorn with `main:app` on `127.0.0.1:8000`. Configure a systemd service with `backend/` as its working directory and the environment file above. Check `/api/health` and `/api/db-health` from EC2 before setting up the proxy.
3. Run `npm ci` and `npm run build` in `frontend/`. Serve `dist/` through Nginx, use `index.html` for frontend routes, and proxy `/api/` to Uvicorn. Restrict public access to the database health endpoint.
4. Point the demo domain to the server through its authoritative DNS provider. Issue an HTTPS certificate, validate the Nginx configuration, and open the site over HTTPS.

<!-- IMAGE 04: 04-web-health.png — The webpage over HTTPS and a successful /api/health response. -->

### Expected result

The web interface opens over HTTPS and FastAPI can connect to RDS.
