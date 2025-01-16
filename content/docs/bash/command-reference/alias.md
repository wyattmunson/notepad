---
title: "alias"
description: "Use alias to create a shorthand command."
summary: ""
date: 2025-01-16T02:15:19-08:00
lastmod: 2025-01-16T02:15:19-08:00
weight: 10
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

{{< callout context="tip" title="alias" icon="outline/terminal-2" >}}
Use `alias` to create a shorthand command.
{{< /callout >}}

Alias can be used to create a shorthand command. For example `gs` can be used instead of typing `git status`.

```bash
alias gs="git status"
```

The alias command should be saved to the `.bash_profile`, `.bashrc`, or equivalent file so it's available every time a new shell session starts.

```bash
echo "alias some_command='bash some_script.sh'" >> ~/.bashrc
```

### Save and alias multiline command

```bash
cat << 'EOF' > /path/to/script.sh
echo "command running..."
ls
EOF
echo "alias some_command='bash /path/to/script.sh'" >> ~/.bashrc
```

The above script:

- Save a multiline command to a file, specifically a bash script
- Use alias to create the command `some_command` that invokes that the bash script
- Saves it to the `~/.bashrc` file so the command is available every time a new shell session starts
