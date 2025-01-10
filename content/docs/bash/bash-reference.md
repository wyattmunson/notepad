---
title: "Bash Reference"
description: "This is the description metadata"
summary: "Full command reference for common bash commands"
lead: "This is the lead metadata"
date: 2023-01-03T23:53:04-08:00
lastmod: 2023-01-03T23:53:04-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "bash-reference-bc6f6ee953e771df27c55e1b6a071b58"
weight: 10
toc: true
---

Full bash reference for commands and common binaries.

| Command                                         | Name                            | Description                                               |
| ----------------------------------------------- | ------------------------------- | --------------------------------------------------------- |
| [alias](#alias---alias-command)                 | alias                           | Create a shorthand command to reference a longer command. |
| [cat](#cat---concatenate)                       | concatenate                     | Display contents of text file; combine multiple files.    |
| [cd](#cd---change-directory)                    | change directory                | Traverse the directory tree and move to different folders |
| [chmod](#chmod---change-mode)                   | change mode                     | Change file permissions                                   |
| [cp](#cp---copy)                                | copy                            | Copy files or directories                                 |
| [curl](#curl---client-url)                      | curl                            | HTTP utility                                              |
| [cut](#curl---cut)                              | cut                             |                                                           |
| [dig](#dig---domain-information-grouper)        | domain information grouper      | Show DNS information for a URL                            |
| [file](#file-command)                           | file                            | Display file type                                         |
| [find](#find-command)                           | find                            | Find a file                                               |
| [echo](#echo-command)                           | echo                            | Print output to the terminal                              |
| [exit](#exit-command)                           | exit                            | Exit current script or shell session                      |
| [export](#export---export)                      | export                          | Export a variable                                         |
| [grep](#grep---global-regular-expression-print) | global regular expression print | Use regex to find files                                   |
| [kill](#kill-command)                           | kill                            | Stop a running process                                    |
| [ls](#ls---list)                                | list                            | List contents of a directory                              |
| [lsof](#lsof---list-open-files)                 | list open files                 | See running processes, check ports                        |
| [mkdir](#mkdir---make-directory)                | mkdir                           | Create a directory                                        |
| [ps](#ps---process-status)                      | ps                              | See status of running process                             |
| [pwd](#pwd---print-working-directory)           | print working directory         | Show current directory                                    |
| [rm](#rm---remove)                              | remove                          | Removes files or directories                              |
| [rsync](#rsync---rsync)                         | rsync                           | Sync files between locations or remotely                  |
| [sudo](#sudo---super-user-do)                   | super user do                   | Elevates a command to super user privileges               |
| [scp](#scp---secure-copy)                       | secure copy                     | Securely copy file between hosts                          |
| [sed](#sed---stream-editor)                     | stream editor                   | Edit a text stream                                        |
| [ssh](#ssh---openssh-client)                    | ssh                             | HTTP utility                                              |
| [tail](#tail-command)                           | tail                            | Print the end of a file                                   |
| [touch](#touch-command)                         | touch                           | Create a file                                             |
| [type](#type-command)                           | type                            | See file or directory type                                |
| [xargs](#xargs)                                 | xargs                           | Yarr                                                      |
| [Redirect Operators](#redirect-operators)       | Redirect                        |                                                           |

## Syntax Reference

```bash
command ARGUMENT [OPTIONS] [FILE_NAME...]
```

## `alias` - alias command

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

## `cat` - ConcATenate

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

### Cat multiline variable to file

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

---

## `cd` - Change Directory

Probably the most rudimentary command. Allows you to traverse the file system.

```bash
$ pwd                       # print working directory - get current location
↪ /users/benish/code

$ ls                        # list out content of current directory
↪ file.txt  lounge-frontend/  lounge-backend/

$ cd lounge-frontend/       # move into "lounge-frontend"
# pwd /users/benish/code/lounge-frontend

$ cd ..                     # move up one directory
# pwd /users/benish/code

$ cd -                      # return to previous directory
# pwd /users/benish/code/lounge-frontend
```

---

## `chmod` - CHange MODe

{{< callout context="tip" title="chmod" icon="outline/terminal-2" >}}
`chmod` changes file ownership.
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

### Change permissions for multiple files

Use wildcards (`*`) to specify multiple files in a single command.

```bash { title="Use wildcard with chmod to target multiple files" }
# give file owner execute permissions for all text files in current dir
chmod u=x *.txt
```

### Change permissions in subdirectories

Use the recursive (`-R`) flag to also include subdirectories in the `chmod` command. This will also target all file as well as the target directory's permissions.

```bash { title="Change permissions for all subdirectories and files" }
# give full permissions to all users
chmod -R 777 DIR_NAME
```

```bash { title="Use wildcard and target subdirectories" }
# give file owner execute permissions for all text files
chmod -R u=x *.txt
```

---

## `cp` - CoPy

{{< callout context="tip" title="Command: Copy" icon="outline/terminal-2" >}}
The `cp` command copies a file or directory from a `source` location to a new or existing `target` location.
{{< /callout >}}

```bash { title="Copy quick reference" }
# syntax
cp [OPTIONS] <sourceFile> <destinationFile>
cp [OPTIONS] SOURCE_DIRECTORY DESTINATION_DIRECTORY

# basic usage
cp sourceFile targetFile

# copy a dir and its contents to a new dir
cp -R directory1 directory2

# copy file and add to an existing target dir
cp fileName ../targetDirectory

# copy all files within source directory to an existing target dir
cp -R sourceDirectory/* targetDirectory/
```

### Copy a file

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

### Copy directories

Copies entire contents of source directory to target directory.

The behavior depends on if the target directory exists:

- If the target directory does not exist, it is created.
- If the target directory does exist, it is copied as a subdirectory into the target directory with the name of source directory

```bash {title="Copy a directory"}
cp -R sourceDirectory targetDirectory
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

---

## `curl` - Client URL

Transfer an URL.

```bash
curl https://example.com

# get file using FTP
curl ftp://ftp.example.com/README
```

```bash { title="Silence output" }
# silence output
curl https://example.com --silent
curl https://example.com -s
```

Use `-L` to follow redirects if the first response is a 3xx.

```bash { title="Follow redirects" }
# follow a location (instead of returning redirect)
curl --location https://example.com
curl -L https://example.com
```

### Download file with cURL

Download a file using cURL. Specify a file name with `-o` or use `-O` to keep the default file name.

```bash { title="Download a file with cURL" }
# download file and set name to file_name
curl https://example.com --output file_name
curl https://example.com -o file_name

# download file and use same file name
curl https://example.com -O
```

### Usename and password authentication

```bash
# username/password authentication
curl http://user:password@example.com/
# OR
curl -u user:password http://example.com/
```

### HTTP POST request

By default, cURL uses a `GET` request. Use the `-X` flag to specify a different method.

```bash { title="Specify HTTP method" }
# specify a POST HTTP method
curl -X POST https://example.com
```

#### HTTP POST with payload

Use cURL to send a POST command with a JSON payload. Use the `--data` flag to specify a JSON payload.

```bash
# curl POST using application/json
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"key":"value","key":"value"}' \
  http://example.com/api

# curl POST using application/json (alternate flags)
curl -X POST "http://example.com" \
-H "Content-Type:application/json" \
-d '{"username":"admin", "password":"magentorocks1"}'

# curl POST using application/x-www-form-urlencoded
curl --data "birthyear=1905&press=%20OK%20" http://example.com/api
```

#### Set paramaterized HTTP POST response to variable

Set HTTP POST response body to variable, using parameterized data payload and values.

```bash
# curl POST with parameters
API_RESPONSE=$(curl -s --location "https://example.com/api/$VAR_ID?ticketId="$VAR_2"&anotherParamKey=paramValue" -X POST \
--header 'Content-Type: application/json' \
--header 'accept: application/json' \
--header 'Authorization: ApiKey '$VAR_3'' \
--data-raw '{
    "id": "'$VAR_4'",
    "email": "benish@example.com"
}')

# access response object key of "id", set to variable
TEAM_ID=$(echo $API_RESPONSE | jq -r '.id')
```

### See redirect path

Use the `-sILK` flags to see the redirect path of a URL using cURL.

```bash
curl -sILk google.com
```

### Certificates

Use the `--cert` flag to specify a `.pem` file.

```bash
# Certificates
curl --cert mycert.pem https://secure.example.com

# use your own CA cert store to verify server's certificate
curl --cacert ca-bundle.pem https://example.com/

# ignore certificate verification
curl --insecure https://secure.example.com
# OR
curl -k https://secure.example.com

# curl resolve (point to different address for a hostname)
curl --resolve www.example.org:80:127.0.0.1 http://www.example.org/
```

---

## `cut` - CUT

Cut stuff.

### Cut by Field

Cut by field using the `-f` flag. The default delimeter is TAB.

```bash
# sales.txt
2022-20-11  11:34   134.23      Electronics
2022-20-10  10:23   500.34      Appliances
```

Use `cut` to extract the first and third column:

```bash
$ cut sales.txt -f 1,3

# output
2022-20-11  134.23
2022-20-10  500.34
```

Display first through thrid field:

```bash
$ cut sales.txt -f -3

# output
2022-20-11  11:34   134.23
2022-20-10  10:23   500.34
```

### Cut with a Delimiter

Use the `-d` flag to specify a delimeter. By default, cut uses tabs the delimiter.

```bash
cut sales.txt -d ":" -f 1,2
```

---

## `dig` - Domain Information Grouper

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

---

## `echo` command

Prints text to the terminal window.

```bash
$ echo some text
↪ some text
```

---

## `exit` command

Exit current shell session with a given exit code.

```bash
# exit current shell with exit code 1
exit 1
```

---

## `export` - EXPORT

{{< callout context="tip" title="export" icon="outline/terminal-2" >}}
The `export` command makes variables and functions available to child shells and processes.
{{< /callout >}}

Mark variables and functions to be passed to child shells and processes.

Export a variable:

> Bash variables are capatalized by convention.

```bash
export VARIABLE_NAME=VARIABLE_VALUE
export NODE_VERSION=14
```

Get text from file and insert into new file:

```bash
export VERSION=`grep '"version":' package.json | cut -d\" -f4`
export IMAGE_NAME_AND_TAG=$(cat .dev-image-name-and-tag)
echo $IMAGE_NAME_AND_TAG > .stage-image-name-and-tag
```

When sourcing a file, the chile process inherits all the variables of the parent process. If the child process sets variables with `export`, these variables are now available in the parent process.

---

## `file` command

Indicates file type.

Usage

```bash
$ file cats.md      # basic usage
↪ ./cats.md: ASCII text, with very long lines

$ file cats.md      # get MIME type
↪ ./cats.md: text/plain; charset=utf-8

$ file some-directory/* # entire contents of "some-directory"
$ file -z flash.        # content of compressed file

file /dev/sda
file -s /dev/sda
file /dev/sda5
file -s /dev/sda5

```

---

## `find` command

Find a file.

Basic usage:

```bash
find . -name search_text
```

Usage

```bash
# ignore text case
find . -iname search_text

# follow subdirectories to level X
find . -name search_text -maxdepth X

# only find files modified in last 4 days
find . -name search_text -mtime 4

# only find empty
find . -name search_text -type f
```

---

## `grep` - Global Regular Expression Print

Search a file to see if it contains a phrase.

### Search file contents

```bash { title="Search contents of a file with grep" }
grep SEARCH_TEXT FILE_NAME
grep phrase log.txt
```

Case insensitive search:

```bash { title="Use case insensitive search with -i" }
grep -i "phrase" file_name.txt
```

Search current directory and all subdirectories:

```bash { title="Search files in all subdirectories with -R" }
grep -R "phrase" .
```

### Search output of other commands

Commands can be "piped in" to grep using the `|` operator.

```bash
# display only matching files
ls | grep SEARCH_TEXT
ls | grep log.txt

# use wildcards
ls | grep *report.txt
ls | grep *report*

echo "tester" | grep "test"
```

### Get count of matching keyword

```bash
grep -c "keyword" log.txt
```

---

## `kill` command

Stop a process by process ID.

```bash
# see processes running in terminal
$ ps
  PID TTY           TIME CMD
27894 ttys001    0:00.08 -bash

# see process for all users and outside of terminal
$ ps aux
$ ps aux | grep chrome


$ kill SIGNAL PID
$ kill -9 ./applications-service

# see all signal commands
# 9 (SIGKILL) - is the most common command to terminate a process
$ kill -l
```

---

## `ls` - LiSt

List contents of a directory.

List current directory:

```bash
ls
```

List a specified directory:

```bash
ls DIRECTORY
ls /local/usr/bin
```

`ls` flags:

```bash
ls -l
ls -lah

# flags
  # list/table view with date, ownership, and size
  -l
  # list hidden files like .git (starting with a dot ".")
  -a
  # human readable file sizes
  -h
  # list subdirectories
  -R
  # order by last modified
  -t
  # order by file size
  -lS
  # list in reverse order (combine with other commands)
  -r
  # show directories with "/" and executables with "*"
  -F
  # display inode number of file or directory
  -i
```

---

## `lsof` - LiSt Open Files

Use it to check if a process is running on a given port.

```bash
lsof -i :5432
```

`lsof` may be in `/usr/sbin` and needs to be invoked directly (`/usr/sbin/lsof -i :5432`).

---

## `sudo` - Super User DO

`sudo` allows a user to execute a command as the superuser or another user, within the specified security policy.

If a command fails because of insufficient prviledges, the `sudo` command can help. This does elevate permissions and should be used with caution.

```bash
sudo COMMAND
sudo rm -rf /data
```

More:

```bash
# add a new user
sudo useradd

# create password for new user
sudo passwd

# add to a group
sudo groupadd

# delete user
sudo userdel

# delete group
sudo groupdel

# add user to a primary group
sudo usermod -g
```

---

## `man` - MANual

Show manual page for a given command. Use to show the manual for any bash command.

```bash
man COMMAND
man ls
```

---

## `mkdir` - MaKe DIRectory

Creates a directory

```bash
mkdir DIRECTORY_NAME
mkdir some directory

# copy subdirectories
-p
```

---

## `ps` - Process Status

Use to see running processes.

```bash
# show running terminal sessions
ps
```

### Show all running processes

```bash
ps aux

# use grep to find process by keyword
ps aux | grep keyword
```

```bash
# check if a process is running
ps auxwww | grep postgres
```

---

## `pwd` - Print Working Directory

Displays the current directory you're in.

```bash
$ pwd
↪ /home/greg/documents
```

---

## `rm` - ReMove

Delets a file or directory.

```bash
rm FILE_NAME
rm file.txt

# remove directory
rm -r directory_name

# force remove file
rm -f file_name
```

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

---

## `scp` - Secure CoPy

Secure copy is used to securely transfer files from one host to another.

```bash
scp -i SOURCE TARGET
scp -i /home/file.txt user@host:/app/data/file.txt
```

---

## `sed` - Stream EDitor

Sed is a stream editor for filtering and replacing text. Sed probably has a shit tonne of stuff going on, used commonly for REGEX string matching and replacement.

```bash
# syntax
sed -i "s,TEXT_TO_FIND,TEXT_TO_REPLACE_WITH,g"

# example usage
sed -i "s,<VERSION_NUMBER>,2.1.15,g" someFile.txt
```

---

## `ssh` - openSSH client

Remote login program

```bash { title="Use SSH with password prompt" }
ssh ubuntu@172.16.0.105
```

```bash { title="Use SSH with key file" }
ssh -i keyName.pem ec2-user@whatever.amazonaws.com
```

---

## `tail` command

Displays content of files; prints the last 10 lines of a file by default.

```bash
tail FILE_NAME
```

**Follow a file**: do not close tail when end of file is reached, but rather wait for additional input.

```bash
tail -f FILE_NAME

# also check if file is renamed or rotated
tail -F FILE_NAME
```

**Specify length**: specify amount of file returned in lines, blocks, or bytes.

```bash
# specify 50 lines
tail -n 50 FILE_NAME

# specify 50 blocks
tail -b 50 FILE_NAME

# specify 50 bytes
tail -c 50 FILE_NAME
```

---

## `touch` command

Touch command creates a new file.

```bash
touch FILE_NAME
```

```bash
# create multiple files
touch FILE_NAME_1 FILE_NAME_2



```

**Full argument list**

- `-A` Adjust access and modification timestamps of value with specified value.
- `-a` Change access time of file. Modification time is not changed, unless `-m` flag also specified.
- `-c` Do not create the file if it does not exist
- `-d` Change access and modification times
- `-h` If file has symbolic link, change time of the link itself, rather than the file the link points to. Using `-h` implies `-c` and will not create a new file.
- `m` Change modification time of file. Access time is not changed unless `-a` is also specified.
- `r` Use access and modification times from specified file instead of the current time of day.
- `t` Change access and modification times to the specified time instead of the current time of day.

```bash
# Adjust access and modification timestamps of value with specified value
touch

# Change access time of file. Modification time is not changed, unless `-m` flag also specified.
touch -a FILE_NAME

```

---

## `type` command

Type give the user information about the command type.

```
type [OPTIONS] FILE_NAME...
```

---

## `xargs`

Can remove quotes form a

```bash
echo '"text"' | xargs
↪ text
```

### Random Commands

**`xargs`**

Can remove quotes form a

```bash
echo '"text"' | xargs
↪ text
```

# Redirect Operators

## `>` - Redirect Output Operator

Redirects the output of standard output (stdout) and writes it to a given file. If the file exists, it's overwritten.

Redirect standard output to a file:

```bash
# list current directory and write results to file_directory.txt
ls > file_directory.txt
```

### Types of Output

Different types of output

| Number | Type            | Abbreviation |
| ------ | --------------- | ------------ |
| 0      | Standard input  | `stdin`      |
| 1      | Standard output | `stdout`     |
| 2      | Standard error  | `stderr`     |

With the `>` operator, standard output (`1`) is assumed. Using `>` is the same as `1>`.

### Redirect Standard Error

Use `2>` to redirect `stderr`:

```bash
command 2> /dev/null
```

### Redirect Standard Error and Standard Output

Use `&>` to redirect `stderr` and `stdout`:

```bash
command &> /dev/null
```

In Bash, `&>` is the same as `>&`; however, the former is preferred.

More complex solutions achieve the same results

```bash
command >file 2>&1
```

What happened:

- The first redirection `>file` prints `stdout` to `file`
- The second redirection `2>&1` duplicates file descriptor `2` to be the same as `1`

### Discard Output of Command

```bash
command > /dev/null
```

The location `/dev/null` immediately deletes all data written to it. Also known as the "bit bucket."

Discard output of stdout and stderr:

```bash
command &> /dev/null
# OR
command >/dev/null 2>&1
```

## `>>` - Redirect Output Append Operator

Redirects the output of standard output (stdout) and writes it to a given file. If the file exists, it's appended to the contents of the file.

```bash
# list current directory and write results to file_directory.txt
ls >> file_directory.txt
```

### Redirect Standard Error and Standard Output`

Use `&>>` to redirect `stderr` and `stdout`:

```bash
command &>> file.txt
```

## `<` - Regular Input Operator

The input redirector pulls data in a stream from a given source.

Take the given text file for example:

```bash
# states.txt
California
Alaska
New York
```

Use above text file as input to `sort` command:

```bash
$ sort < states.txt
Alaska
California
New York
```

## `|` - Pipe Operator

Takes the output of a given program and redirects it as input for another program. This is known as "piping".

Use `ls` to print directory contents and use `grep` to find matching regex pattern:

```bash
# show files names that match "2004"
ls | grep 2004
```

https://catonmat.net/bash-one-liners-explained-part-three
