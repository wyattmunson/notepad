---
title: "Bash Cheatsheet"
description: "Cheatsheet for handy bash commands"
summary: "Cheatsheet for handy bash commands"
lead: "Cheatsheet for handy bash commands"
date: 2023-01-03T23:52:57-08:00
lastmod: 2023-01-03T23:52:57-08:00
draft: false
images: []
menu:
  docs:
    parent: "guides"
    identifier: "bash-cheatsheet-92beadd4d4659911b67d21bcd0e33c74"
weight: 2
toc: true
---

This is a Quick Reference Sheet for bash commands and usage. For a more detailed guide, see [Bash Reference](/docs/bash/bash-reference).

{{< card-grid >}}
{{< link-card title="Bash" href="#file-commands" description="Bash cheat sheet, reference, and snippets." >}}
{{< link-card title="Python" href="/docs/python/" >}}
{{< link-card title="Linux" href="/docs/linux/" >}}
{{< /card-grid >}}

- [File Commands](#file-commands)

## File Commands

### ls

Use `ls` to list files and directories.

```bash
ls        # list current directory files
ls -lah   # show metadata and hidden files
ls /dir   # list specified direcotry's file contents
```

### Changing directories

```bash
pwd       # print working directory - get current location
cd dir    # move into "dir" directory
cd ..     # move up one directory
cd -      # return to previous directory
cd ~      # move to user's home directory
cd /      # move to root directory
```

### Creating a file

```bash
touch FILE      # create a file
vim FILE        # create/open file in vim text editor
nano FILE       # create/open file in nano text editor
mktemp -t FILE  # make temp file in /tmp (deleted on boot)
mkdir DIR       # create a directory
mkdir -p DIR/SUDIR  # create directory and subdirectories
```

### Moving a file

```bash
# if destination is a dir, target is placed in dir
# if destination is a file, target overwrites file
mv source_file target_file
mv file.txt /home/usr/file.txt

mv -f # force move and overwrite
mv -i # interactive prompt befor everwrite
mv -u # update - move when source is newer than target
mv -v # verbose - print source and target files

# rename file foo to bar
mv foo bar

# move all JSON files to subdirectory bar
mv *.json bar

# move all files in subdirectory foo to current directory
mv foo/* .
```

Rsync to move.

```bash
# local to local
rsync [OPTIONS] SOURCE TARGET
# local to remote (push)
rsync [OPTIONS] SOURCE USER@HOST:TARGET
# remote to local (pull)
rsync [OPTIONS] USER@HOST:SOURCE TARGET

# basic usage
rsync -a /source/foo/ /target/foo/

# create/copy of 'foo' into '/target' with subdirs
rsync -azhP /source/foo /target

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

### Copying a file

```bash
cp source_file target_file

# copy directory, and create target_dir if it doesn't exist
cp -r source_dir target_dir

# copy source file to target file
cat  SOURCE_FILE > TARGET_FILE

# merge to files together
cat FILE_1 >> FILE_2
```

### Deleting a file

```bash
rm file_name

# remove directory
rm -r directory_name

# force remove file
rm -f file_name

# display deletion status confirmation
rm -v file_name

# delete a directory (fails if directory is not empty)
rmdir DIRECTORY_NAME
# delete a non-empty directy (and its contents)
rmdir -r DIRECTORY_NAME
# delete directory and its contents
rm -r DIRECTORY_NAME
# force delete directory and its contents
rm -rf DIRECTORY_NAME
```

### Reading a file

```bash
# display contents of file to terminal window
cat FILE_NAME
# display number of lines
cat -n FILE_NAME
# display number of lines (alternate)
nl FILE_NAME
wc FILE_NAME
# display matching lines
cat FILE_NAME | grep -i "search text"
# get line count of matching lines
cat FILE_NAME | grep -i "search text" | wc

# display first 10 lines of file to terminal window
head FILE_NAME
# display last 10 lines of file to terminal window
tail FILE_NAME
# display last XX lines of file to terminal window
tail -n XX FILE_NAME
# display last 10 lines of file and keep open for changes
tail -f FILE_NAME

more FILE_NAME
less FILE_NAME

# find differences in two files
diff file_name1 file_name2

# print every row of 2nd column
awk -F ',' '{print $2}' airports.csv
# print distinct values of 2nd column
awk -F ',' '{print $2}' airports.csv | sort -u
# print each value in 10th column with count (duplicates only)
cut -f10 -d, "airports.csv" | sort | uniq -cd | sed 's/  *//'
# print each value in 10th column with count
cut -f10 -d, "airports.csv" | sort | uniq -c | sed 's/  *//'

# removes lines where given column is empty
awk '$1!=""' input_file

# partial match of text in given column
awk -F ',' '$6~/no longer/' reduced_airlines.csv

# awk variable
awk '/'$VAR'/' airlines.csv
awk -F ',' '$3~/'$VAR'/' airlines.csv
```

### Finding a file

```bash
# seach for file or dir at current dir and all subdirs
find . -name NAME [TYPE]
find . -name search_text
find . -iname search_text   # case insensitive
find . -size +10M           # above size (M, G, T, P)
find . -size -10M           # below size (M, G, T, P)
find . -name "*.jpg"        # wildcard search
find . -name "tmp.???"      # wildcard search
find . -delete              # delete matching files/dirs
find . -maxdepth X          # max subdir depth
find . -type f              # only return files
find . -type d              # only return dirs

# print out directory tree structure
tree
```

#### Searching inside files

Use `grep` to search the contents of a file.

```bash
# search file for matching text
grep search_text file_name
grep -i "phrase" file.txt

# case-insensitive search
grep -i "phrase" file_name

# search current directory and all subdirectories:
grep -R "phrase" .

# find matching file names
ls | grep "phrase"

# use wildcards
ls | grep *report.txt
ls | grep *report*

if grep -q "Text to search" file_name.txt; then
```

### Writing to a File

```bash
# write variable to a file
echo $VAR_NAME > file.txt

# write multiple lines to a file
cat << 'EOF' > /path/to/script.sh
echo "Running script..."
ls
EOF
```

### Creating a Symbolic Link

```bash
# create symbolic link
ln -s FILE_NAME LINK
# check where symbolic link points to
readlink FILE_NAME
```

### Check file information

```bash
file FILE_NAME
file package.json

# use file_list.txt to read file names (separated by line)
file -f file_list.txt
# check inside compressed files
file -z file_name

# check file line count (display: lines, words, bytes)
wc file_name
```

### Get directory size

See the size of a directory

```bash
# get size of directory at given location
du -sh /some/dir

# get size of each file/directory
du -h /some/dir
```

## Alias

```bash
# create basic alias
alias gs='git status'

# create multi-line command
cat << 'EOF' > /path/to/script.sh
echo "command running..."
ls
EOF
echo "alias some_command='bash /path/to/script.sh'" >> ~/.bashrc
```

## Text

## `sed` - Stream EDitor

Sed is a stream editor for filtering and replacing text. Sed probably has a shit tonne of stuff going on, used commonly for REGEX string matching and replacement.

### Use sed to find and replace text

```bash
sed -i "s,TEXT_TO_FIND,TEXT_TO_REPLACE_WITH,g"

# use a sigial charachter in a file and replce the text
sed -i "s,<VERSION_NUMBER>,10,g" someFile.txt

# formatting for macOS 10.9+
sed -i'.original' -e  "s,<SOMETHING_TO_FIND>,REPLACE_TEXT,g" someFile.txt
```

### Alias sed

{{< callout context="tip" icon="outline/terminal-2" >}}
The default `sed` binary on MacOS behaves differently from `sed` on linux machines. Install `gnu-sed` from homebrew.
{{< /callout >}}

Install `gnu-sed` with homebrew on MacOS machines to get the version of `sed` that has similar behavior to linux machines.

Alias `gnu-sed` to `sed` so it will be used instead of the default `sed` command.

```bash
brew install gnu-sed
alias sed=gnu-sed
```

### Cutting

```bash
# cut 1 & 3rd field from file (Tab delimeted)
cut file_name -f 1,3
# cut up to 3rd field from file (Tab delimeted)
cut file_name -f -3
# specify alternate delimeter
cut file_name -d ":" -f -3
```

## System Commands

### Shutdown and Reboot

Various commands to shutdown the system immediately or with a delay.

```bash { title="Shutdown system" }
# shutdown immediately
sudo shutdown -h now

# shutdown in 1 minute
sudo shutdown

# cancel shutdown command
shutdown -c

# alternate
sudo halt
```

Restart / reboot the system.

```bash { title="Restart system" }
# reboot immediately
sudo reboot

# alternate
sudo shutdown -r now
```

#### Using systemcl to shutdown or reboot

Use `systemctl` to shutdown and reboot and avoid using `sudo` for commands. This only works with Ubuntu 15.04 and later.

```bash { title="Restart and shutdown with systemctl" }
systemctl poweroff
systemctl reboot
systemctl suspend
systemctl hibernate
systemctl hybrid-sleep
```

## System Information

```bash
date        # get date
cal         # show current month's calendar
uptime      # show current uptime
w           # display who's online
whoami      # display current logged in user
finger user # display infomation about "user"
uname -a    # show kernel information
df          # show disk usage
du          # show directory space usage
# show disk quota
quota -v
free        # show memory and swap usage
cat /proc/cpuinfo   # CPU information
cat /proc/meminfo   # memory information

# switch to given user
su USER
# swtich to root user (may need: sudo su -)
su -
# execute command as super user
sudo COMMAND
```

### Disk Size

`df` displays the size of each file system.

```bash
df -h
```

## Variables

```bash
# set a variable
VARIABLE_NAME=some_text

# reference a variable
echo $VARIABLE_NAME

# set command output to variable
VAR_NAME=$(ls -la)
```

### Bash Builtin Variables

```bash
$0        # Name of the Bash script
$1 - $9   # First 9 arguments to the script
$#        # Number of arguments were passed to script
$@        # All the arguments supplied to the Bash script.
$?        # The exit status of the most recently run process.
$$        # The process ID of the current script.
$USER     # The username of the user running the script.
$HOSTNAME # The hostname of the machine the script is running on.
$SECONDS  # The number of seconds since the script was started.
$RANDOM   # Returns a different random number each time is it referred to.
$LINENO   # Returns the current line number in the Bash script.
```

### Reading Input

Get user input and save to a variable in an interactive shell session.

```bash
# set a variable
read VARIABLE_NAME
# set a variable, with prompt statement
read -p 'Enter name: ' VARIABLE_NAME
# set a variable, with prompt statement, password
read -sp 'Enter password: ' VARIABLE_NAME
```

## If Statements

```bash
if [ $1 -gt 100]
then
  echo "Some text"
elif [ $2 == 'no']
then
  echo "Alternate matching condition"
else
  echo "Final catch condition"
fi
```

```bash
# use semi-colons to combine lines
if [ $1 -gt 100]; then
  echo "Some text"
fi

# one-liner
if [ $1 -gt 100]; then echo "Some text"; fi
```

### Wildcards

```bash
if [[ $1 == *"$SUBSTRING"* ]]
```

### Boolean Operators

```bash
&&  # and
||  # or
```

```bash
if [[ $1 -gt 100 && $2 == 'yes']]
elif [[ $1 -gt 50 || $2 == 'no']]
```

### Equality Operators

```bash
=
==  # string comparisons
-eq # equals, numeric comparisons
-gt # greater than
-ge # greater than or equal to
-lt # less than
-le # less than or equal to
-ne # not equal
```

## Networking

```bash
# ping
ping google.com

# get DNS information about domain
dig google.com
```

### cURL

```bash
curl https://example.com

# DOWNLOAD FILE
# set name to file_name
curl https://example.com -o file_name
# use existing file name
curl https://example.com -O

# silence output
curl https://google.com -s

# make verbose
curl https://google.com -v

# follow a location (instead of returning redirect)
curl --location https://google.com

# fail fast: return no output on server errors
curl --fail https://google.com

# AUTH: USERNAME / PASSWORD
# username/password in URL
curl http://user:password@example.com/
# username/password in -u flag
curl -u user:password http://example.com/

# curl POST using application/x-www-form-urlencoded
curl --data "birthyear=1905&press=%20OK%20" http://example.com/api

# curl POST using application/json
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"key":"value","key":"value"}' \
  http://example.com/api

