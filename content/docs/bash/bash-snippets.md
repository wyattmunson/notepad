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

For example, and async process can create a file when complete. The parent script can watch a file location, and only continue when that file is created.

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

```bash { title="Check if file exists" }
if [ -f "/some/file.txt" ]; then
  echo "Python 3 installed"
fi
```

```bash { title="Check if directory exists" }
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

## AWS

### Extract AWS credentials

```bash
echo -e "===> EXISTING IDENTITY: "
aws sts get-caller-identity
aws configure list

echo -e "===> Assuming the role..."
aws sts assume-role --role-arn arn:aws:iam::123456789:role/roleName --role-session-name role-session-${UNIQUE_ID} >> credentials.json
export AWS_SECRET_ACCESS_KEY=$(cat credentials.json | grep "SecretAccessKey" | cut -d ':' -f2 | cut -d '"' -f2)
export AWS_SESSION_TOKEN=$(cat credentials.json | grep "SessionToken" | cut -d ':' -f2 | cut -d '"' -f2)
export AWS_ACCESS_KEY_ID=$(cat credentials.json | grep "AccessKeyId" | cut -d ':' -f2 | cut -d '"' -f2)

echo -e "===> ASSUMED IDENTITY:"
aws sts get-caller-identity
aws configure list
```

## Helm

```bash { title="" }
#!/bin/bash
# Script example:
# ./helm-deploy.sh ./lle-values.yaml dev 90s
# Arguments:
# $1 - location of custom values file relative to HELM_DIR (i.e. ./lle-values.yaml)
# $2 - namespace to deploy to
# $3 - timeout length - use 90s for most cases but 600s if you're using an NLB

## debug values:
#export HELM_DIR="./sample-web-service-application-go/helm/"
#export CHART_NAME="sample-web-service-application-go"
#export VERSION="1.0"
#export HELM_REPO="https://nexus.internal.westcreekfin.com/repository/helm-hosted/"

values_file=${values_file:-values.yaml}
namespace=${namespace:-default}
timeout=${timeout:-90s}
helm_dir=${helm_dir:-${HELM_DIR}}
chart_name=${chart_name:-${CHART_NAME}}
version=${version:-${VERSION}}

while [ $# -gt 0 ]; do
    if [[ $1 == *"--"* ]]; then
        v="${1/--/}"
        declare $v="$2"
        echo $1 $2
    fi

    shift
done

cd $helm_dir
export BASE_CHART=$(grep -A2 'name: acme-core-api' $chart_name/Chart.yaml | tail -n1 | awk '{ print $2 }')
bash chart-pull.sh $HELM_REPO "acme-core-api-${BASE_CHART}.tgz"

sed -r "s/appVersion: bleeding-edge/appVersion: $version/" -i $chart_name/Chart.yaml #todo change to sed after testing

if [[ $(helm list -n ${namespace} | awk -v chart="${chart_name}" '$1 == chart' | awk  '{print $8}') == failed ]]; then
echo "Deleting previous failed chart release before deployment"
helm del ${chart_name} -n ${namespace}
fi

echo "Deploying chart"
helm upgrade --install --force ${chart_name} -n ${namespace} --wait --timeout ${timeout} -f ${values_file} ./${chart_name} || kubectl describe pod -l release=${chart_name} -n ${namespace} && kubectl logs -l release=${chart_name} -n ${namespace}
```

```bash { title="" }
echo '|\/\/|'
echo "\o.O |"
echo "(____)"

    echo "___/|"
    echo "\o.O|"
    echo "(___)"

```

## Deploy Helm v2

```bash { title="" }
#!/bin/bash

echo -e "\nHelm deployment starting..."

# Check env vars are set, exit if not
if [[ -z $NAMESPACE  ]]; then echo -e "\nERROR: NAMESPACE not exported. Exiting.\nSet to k8s namespace." && exit 1; fi
if [[ -z $VALUES_FILE  ]]; then echo -e "\nERROR: VALUES_FILE not exported. Exiting.\nSet to environment specific values file like infra/helm." && exit 1; fi
if [[ -z $VERSION  ]]; then echo -e "\nERROR: VERSION not exported. Exiting. \nSet to version number of your application." && exit 1; fi

export HELM_REPO=https://nexus.internal.example.com/repository/helm-hosted/
export CHART_NAME=$CI_PROJECT_NAME
export HELM_DIR=infra/helm
cd $HELM_DIR

# Get base chart version number from Chart.yaml
export BASE_CHART=$(grep -A2 'name: acme-core-api' ${CHART_NAME}/Chart.yaml | tail -n1 | awk '{ print $2 }')
if [[ -z $BASE_CHART  ]]; then echo -e "\nERROR: BASE_CHART not set. Exiting. \nSet acme-core-api version in Chart.yaml." && exit 1; fi

# Get core acme API, use local chart-pull script file
bash /opt/deploy-scripts/chart-pull.sh $HELM_REPO "acme-core-api-${BASE_CHART}.tgz"

# Overwrite bleeding-edge with version number
sed -r "s/appVersion: bleeding-edge/appVersion: \"$VERSION\"/" -i $CHART_NAME/Chart.yaml

# Get chart deployment status
export DEPLOYMENT_STATUS=$(helm status -n ${NAMESPACE} ${CHART_NAME} -o json | jq -r '.info.status')
echo -e "\n Deployment Status: $DEPLOYMENT_STATUS"

# Check for failed installs
if [[ $DEPLOYMENT_STATUS == 'failed' ]]; then
    echo -e "\nFailed installation detected, removing..."
    helm delete ${CHART_NAME} -n ${NAMESPACE}
# Upgrade when previous deployment suceeded
elif [[ $DEPLOYMENT_STATUS == 'deployed' ]]; then
    echo -e "\nPrevious installation was successful, upgrading..."

    helm upgrade ${CHART_NAME} ./${CHART_NAME} -n ${NAMESPACE} -f ${CHART_NAME}/${VALUES_FILE}

    # Verify release successful
    echo -e "\nHelm completed. Querying cluster to verify release was successful"
    kubectl rollout status deployment/${CHART_NAME} -n ${NAMESPACE}

    # Call listening post
    bash /opt/deploy-scripts/deployBeacon.sh $VERSION $CI_ENVIRONMENT_NAME $NAMESPACE k8s

    echo "___/|"
    echo "\o.O|"
    echo "(___)"

    exit 0
fi

# Install fresh chart when no prior version installed
echo -e "\nNo installation detected, installing..."
helm install ${CHART_NAME} -n ${NAMESPACE} -f ${CHART_NAME}/${VALUES_FILE} ./${CHART_NAME}

# Verify release successful
echo -e "\nHelm completed. Querying cluster to verify release was successful..."
kubectl rollout status deployment/${CHART_NAME} -n ${NAMESPACE}

# Call listening post
bash /opt/deploy-scripts/deployBeacon.sh $VERSION $CI_ENVIRONMENT_NAME $NAMESPACE k8s

echo "___/|"
echo "\o.O|"
echo "(___)"
```

## Example

```bash { title="" }

```
