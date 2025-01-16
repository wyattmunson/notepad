---
title: "grep"
description: "Use grep to find content in files or match string with regex matching."
summary: ""
date: 2025-01-16T02:06:17-08:00
lastmod: 2025-01-16T02:06:17-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

{{< callout context="tip" title="Command: grep (Global Regular Expression Print)" icon="outline/terminal-2" >}}
Use grep to find content in files or match string with regex matching.
{{< /callout >}}

Search a file to see if it contains a phrase.

## Search file contents

```bash { title="Search contents of a file with grep" }
grep SEARCH_TEXT FILE_NAME
grep phrase log.txt
```

Case insensitive search:

```bash { title="Use case insensitive search with -i" }
grep -i "phrase" file_name.txt
```

Search current directory and all subdirectories:

```bash { title="Search files in all subdirectories with -R" }
grep -R "phrase" .
```

## Search output of other commands

Commands can be "piped in" to grep using the `|` operator.

```bash
# display only matching files
ls | grep SEARCH_TEXT
ls | grep log.txt

# use wildcards
ls | grep *report.txt
ls | grep *report*

echo "tester" | grep "test"
```

## Get count of matching keyword

```bash
grep -c "keyword" log.txt
```