#
curl https://mysubdomain.zendesk.com/api/v2/groups.json \
    -v -u myemail@example.com:mypassword


# CERTIFICATES
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

### wget

```bash
# download file with wget
wget http://example.com/file.txt
# specify custom file name (also use to overwrite)
wget -O custom_name.txt http://example.com/file.txt
# download to a specific directory
wget -P Documents/ http://example.com/file.txt
# silence output
wget -p http://example.com/file.txt
# show progress bar
wget -q --show-progress http://example.com/file.txt
```

Send REST requests with wget

```bash
# send GET request
wget -O- http://example.com/posts
# send POST request
wget -O- --method=post -q --body-data='{"key": "value"}' \
  --header=Content-Type:application/json \
  --header="Authorization: Bearer abc123"
  http://example.com/posts
```

## Compressing

```bash
# untar tar.gz files
tar -zxvf file.tar.gz

# specify output file name
tar -zxvf file.tar.gz output_file.txt

# tar using gzip compression
tar -cvzf target.tar.gz source_file_or_directory

# tar using bzip2 compression
tar -cvjf target.tar.bz2 source_file_or_directory

tar -cf file.tar files  # create a tar named file.tar containing files
tar xf file.tar         # extract the files from file.tar
tar czf file.tar.gz files   # create a tar with Gzip compression
tar xzf file.tar.gz     #  extract a tar using Gzip
tar cjf file.tar.bz2    # create a tar with Bzip2 compression
tar xjf file.tar.bz2    # extract a tar using Bzip2
gzip file               # compresses file and renames it to file.gz
gzip -d file.gz         # decompresses file.gz back to file
```

