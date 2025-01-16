---
title: "sed"
description: "Sed is a stream editor for filtering and replacing text. "
summary: ""
date: 2025-01-16T02:25:20-08:00
lastmod: 2025-01-16T02:25:20-08:00
weight: 465
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## `sed` - Stream EDitor

Sed is a stream editor for filtering and replacing text. Sed probably has a shit tonne of stuff going on, used commonly for REGEX string matching and replacement.

```bash
# syntax
sed -i "s,TEXT_TO_FIND,TEXT_TO_REPLACE_WITH,g"

# example usage
sed -i "s,<VERSION_NUMBER>,2.1.15,g" someFile.txt
```
