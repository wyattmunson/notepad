---
title: "rsync"
description: "Use rsync to transfer files via CLI."
summary: ""
date: 2025-01-16T02:24:14-08:00
lastmod: 2025-01-16T02:24:14-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## `rsync` - Rsync

{{< callout context="tip" title="rsync" icon="outline/terminal-2" >}}
The numeric method uses numbers to identify permissions and their position to identify the linux user, e.g., `chmod 744 FILE_NAME`.
{{< /callout >}}

### Sync files locally

```bash { title="Use rsync locally" }
rsync -avh source/ target/
```

This will sync all the files in `source/` to `target/`. If `target/` does not exist it will be created.

```bash
# local to local
rsync [OPTIONS] SOURCE TARGET
# local to remote (push)
rsync [OPTIONS] SOURCE USER@HOST:TARGET
# remote to local (pull)
rsync [OPTIONS] USER@HOST:SOURCE TARGET

# basic usage
rsync -a /source/foo/ /target/foo/

# flags
  -a          # archive mode - sync recursively
  -z          # force compression to send to destination machine
  -P          # progress bar and keep partially transferred files
  -q          # quiet to supress non-error messages
  -v          # verbose
  -h          # human readable output

# copy local source to remote target
rsync -a /foo/ user@remote_host:/foo/
# copy from remote source to local target
rsync -a user@remote_hostname:/foo/ /foo/
# use for large transfers and/or unstable internet connections
rsync -aP source target
# exclude "directory" and "another_dir"
rsync -a --exclude=directory --exlucde=another_dir source target
# include json files and no others
rsync -ae ssh --include="*.json" --exlucde="*" source target
# delete files that are not in source directory
rsync -a --delete source target
# rsync over ssh
rsync -ae ssh user@remote_hostname:/foo/ /foo/
# SSH other than port 22
rsync -a -e "ssh -p 123" /source/ user@remote_hostname:/target/
# specify max file size for transfer
rsync -a --max-size="200k" source target
# do dry run
rsync --dry-run --remove-source-files -v source target
# set bandwidth limit
rsync --bw-limit=100 -a source target
```
