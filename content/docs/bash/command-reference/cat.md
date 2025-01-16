---
title: "cat"
description: "Use cat combines files"
summary: ""
date: 2025-01-16T02:16:19-08:00
lastmod: 2025-01-16T02:16:19-08:00
weight: 50
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

{{< callout context="tip" title="cat" icon="outline/terminal-2" >}}
`cat` combines files.
{{< /callout >}}

Intended to combine or concatenate multiple text files into one. Used for a variety of things, like the simple `cat filename.txt` to print out a files contents.

Use `cat` to print out contents of file to terminal:

```bash
$ cat FILE_NAME
$ cat log.txt
```

Use the more or less command to print out one page at a time:

```bash
$ cat file | more
$ cat file | less
```

Use `cat` to combine multiple files:

```bash
# combine file1 and file2 into megaFile.txt
$ cat file1 file2 > megaFile.txt

# combine all files in current directory to monsterFile.txt
$ cat * > monsterFile.txt

# combine all text files
cat *.txt > combined.txt
```

## Cat multiline variable to file

Use `cat` to send a multiline string to a file.

```bash { title="Send multiline string to a file" }
cat <<EOF > file_name.yaml
version: 1
type: prod
settings:
  status: active
  online: false
EOF
```
