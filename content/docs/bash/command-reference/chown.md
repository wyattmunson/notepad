---
title: "chown"
description: "Use chown to change file ownership in bash."
summary: ""
date: 2025-01-17T15:06:43-08:00
lastmod: 2025-01-17T15:06:43-08:00
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

{{< callout context="tip" title="Command: chown - CHange OWNership" icon="outline/terminal-2" >}}
`chown` changes file ownership.
{{< /callout >}}

```bash { title="Basic usage of chown" }
chown OWNER_NAME FILE_NAME
chown root file.txt
```

```bash
# change owner to root
chown root file.txt
# change group to admin
chown :admin file.txt
# change owner to root, and group to admin
chown root:admin file.txt

# recursive (affects all subdirectories)
chown -R root:admin ./some/dir

# change ownership of multiple files
chown root:admin file1.txt file2.txt
```

```bash { title="Common flags for chown" }
chown OWNER_NAME FILE_NAME
# print confirmation of ownership change
chown -c root file.txt

# print verbose logs
chown -v root file.txt

# force changes, suppress error logs
chown -f root file.txt

# only change where current user is admin
chown --from=admin root file.txt

# only change where current group is someGroup
chown --from=:someGroup root file.txt

# copy file ownership from one file to another
chown --reference=file1.txt file2.txt
```

## View Group Information

```bash { title="View all groups" }
# view current user's group
groups

# view all groups
cat /etc/group
```
