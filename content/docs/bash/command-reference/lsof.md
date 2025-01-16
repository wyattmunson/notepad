---
title: "lsof"
description: "Use lsof to check if a process is running on a given port."
summary: ""
date: 2025-01-16T02:20:14-08:00
lastmod: 2025-01-16T02:20:14-08:00
weight: 245
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Use it to check if a process is running on a given port.

```bash
lsof -i :5432
```

`lsof` may be in `/usr/sbin` and needs to be invoked directly (`/usr/sbin/lsof -i :5432`).