## Searching

```bash
# search file for matching text
grep search_text file_name
grep -i "phrase" file.txt

# case-insensitive search
grep -i "phrase" file_name

# search current directory and all subdirectories:
grep -R "phrase" .

# find matching file names
ls | grep "phrase"

# use wildcards
ls | grep *report.txt
ls | grep *report*
```

## Running Commands

```bash
whereis command # show possible locations of "command"
which command   # show default location of "command" to be used

# see manual page for grep
man grep

# measure execution time to run grep
time grep
```

## Homebrew

```bash
# install formula
brew install FORMULA
# uninstall formula
brew uninstall FORMULA
# list all installed formulae
brew list
# get info on formula
brew info FORMULA
# display all locally available forumae for brewing
brew search
# substring match on forumae for brewing
brew search SEARCH_TEXT

# check version
brew --version
# print help
brew help
# print help for sub command
brew help SUB_COMMAND
# check for problems
brew doctor

# delete old versions of all installed formulae
brew cleanup
# dry-run of delete old versions of all installed formulae
brew cleanup -n
# delete old versions of a given installed formulae
brew cleanup FORMULA

```

### Upgrading Homebrew and Formulas

```bash
# get latest version of homebrew and formula
brew update
# upgrade out of date and unpinned brews
brew upgrade
# upgrade specified brew
brew upgrade FORMULA
# show formulae with updates available
brew outdated
# lock version and prevent updates for given forumlae
brew pin FORMULA
# unlock version and allow updates for given formulae
brew unpin FORMULA
```

