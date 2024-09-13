---
title: "n8n"
description: ""
summary: ""
date: 2024-09-12T23:37:54-07:00
lastmod: 2024-09-12T23:37:54-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Docker

```bash { title="Start Compose" }
docker compose up -d
```

```dockerfile { title="Dockerfile" }
version: "3.7"

services:
  n8n:
    image: docker.n8n.io/n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_HOST=${SUBDOMAIN}.${DOMAIN_NAME}
      - N8N_PORT=5678
      - N8N_PROTOCOL=https
      - N8N_SECURE_COOKIE=false # allow non-SSH access
      - NODE_ENV=production
      - WEBHOOK_URL=https://n8n.example.com/
      - GENERIC_TIMEZONE=America/Los_Angeles
    volumes:
      - ./n8n_data:/home/node/.n8n

volumes:
  n8n_data:
    external: true
```
