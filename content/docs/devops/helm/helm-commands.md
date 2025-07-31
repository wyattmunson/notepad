---
title: "Helm Commands"
description: "Helm commands to manage Helm via CLI."
summary: ""
date: 2025-01-16T04:37:11-08:00
lastmod: 2025-01-16T04:37:11-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm upgrade --install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --set installCRDs=true --wait
```