### External Repositories Using Tap

Add additional homebrew repositories with `brew tap`.

```bash
# list current taps (tapped repositories)
brew tap
# tap a formula repository from github
brew tap USER/REPO
# tap a formula repository from given URL
brew tap USER/REPO URL
# untap a formula repository
brew untap USER/REPO
```

### Cask

> This is outdated! Use updated `brew <command> --cask` instead.

```bash
# tap the Cask repository in GitHub
brew tap homebrew/cask
# install a given cask
brew cask install CASK
# reinstall a given cask
brew cask reinstall CASK
# uninstall a given cask
brew cask uninstall CASK
# list all installed casks
brew cask list
```

## Bash

### Input and Output Redirection

```bash
cmd1 | cmd2 # pipe output of cmd1 to input of cmd2
< file      # use standard input of a file
> file      # redirect standard output to file
>> file     # redirect standard output to file, (append)
<> file     # use file as stdin and stdout
&> file     # redirect stdout and stderr
2> file     # redirect stderr only
```

### Select last charachters in string

Extract the last charachters in a string

```bash
stringer="tester"
echo "${stringer: -1}"
# ⮑ r
```

### Remove last n charachters in string

Exclude the last charachters in a string

```bash
stringer="someFile.md"
short_str=${stringer::-3}
echo $short_str
# ⮑ someFile
```

