---
title: "Login Banner"
description: ""
summary: "Configure a login banner to display to users during a SSH login."
date: 2024-10-08T20:26:04-07:00
lastmod: 2024-10-08T20:26:04-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Use a login banner to display a message when a user is initialting a SSH session.

When using password-based authentication, this message will appear before the password prompt. If using SSH authentication, this message will appear alongside any post-login banners.

## Configure Pre Login Banner

To create a login message, edit the `/etc/issue.net` file, update the SSH config `Banner` entry, and reaload the SSH service.

### Create banner message

Update the `/etc/issue.net` file.

```bash { title="Edit issue.net file" }
sudo vim /etc/issue.net
```

### Update SSH config file

Enable the banner in `/etc/ssh/sshd_config`.

```bash { title="Open SSH confile file" }
sudo vim /etc/ssh/sshd_config
```

Uncomment the `Banner` line and add the path to the banner file.

```bash { title="/etc/ssh/sshd_config" }
Banner /etc/issue.net
```

### Reload the SSH service

Reload the SSH service using `systemctl`.

```bash
sudo systemctl reload ssh
```

{{< callout context="caution" icon="outline/alert-triangle" >}}
Changes to the `/etc/ssh/sshd_config` file will not take effect until the SSH service is reloaded.
{{< /callout >}}

## Post Login Banner

Use the `motd` (or, Message of the Day) file to generate a post-login banner.

```bash
sudo vim /etc/motd
```

--exclude='.Spotlight-V100' --exclude='.TemporaryItems' --exclude='.fseventsd'
