---
title: "Bash Snippets"
description: ""
summary: "Handy snippets of Bash code."
date: 2024-10-22T19:11:44-07:00
lastmod: 2024-10-22T19:11:44-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Get Filename and Extension

```bash
filename=$(basename -- "$fullfile")
extension="${filename##*.}"
filename="${filename%.*}"
```

```bash
# output somefile.txt
txt
somefile

#output dir/somefile.txt
txt
somefile
```

## Write Multiple Lines to File

```bash
cat << 'EOF' > /path/to/script.sh
echo "command running..."
ls
EOF
```

### Write Multiple Lines to Variable

```bash
MULTILINE_VAR=$(cat <<EOF
    {
    "title":"Hello localhost",
    "provider":{
        "type":"github",
        "username":"",
        "password":"${PASSWORD}",
        "host":""
        },
    }
EOF
)
```

## Check if script is called directly

Check if the current script was called directly, instead of using `source`.

```bash
# Check if the current script was called directly
if [[ "${BASH_SOURCE[0]}" == "${0}" ]]; then
    echo "ERROR: Script called directly. Use 'source' to call this script."
    echo 'RUN => source ./aliash.sh'
    exit 1
fi
```

## Process File Path

```bash
# call script with var: script.sh /some/file/path

# extracts file name and extension from path
# using $1 (-- allows file names starting with hyphens)
fileNameOnly=$(basename -- "$1")
# returns file extension (removes everything including last dot)
extension="${fileNameOnly##*.}"
# returns file name (removes dot followed by anything at the end)
fileNameOnly="${fileNameOnly%.*}"
```

## cURL with Parameters

ADDED: TO - command-reference/curl

```bash
RESPONSE=$(curl -s "https://example.com/api/users?account="$ACCOUNT"&realm="$REALM"" \
  -H 'x-api-key: '$TOKEN'' \
  -H 'accept: */*' \
  -H 'content-type: application/json' \
  --data-raw '{"data":{"id":"'$USER_ID'","email":"'$EMAIL'","tags":{"key": "value"}}}' \
  --compressed)

STATUS=$(echo $RESPONSE | jq -r ".status")

if [ "$STATUS" = "success" ]; then echo "RES OK"; fi
```

### cURL with URL encoded parameters.

```bash
RESPONSE=$(curl -s --location "http://example.com" \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'user_name=gregbenish' \
--data-urlencode 'password='$PASSWORD'')
```

### Capture HTTP Response Code

ADDED: TO - command-reference/curl

```bash
RES_CODE=$(curl --write-out %{http_code} --silent --output /dev/null --location "http://exmaple.com" \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer '$TOKEN'' \
--data-raw '{
    "email": "'$EMAIL'",
    "lastName": "Benish"
}')

if [[ "$RES_CODE" -eq 201 ]]; then echo "Got 201"; fi

if [[ "$RES_CODE" -ge 200 && "$RES_CODE" -lt 300 ]]; then
  echo "Got 2xx response code"
fi
```

### cURL as a Function

ADDED: TO - command-reference/curl

```bash
curl_cmd() {
  local URL="$1"
  local METHOD="$2"   # HTTP method
  local PAYLOAD="$3"  # payload
  local HEADER="$4"
  local OUTPUT
  local EXIT_CODE

  # Input validation
  if [ -z "$URL" ] || [ -z "$METHOD" ]; then
    echo "Error: URL and method are required"
    return 1
  fi

  # Execute curl command
  OUTPUT=$(curl -sfS \
    "${URL}" \
    -X "${METHOD}" \
    -d "${PAYLOAD}" \
    -H "${HEADER}")

  CURL_EXIT_CODE=$?

  # Error handling and debugging
  if [ $CURL_EXIT_CODE -ne 0 ]; then
    echo "ERROR: cURL command failed with exit code $CURL_EXIT_CODE"
    echo "COMMAND ATTEMPTED: curl -sfS ${URL} -X ${METHOD} -d ${PAYLOAD} -H ${HEADER}"
    echo "curl output: ${OUTPUT}"
    return $CURL_EXIT_CODE
  fi

  echo "OUTPUT: ${OUTPUT}"
  return $OUTPUT
}

curl_cmd "${url}" "POST" "${json_data}" "X-Api-Key: ${PV_HARNESS_PAT}"
```

## Wait for File Creation Before Continuing

Wait for a file to exist/be created before continuing.

```bash
while [ ! -f /some/file/path ]; do
    echo "Waiting for task to finish"
    sleep 1
done
```

- The `-f` check if a file exists
- The `!` negates the condition

### Wait for HTTP endpoint before continuing

ADDED: TO - command-reference/curl

```bash
while ! curl --silent --fail --output /dev/null http://localhost:9000
do
    echo "Waiting for server to start..."
    sleep 1
done
```

## Check if Something Exists

### Check if file or directory exists

```bash
if [ -f "/some/file.txt" ]; then
  echo "Python 3 installed"
fi

# check if directory exists
if [ -d "/some/directory" ]; then
  echo "Directory exists"
fi
```

### Check if command exists

```bash
if command -v python3 &> /dev/null; then
  echo "Python 3 installed"
else
  echo "Python 3 is NOT installed"
fi
```

## Generate Random Hash

```bash
random_suffix=$(echo $RANDOM | md5sum | head -c 10; echo;)
echo $RANDOM_HASH
↪ 38ac82e4fa
```

## Alias

Create a shorthand alias to call another command. Save

```bash
echo "alias some_command='bash /path/to/script.sh'" >> ~/.bashrc
```

- The above creates a command called `some_command` that will invoke `bash /path/to/script.sh`
- It writes it to the `~/.bashrc` file so it's available every time the terminal session is started

### Save and alias command script

This script:

- Save a multiline command to a file, specifically a bash script
- Use alias to create the command `some_command` that invokes that the bash script
- Saves it to the `~/.bashrc` file so the command is available every time a new shell session starts

```bash
cat << 'EOF' > /path/to/script.sh
echo "command running..."
ls
EOF
echo "alias some_command='bash /path/to/script.sh'" >> ~/.bashrc
```

## JSON Template

```bash
JSON_TEMPLATE=$(cat <<EOF
    {
    "description":"",
    "parent_ref":"${VENDOR}/${PACKAGE}",
  "uid":"%s",
    "provider":{
        "type":"github",
        "username":"",
        "password":"${PASSWORD}",
        "host":""
        },
  "provider_repo":"%s",
    "pipelines":"ignore"
    }
EOF
)
```

## Save Variable to File

ADDED

```bash
# set variable `TOKEN` to file `.token`
echo "$TOKEN" > .token
# optionally unset token after saving it
unset TOKEN
```

## Using JQ

```json
{
  "someKey": "someValue",
  "anotherKey": "anotherValue"
}
```

```bash
$ jq -r '.someKey' data.json
↪ someValue
```

## Get directory where a script is running

```bash
THIS=`which $0`
PREFIX=`dirname $THIS`
echo "$PREFIX"
```

```bash
/bin/bash
bash-3.2$ THIS=`which $0`
bash-3.2$ PREFIX=`dirname $THIS`
bash-3.2$ echo "$PREFIX"
↪ /bin
```

## Silent with exit status

```bash
if curl --silent --fail https://example.com; then
    echo "Request succeeded."
else
    echo "Request failed."
fi
```
