---
title: "cp"
description: "The cp command copies a file or directory from a source location to a new or existing targe` location."
summary: ""
date: 2025-01-16T02:09:46-08:00
lastmod: 2025-01-16T02:09:46-08:00
weight: 58
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

{{< callout context="tip" title="Command: Copy" icon="outline/terminal-2" >}}
The `cp` command copies a file or directory from a `source` location to a new or existing `target` location.
{{< /callout >}}

```bash { title="Copy quick reference" }
# syntax
cp [OPTIONS] <sourceFile> <destinationFile>
cp [OPTIONS] SOURCE_DIRECTORY DESTINATION_DIRECTORY

# basic usage
cp sourceFile targetFile

# copy contents of dir1 into dir2
cp -R dir1 dir2

# copy file and add to an existing target dir
cp fileName ../targetDirectory

# copy all files within source directory to an existing target dir
cp -R sourceDirectory/* targetDirectory/
```

## Copy a file

Copies content of `file1` to `file2`. Creates file2 if it doesn't exist, overwrites it if it does.

#### Copy file to directory

A file can be copied to an existing directory. The file will be added to the target directory without affecting the other files in the target. If the source file exists in the target, it will be overwritten.

```bash { title="Copy a file to an existing directory" }
cp file1 dir1
```

### Copy multiiple files to a directory

Overwrites contents of directory if it exits, creates one if it does not. Can supply 1..n files.

```bash { title="Copy multiple files to a target directory" }
cp file1 file2 file3 targetDirectory
```

## Copy directories

Copies entire contents of source directory to target directory.

```bash {title="Copy a directory"}
cp -R sourceDirectory targetDirectory
```

The behavior depends on if the target directory exists:

- If the target directory does not exist, it is created. The contents of the source dir are copied into the target.
- If the target directory does exist, the source is copied as a subdirectory into the target directory with the name of source directory

```bash
$ find .
↪ sourceDir/subDir/file.txt
  existingDir/

# copy to an existing directory
$ cp -R sourceDir existingDir
$ find existingDir
↪ existingDir/sourceDir/subDir/file.txt

# copy to a new directory
$ cp -R sourceDir newDir
$ find newDir
↪ newDir/subDir/file.txt
```

{{< callout context="caution" icon="outline/alert-triangle" >}}
If the target directory exists, the source directory becomes a subdirectory in the target directory.
{{< /callout >}}

### Other copy flags

#### Wildcards

Use wildcards to target multiple source files to copy.

```bash { title="Use wildcards" }
cp *.txt targetDirectory
```

#### Force copying

Force copying, even in cases when user lacks write permissions, delete destination file if necessary.

```bash { title="Force copy a file" }
cp -f file1 file2
```

#### Create backup

Create a backup of the target file in the same folder with a different name and format.

```bash { title="Create a backup when copying" }
cp -b file1 file2
```

#### Preserve file charachteristics

Preserve file charachteristics like modification time, access time, ownership, and permissions.

```bash { title="Preserve charachteristics" }
# preserve charachteristics
cp -p file1 file2
```

#### Interactive mode

```bash { title="Interactive mode to prompt before overwriting" }
# interactive - promt before overwriting
$ cp -i s.txt f.txt
```
