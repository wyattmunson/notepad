---
title: "Git"
description: ""
summary: ""
date: 2024-07-01T13:22:34-07:00
lastmod: 2024-07-01T13:22:34-07:00
draft: false
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Git CLI commands

```bash { title="Basic git flow commands" }
# see current branch and file status
git status

# add commits to staging
git add .

# add commit message
git commit -m "fix: update date selector"

# push
git push
git push -u REMOTE_NAME BRANCH_NAME
git push -u origin main
```

### Branches

```bash { title="Basic git branch commands" }
# see current branches
git branch
# see all remote branches
git branch -a
# See what braches are tracking upstream branches
git branch -vv

# create new branch (does not checkout to new branch)
git branch NEW_BRANCH_NAME

# rename current branch
git branch -m NEW_BRANCH_NAME
```

```bash { title="Delete git branch" }
# delete git branch
git branch -d <branchName>

# force delete git branch (with unmerged changes)
git branch -D <branchName>
```

#### Checkout to different branch

Use `git checkout` to change to a new or existing branch.

```bash { title="Checkout to different git branch" }
# checkout to existing branch
git checkout EXISTING_BRANCH_NAME

# create and checkout to new branch
git checkout -b NEW_BRANCH_NAME
```

### Initial repository setup

```bash
git init
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:USERNAME/REPO_NAME.git
git push -u origin main
```

### Staging Changes

Before commiting changes, they must be `staged` for commit.

```bash { title="Stage changes for commit" }
# git add all changes at current dir (and below)
git add .

# git add all changes in repo
git add -A

# git add specific files or dirs
git add dir/
```

```bash { title="Unstage changes for commit" }
# unstage all changes
git reset HEAD

# unstage specific changes
git reset HEAD dir/file.txt
```

```bash { title="Unstage changes for commit and delete all changes" }
git reset HEAD
git checkout .
```

### Git Config

Git Config stores metadata - globally and per repository.

```bash
git config --list

git config --global user.email "your_email@example.com"

# add git config
git config --add core.sshcommand "ssh -i ~/.ssh/github"

# remove git config
git config --unset remote.github.url
```

### Remotes

Remotes refer to the location of a repository stored on an online tool like GitHub or GitLab, versus the local copy stored on each users machine. When pushing and pulling, the command is targeting a remote.

Most repos push and pull from a single remote called `origin`. In more advanced use cases, multiple remotes are used for repos stored in multiple locations.

```bash
# see remote origin
git remote show origin

# add another remote
git remote add REMOTE_NAME REMOTE_URL
git remote add origin git@github.com:USERNAME/REPO_NAME.git
git remote add second-remote git@github.com:USERNAME/REPO_NAME.git
```

```bash { title="Remove git remote" }
git remote remove origin
```

### Tag Commit

```bash
# create a tag
git tag -a v0.1.0 -m "Version 0.1.0 - do a thing"

# push tags to remote
git push --tags
```

### Pulling without pull strategy set

```
warning: Pulling without specifying how to reconcile divergent branches is
discouraged. You can squelch this message by running one of the following
commands sometime before your next pull:

  git config pull.rebase false  # merge (the default strategy)
  git config pull.rebase true   # rebase
  git config pull.ff only       # fast-forward only

You can replace "git config" with "git config --global" to set a default
preference for all repositories. You can also pass --rebase, --no-rebase,
or --ff-only on the command line to override the configured default per
invocation.
```

## Authenticate with External Git Providers

Different methods for authenticating with external git providers like GitHub.

### Configure a remote to use a PAT

Use a Personal Access Token to authenticate over HTTPS with an external git provider like GitHub.

This is useful for private repositories that cannot use password or SSH based authentication.

```bash { title="Authenticate with PAT" }
# run `git remote add` command first as normal
git remote set-url origin https://USER_NAME:PAT@github.com/ORG_NAME/REPO_NAME.git
```

- `USER_NAME` - GitHub username of user that owns/created PAT
- `PAT` - Classic GitHub PAT (Personal access token). This token can additionally be set to authenticate with SSO if within a company managed GH org.
- `ORG_NAME` - GitHub Org name, or username if not with a GH org
- `REPO_NAME` - Name of GH repository

### Specify a certain SSH key

Use a specific SSH key to authenticate over SSH with an external git provider like GitHub.

```bash { title="Authenticate with specific SSH key" }
# set given SSH key for current repo
git config core.sshCommand "ssh -i /path/to/keyfile"

# set given SSH key for all repos
git config --global core.sshCommand "ssh -i /path/to/keyfile"
```

{{< callout caution >}} Support for `core.sshCommand` is only available in git version 2.10.0 and above. {{< /callout >}}

![xkcd git](./git_xkcd.png)

## Git Ignore

Use the `.gitignore` file to remove files from git trakcing.

{{< callout context="caution" title="Caution" icon="outline/alert-triangle" >}}
The `.gitignore` file will only ignore files before they are tracked by git. If git is already tracking changes in those files, it will continue to track changes.
{{< /callout >}}

If a file is added to `.gitignore`

### Remove tracked files from the git index

If git is tracking changes in a file before it's added to the `.gitignore`, git will ignore the `.gitignore`. These files must be removed from the git index first.

```bash { title="Remove files from git index" }
git rm --cached -r FILE_PATH
git commit -m "MESSAGE"
```

## Misc Config

### Safe Directory

When git detects dubious file ownership, it will block a `git init`

```bash
git config --global --add safe.directory /var/homelabos
```
