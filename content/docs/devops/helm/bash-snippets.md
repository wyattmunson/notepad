---
title: "Bash Snippets"
description: "Bash snippets for using Helm."
summary: ""
date: 2025-01-16T04:34:11-08:00
lastmod: 2025-01-16T04:34:11-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

```bash
#Harbor registry endpoint setup
CURL_OPTS="-i"
MY_API_HARBOR_REGISTRY_ENDPOINT_CREATE="https://${MY_CONTAINER_REGISTRY_LINK}/api/v2.0/registries"
MY_API_RESPONSE=$(curl "${MY_API_HARBOR_REGISTRY_ENDPOINT_CREATE}" ${CURL_OPTS} --basic -u "${MY_CONTAINER_REGISTRY_USER}:${MY_CONTAINER_REGISTRY_PASSWORD}" -X POST \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
--data @<(cat <<EOF
{
			  "id": 0,
			  "url": "https://public.ecr.aws",
			  "name": "my-aws-ecr",
			  "credential": {
			    "type": "basic",
			    "access_key": "",
			    "access_secret": ""
			  },
			  "type": "docker-registry",
			  "insecure": false,
			  "description": "my-desc",
			  "status": "",
			  "creation_time": "",
			  "update_time": ""
}
EOF
)
)
echo "DEBUG: ${MY_API_RESPONSE}"
# location: /api/v2.0/registries/<ID>
MY_CONTAINER_REGISTRY_ENDPOINT_ID=$(echo "${MY_API_RESPONSE}" | grep location | grep -o "[^/]*$")

# Define JSON template
REPLICATION_RULE_TEMPLATE='{
  "name": "%s",
    "type": "docker-registry",
    "url": "https://public.ecr.aws",
  "description": "%s",
    "enabled": true,
    "src_registry": {
    "id": '"${MY_CONTAINER_REGISTRY_ENDPOINT_ID}"'
    },
    "dest_namespace": "library",
  "filters": [
    {
    "type": "name",
      "value": "%s"
    },
    {
    "type": "tag",
      "value": "%s"
    }
  ],
    "trigger": {
    "type": "manual"
    }
}'

# Define function
setup_replication_rule2() {
  local rule_json="$1"
  echo "DEBUG: ${rule_json}"
  local api_response=$(curl "${MY_CONTAINER_REGISTRY_LINK}/api/v2.0/replication/policies" ${CURL_OPTS} --basic -u "${MY_CONTAINER_REGISTRY_USER}:${MY_CONTAINER_REGISTRY_PASSWORD}" -X POST \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
    --data "${rule_json}")
  echo "DEBUG: ${api_response}"
  local policy_id=$(echo "${api_response}" | grep Location | cut -d' ' -f2 | cut -d'/' -f6)
  echo "Policy ID: ${policy_id}" # Debug output
  # Trigger replication directly
  curl "${MY_CONTAINER_REGISTRY_LINK}/api/v2.0/replication/executions" ${CURL_OPTS} --basic -u "${MY_CONTAINER_REGISTRY_USER}:${MY_CONTAINER_REGISTRY_PASSWORD}" -X POST \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
    --data "{\"policy_id\":${policy_id}}"
}

# Generate specific replication rules
REPLICATION_RULE_MYFRONTENDSERVICE=$(printf "${REPLICATION_RULE_TEMPLATE}" "my-frontend-service" "Replication Rule for my-frontend-service" "l7w9l6a8/my-frontend-service" "v*")

setup_replication_rule2 "${REPLICATION_RULE_MYFRONTENDSERVICE}"
```
