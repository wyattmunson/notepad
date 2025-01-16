---
title: "dig"
description: "Use dig to get DNS information about a domain name."
summary: ""
date: 2025-01-16T02:03:11-08:00
lastmod: 2025-01-16T02:03:11-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

{{< callout context="tip" title="Command: dig (Domain Information Grouper)" icon="outline/terminal-2" >}}
The `export` command makes variables and functions available to child shells and processes.
{{< /callout >}}

Get information about DNS name servers. Commonly used to see what domain names resolve to.

```bash
# run dig on github.com
$ dig github.com

# results
#
# ...
#
# ;; ANSWER SECTION:
# github.com.		42	IN	A	140.82.113.3
#
# ...
```

Use the `ANSWER SECTION` to see the DNS records of the search. If there are no answers, the requested DNS record could not be found or does not exist.

```bash
# quick dig
$ dig github.com +short
↪ 140.82.113.3

# get trace route
$ dig github.com +trace

# bring your own nameserver
$ dig @ns1.you-specify.com github.com

# trim dig output
$ dig github.com +nostats       # no stats
$ dig github.com +nocomments    # no header
$ dig github.com +noall +answer # remove everything, show answer

# Request different types of records
$ dig github.com NS
```

Request different types of records

- A = Internet Address, default (IP address)
- TXT = Text annotations
- MX = Mail eXchange (mail servers)
- NS = Name Server
