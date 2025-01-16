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

Invert match - show lines that do not match:

```bash { title="Find lines that do not match" }
grep -v "phrase" file_name.txt
```

Search current directory and all subdirectories:

```bash { title="Search files in all subdirectories with -R" }
grep -R "phrase" .
```

Specify multiple search patterns:

```bash { title="Specify multiple search patterns" }
grep -e "phrase1" -e "phrase2" file
```

Match whole words only:

```bash { title="Match whole words only" }
grep -w "search_word" file
```

### Get count of matches

Get count of matching keyword:

```bash
grep -c "keyword" log.txt
```

Count the number of matching lines:

```bash { title="Count the number of matching lines" }
grep -n "phrase" file
```

### Show lines before and after

Show N lines after a match.

```bash { title="Show N lines after a match" }
grep -A 3 "pattern" file
```

Show N lines before a match.

```bash { title="Show N lines before a match" }
grep -B 3 "pattern" file
```

Show N lines before and after a match (context).

```bash { title="Show N lines before and after a match" }
grep -C 2 "pattern" file
```

### Use extended regex

```bash { title="Use extended regex" }
grep -E "pattern1|pattern2" file
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
