---
title: "export"
description: "The export command makes variables and functions available to child shells and processes."
summary: ""
date: 2025-01-16T02:31:11-08:00
lastmod: 2025-01-16T02:31:11-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

{{< callout context="tip" title="export" icon="outline/terminal-2" >}}
The `export` command makes variables and functions available to child shells and processes.
{{< /callout >}}

Mark variables and functions to be passed to child shells and processes.

Export a variable:

> Bash variables are capitalized by convention.

```bash
export VARIABLE_NAME=VARIABLE_VALUE
export NODE_VERSION=14
```

Get text from file and insert into new file:

```bash
export VERSION=`grep '"version":' package.json | cut -d\" -f4`
export IMAGE_NAME_AND_TAG=$(cat .dev-image-name-and-tag)
echo $IMAGE_NAME_AND_TAG > .stage-image-name-and-tag
```

When sourcing a file, the chile process inherits all the variables of the parent process. If the child process sets variables with `export`, these variables are now available in the parent process.
