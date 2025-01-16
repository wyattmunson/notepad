---
title: "ssh"
description: "Remote login program"
summary: ""
date: 2025-01-16T02:25:24-08:00
lastmod: 2025-01-16T02:25:24-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Remote login program

```bash { title="Use SSH with password prompt" }
ssh ubuntu@172.16.0.105
```

```bash { title="Use SSH with key file" }
ssh -i keyName.pem ec2-user@whatever.amazonaws.com
```