## Misc Commands

### Set

```bash
set -euxo pipefail
```

- `set -u` - attempts to use undefined variables as errors
- `-e` - exits immediately if a command fails
- `-x` - enable debugging by printing every command before it is executed
- `-o pipefail` - exit status of first non-zero exit code in a command pipeline (`cmd1 | cmd2 | cmd3`). Normal behavior is to return the exit code of the last command.

## Mac Commands

Commands that are specific to mac machines

### List all USB devices

```bash
ioreg -p IOUSB
```

```bash
+-o Root  <class IORegistryEntry, id 0x100000100, retain 37>
  +-o AppleT8112USBXHCI@00000000  <class AppleT8112USBXHCI, id 0x100000395, registered, matched, active, busy 0 (94208 ms), retain 1353>
  | +-o USB3.0 Hub             @00200000  <class IOUSBHostDevice, id 0x1000527a9, registered, matched, active, busy 0 (13629 ms), retain 38>
  | | +-o AX88179A@00230000  <class IOUSBHostDevice, id 0x1000528c5, registered, matched, active, busy 0 (280 ms), retain 37>
  | | +-o Ugreen Storage Device@00220000  <class IOUSBHostDevice, id 0x100052a51, registered, matched, active, busy 0 (13233 ms), retain 124>
  | +-o USB2.0 Hub             @00100000  <class IOUSBHostDevice, id 0x1000527c0, registered, matched, active, busy 0 (509 ms), retain 40>
  |   +-o USB2.0 Hub@00140000  <class IOUSBHostDevice, id 0x1000527db, registered, matched, active, busy 0 (482 ms), retain 38>
  |   | +-o USB2.0 HUB@00144000  <class IOUSBHostDevice, id 0x1000527fb, registered, matched, active, busy 0 (397 ms), retain 39>
  |   | | +-o Yeti Stereo Microphone@00144200  <class IOUSBHostDevice, id 0x100052815, registered, matched, active, busy 0 (45 ms), retain 37>
  |   | | +-o AKKO 3108 V2@00144400  <class IOUSBHostDevice, id 0x100052827, registered, matched, active, busy 0 (74 ms), retain 38>
  |   | | +-o L01 HD Webcam@00144300  <class IOUSBHostDevice, id 0x100052852, registered, matched, active, busy 0 (262 ms), retain 34>
  |   | +-o USB Audio@00141000  <class IOUSBHostDevice, id 0x1000528ac, registered, matched, active, busy 0 (70 ms), retain 31>
  |   +-o Anker USB-C Basic Dock @00150000  <class IOUSBHostDevice, id 0x1000528f9, registered, matched, active, busy 0 (6 ms), retain 18>
  +-o AppleT8112USBXHCI@02000000  <class AppleT8112USBXHCI, id 0x10000046c, registered, matched, active, busy 0 (13795 ms), retain 115>
  +-o AppleT8112USBXHCI@01000000  <class AppleT8112USBXHCI, id 0x10000048c, registered, matched, active, busy 0 (41897 ms), retain 428>
    +-o ArmorATD@01200000  <class IOUSBHostDevice, id 0x10005295a, registered, matched, active, busy 0 (6811 ms), retain 126>
```
