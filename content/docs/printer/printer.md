---
title: "Printer"
description: ""
summary: ""
date: 2024-06-21T18:39:16-07:00
lastmod: 2024-06-21T18:39:16-07:00
draft: true
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

# Reinstall icu4c

brew reinstall icu4c

```bash
$ npm run dev
dyld[64354]: Library not loaded: /opt/homebrew/opt/icu4c/lib/libicui18n.73.dylib
  Referenced from: <04514B46-460C-3198-8477-203726DC0E89> /opt/homebrew/Cellar/node/21.6.2/bin/node
  Reason: tried: '/opt/homebrew/opt/icu4c/lib/libicui18n.73.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/opt/icu4c/lib/libicui18n.73.dylib' (no such file), '/opt/homebrew/opt/icu4c/lib/libicui18n.73.dylib' (no such file), '/usr/local/lib/libicui18n.73.dylib' (no such file), '/usr/lib/libicui18n.73.dylib' (no such file, not in dyld cache), '/opt/homebrew/Cellar/icu4c/74.2/lib/libicui18n.73.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Cellar/icu4c/74.2/lib/libicui18n.73.dylib' (no such file), '/opt/homebrew/Cellar/icu4c/74.2/lib/libicui18n.73.dylib' (no such file), '/usr/local/lib/libicui18n.73.dylib' (no such file), '/usr/lib/libicui18n.73.dylib' (no such file, not in dyld cache)
zsh: abort      npm run dev
(base)
```

Hello test

Print with Markdown

1. Upload files to dropbox
1. Linux icron.d process executes a script
1. Pandoc converts md to html
1. Htmldoc converts html to a pdf
   - 79.5x297mm pdf
   - T 1mm, B 1mm, L 3 mm, R 5 mm
1. Command run to Epson printer
1. Files are moved to a complete folder
1. Print weather report

### Converting Files

Convert MD to HTML

```bash
# list out printers
pandoc FILE.md -o FILE.html --wrap=preserve -f markdown -t html
```

Convert HTML to PDF

```bash
htmldoc -f FILE.pdf README.html \
--no-toc \
--no-title \
--no-numbered \
--header . \
--footer . \
--top 1mm \
--bottom 1mm \
--left 3.5mm \
--right 5mm \
--size 79.5x297mm \
--embedfonts \
--gray
```

### Send to printer

```bash
# list out printers
lpstat -p

# print test.txt file to printer
lp -d _192_168_1_27 "test.txt"

```

### Install wkhtmltopdf

https://wkhtmltopdf.org/downloads.html
