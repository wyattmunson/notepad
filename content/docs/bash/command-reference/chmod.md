---
title: "chmod"
description: "The chmod changes file ownership."
summary: ""
date: 2025-01-16T02:11:08-08:00
lastmod: 2025-01-16T02:11:08-08:00
weight: 55
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

{{< callout context="tip" title="chmod" icon="outline/terminal-2" >}}
`chmod` changes file access permissions.
{{< /callout >}}

Changes file permissions: who can read, write, and execute the file.

```bash { title="Basic chmod usage example" }
chmod +x FILE_NAME
chmod 744 FILE_NAME

```

The file permissions can be specified with either the symbolic or numeric method.

**File Permissions**

{{< callout context="caution" title="Understand file permissions" icon="outline/school" >}}
Each file has three personas that can access the file: the file owner (or creator), the group, and all other users. Each persona then has different levels of permissions for that file: read, write, execute, or some combination.
{{< /callout >}}

When using chmod you are specifying two things:

1. **The persona**: file owner, file group, or all users
1. **The permissions**: read, write, execute, or some combination

A single command can specify a single persona or multiple personas. Each persona can (and usually does) have different levels of permissions.

## Two Methods

The `chmod` command uses two methods to update permissions:

1. Symbolic Method: Uses letters like `+x` to update permissions
1. Numeric Method: Uses numbers like `755` to update permissions

### Symobilc Method

{{< callout context="note" title="chmod" icon="outline/rocket" >}}
The symolic method users letters to refer to groups and permissions, e.g., \
`chmod +x FILE_NAME`
{{< /callout >}}

Symbolic method uses letters to refer to the groups and permissions, e.g., `g+x`.

Groups:

- `u` - file owner (user)
- `g` - group, users in the same linux group
- `o` - all other users
- `a` - all users (`u`, `g`, `o`)

Attachments:

- `+` - add specified permission
- `-` - remove specified permission
- `=` - change current permission to specified permission (if no permissions are specified, all permissions are removed)

Permissions:

- `r` - read
- `w` - write
- `x` - execute

**Use `chmod` with symbolic method**:

```bash { title="Change permissions with chmod symbolic method" }
# give file owner execute permissions
chmod u=x FILE_NAME

# remove group permissions to write to file
chmod g-w FILE_NAME

# add execute to file owner, remove all permissions from group
chmod u+x,g= FILE_NAME
```

### Numeric Method

{{< callout context="note" title="Numeric Method" icon="outline/rocket" >}}
The numeric method uses numbers to identify permissions and their position to identify the linux user, e.g., `chmod 744 FILE_NAME`.
{{< /callout >}}

In the numeric method, each persona is assigned a single digit that represents their permissions: read, write, execute, or some combination thereof.

- `r` - read - 4
- `w` - write - 2
- `x` - execute - 1
- No permission - 0

To combine permissions, add the value for each permission:

```
Read (4) + Write (2) + Execute (1) => 7
Read (4) + Execute (1)             => 5
Read (4)                           => 4
```

Permissions for all groups are given in a three digit code. The position of each digit refers to the three linux user group. The order is file owner + file group + all users.

```bash
# numeric group order syntax
chmod [FILE_OWNER][FILE_GROUP][ALL_USERS] FILE_NAME

chmod 755 FILE_NAME
      ^^^
      |||_  all other users (o)
      ||__  group (g)
      |___  file owner
```

**Use `chmod` with numeric method**:

```bash { title="Example use of numeric method" }
# give file owner - read, write, execute; group - read, write; all other users - read
chmod 764 FILE_NAME

# give all permissions to all groups
chmod 777 FILE_NAME
```

## Change permissions for multiple files

Use wildcards (`*`) to specify multiple files in a single command.

```bash { title="Use wildcard with chmod to target multiple files" }
# give file owner execute permissions for all text files in current dir
chmod u=x *.txt
```

## Change permissions in subdirectories

Use the recursive (`-R`) flag to also include subdirectories in the `chmod` command. This will also target all file as well as the target directory's permissions.

```bash { title="Change permissions for all subdirectories and files" }
# give full permissions to all users
chmod -R 777 DIR_NAME
```

```bash { title="Use wildcard and target subdirectories" }
# give file owner execute permissions for all text files
chmod -R u=x *.txt
```
