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

# Git

- [Git CLI commands](#git-cli-commands)
  - [Initial repository setup](#initial-repository-setup)
  - [Branches](#branches)
  - [Git Config](#git-config)
  - [Remotes](#remotes)
  - [Tag Commit](#tag-commit)

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

#### Configure a remote to use a PAT

Use a Personal Access Token to authenticate with an external git provider like GitHub.

This is useful for private repositories that cannot use password or SSH based authentication.

```bash { title="Remove git remote" }
# run `git remote add` command first as normal
git remote set-url origin https://USER_NAME:PAT@github.com/ORG_NAME/REPO_NAME.git
```

- `USER_NAME` - GitHub username of user that owns/created PAT
- `PAT` - Classic GitHub PAT (Personal access token). This token can additionally be set to authenticate with SSO if within a company managed GH org.
- `ORG_NAME` - GitHub Org name, or username if not with a GH org
- `REPO_NAME` - Name of GH repository

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
