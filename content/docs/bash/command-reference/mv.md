---
title: "mv"
description: "Move a file with mv."
summary: ""
date: 2025-01-16T03:06:49-08:00
lastmod: 2025-01-16T03:06:49-08:00
weight: 295
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

```bash
# if destination is a dir, target is placed in dir
# if destination is a file, target overwrites file
mv source_file target_file
mv file.txt /home/usr/file.txt

mv -f # force move and overwrite
mv -i # interactive prompt befor everwrite
mv -u # update - move when source is newer than target
mv -v # verbose - print source and target files

# rename file foo to bar
mv foo bar

# move all JSON files to subdirectory bar
mv *.json bar

# move all files in subdirectory foo to current directory
mv foo/* .
```
