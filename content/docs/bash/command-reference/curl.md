---
title: "curl"
description: "Use cURL to make HTTP and FTP requests."
summary: ""
date: 2025-01-16T01:08:23-08:00
lastmod: 2025-01-16T01:08:23-08:00
weight: 65
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

{{< callout context="tip" title="Command: curl - Client URL" icon="outline/terminal-2" >}}
Transfer a URL with `curl`. Make requests via HTTP, FTP, ect.
{{< /callout >}}

Transfer an URL.

```bash
curl https://example.com

# get file using FTP
curl ftp://ftp.example.com/README
```

## Silence output

Silence the stdout terminal output of curl.

```bash { title="Silence output" }
# silence output
curl https://example.com --silent
curl https://example.com -s

# save file without any CLI output
curl https://example.com -s -o file.txt

# silent and verbose (show detailed req and res data)
curl --silent --verbose https://example.com
```

## Follow Redirects

Use `-L` to follow redirects if the first response is a 3xx.

Curl does not follow redirects by default.

```bash { title="Follow redirects" }
# follow a location (instead of returning redirect)
curl --location https://example.com

# shorthand
curl -L https://example.com
```

## Download file with cURL

Download a file using cURL. Specify a file name with `-o` or use `-O` to keep the default file name.

```bash { title="Download a file with cURL, specify new name" }
# download file and set name to file_name
curl https://example.com --output file_name
curl https://example.com -o file_name
```

```bash { title="Download a file with cURL, keep existing name" }
# download file and use same file name
curl https://example.com -O
```

## Set HTTP headers with curl

```bash { title="Specify HTTP method" }
# specify a HTTP header
curl https://example.com -H 'Content-Type: application/json'

# use parameter in HTTP header
curl https://example.com -H 'Authorization: Bearer '$TOKEN''

# specify multiple HTTP headers
curl https://example.com -H 'Content-Type: application/json' \
  -H 'api-key: '$API_KEY''
```

## Set HTTP method with curl

By default, cURL uses a `GET` request. Use the `-X` flag to specify a different method.

```bash { title="Specify HTTP method" }
# specify a POST HTTP method
curl --request POST https://example.com

# specify a POST HTTP method (shorthand)
curl -X POST https://example.com
```

### HTTP POST with payload

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

## Save response to variable

Set HTTP POST response body to variable, using parameterized data payload and values.

```bash
# curl POST with parameters
API_RESPONSE=$(curl "https://example.com/api/user/1")

# access response object key of "id", set to variable
TEAM_ID=$(echo $API_RESPONSE | jq -r '.id')
```

### cURL with Parameters

```bash
RESPONSE=$(curl -s "https://example.com/api/users/$USER_ID?account="$ACCOUNT"&realm="$REALM"" \
  -H 'x-api-key: '$TOKEN'' \
  -H 'accept: */*' \
  -H 'content-type: application/json' \
  --data-raw '{"data":{"id":"'$USER_ID'","email":"'$EMAIL'","tags":{"key": "value"}}}' \
  --compressed)

STATUS=$(echo $RESPONSE | jq -r ".status")

if [ "$STATUS" = "success" ]; then echo "RES OK"; fi
```

Variables:

- URL path in single quotes (`""`)
- URL path variables do not need quotes
- URL parameter variables need quotes (`?id="$ID"`)
- URL headers variables use single quotes (`-H 'x-api-key: '$TOKEN''`)
- Data payload variables use single quotes (`"id":"'$ID'"`)

### cURL with URL encoded parameters.

```bash
RESPONSE=$(curl -s --location "http://example.com" \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'user_name=gregbenish' \
--data-urlencode 'password='$PASSWORD'')
```

## Save response code to variable

Save HTTP response code to a variable.

Can check to see if the response was OK, e.g., 200-299, or a specific response code to handle errors differently.

```bash
# curl POST with parameters
RES_CODE=$(curl --write-out %{http_code} -s --location "https://example.com")

if [[ "$RES_CODE" -ne 201 ]] ; then
  echo "Failed to create resource. Exiting..." && exit 1
fi

if [[ "$RES_CODE" -ge 200 && "$RES_CODE" -lt 300 ]]; then
  echo "Got 2xx response code"
fi
```

## See redirect path

Use the `-sILK` flags to see the redirect path of a URL using cURL.

```bash
curl -sILk google.com
```

## Certificates

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

## Authentication

Various types of authentication.

### Username and password authentication

```bash
# username/password authentication
curl http://user:password@example.com/
# OR
curl -u user:password http://example.com/
curl -u "user:password" http://example.com/
```

### Basic type authentication

```bash
# username/password authentication
curl curl -i http://example.com \
  -H "Authorization: Basic Aa123Aa123Aa123="
```

### Bearer token authentication

```bash
# username/password authentication
curl curl -i http://example.com \
  -H "Authorization: Bearer $BEARER_TOKEN"
```

## Fail fast

Use the `--fail` flag to fail fast and return no output on server outputs.

Useful for scripts to better handle errors.

```bash
# Certificates
curl --fail https://example.com
```

## Wait for HTTP endpoint before continuing

```bash
while ! curl --silent --fail --output /dev/null http://localhost:9000
do
    echo "Waiting for server to start..."
    sleep 1
done
```

## cURL as a Function

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
