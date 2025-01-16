---
title: "cut"
description: "Cut stuff."
summary: ""
date: 2025-01-16T02:29:55-08:00
lastmod: 2025-01-16T02:29:55-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Cut stuff.

### Cut by Field

Cut by field using the `-f` flag. The default delimeter is TAB.

```bash
# sales.txt
2022-20-11  11:34   134.23      Electronics
2022-20-10  10:23   500.34      Appliances
```

Use `cut` to extract the first and third column:

```bash
$ cut sales.txt -f 1,3

# output
2022-20-11  134.23
2022-20-10  500.34
```

Display first through thrid field:

```bash
$ cut sales.txt -f -3

# output
2022-20-11  11:34   134.23
2022-20-10  10:23   500.34
```

### Cut with a Delimiter

Use the `-d` flag to specify a delimeter. By default, cut uses tabs the delimiter.

```bash
cut sales.txt -d ":" -f 1,2
```
