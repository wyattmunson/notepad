---
title: "Bash Snippets"
description: ""
summary: ""
date: 2024-10-22T19:11:44-07:00
lastmod: 2024-10-22T19:11:44-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Get Filename and Extension

```bash
filename=$(basename -- "$fullfile")
extension="${filename##*.}"
filename="${filename%.*}"
```



```bash
# output somefile.txt
txt
somefile

#output dir/somefile.txt
txt
somefile
```
