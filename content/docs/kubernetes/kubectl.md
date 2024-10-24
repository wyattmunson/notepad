---
title: "Kubectl CLI"
description: "Kubectl is a command line interface to interact with a Kubernetes cluster."
summary: "Kubectl is a command line interface to interact with a Kubernetes cluster."
lead: ""
date: 2023-02-13T19:10:57-08:00
lastmod: 2023-02-13T19:10:57-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "kubectl-552b78dc03109125d11cdf8e09e2f0f9"
weight: 999
toc: true
---

`kubectl` is a command line interface tool that interacts with the `kube-apiserver`/control plane.

API resources can generally use with `get`, `describe`, `delete`. See the full list of API resources with `kubectl api-resources`.

## Creating Resrouces

Use the `kubectl apply` command to create or update a resource. The `-f` flag specifies a file or directory name.

```bash
# Specify file or directory
$ kubectl apply -f ./deploy-dev.yaml        # create resrouce from file
$ kubectl apply -f ./dev.yaml ./prod.yaml   # from two files
$ kubectl apply -f ./dir                    # from an entire directory

# Apply YAML from stdin
cat deploy.json | kubectl apply -f -
```

## Viewing, Updating, and Deleting Resrouces

Kuberentes API resoruces can be generally be used with `get`, `describe`, `edit`, and `delete`.

{{< callout icon="👉" >}}

<p>See full list of API Resources with <code>kubectl api-resources</code>.</p>
{{< /callout >}}

### Viewing Resources

View resources with the `kubectl get` command.

```bash
# get services in default namespace
$ kubectl get pods

# get services in dev namespace
$ kubectl get pods -n dev

# get pods all namespaces
$ kubectl get pods -A

# list pods with more details
$ kubectl get pods -o wide

# get deployment dev-dep
$ kubectl get pods customer-service-ae892934-482da2f

# get pod's YAML
$ kubectl get pod ui-pod -o yaml

#list pod without cluster specific information
$ kubectl get  --export

# get all k8s resoruces in given namespace
kubectl get all -n NAMESPACE

# get all k8s resources in all namespaces
kubectl get all -A

# Can get, describe, and delete resources
$ k get pods -n tick
$ k desribe pods/influx-influxdb-j3ilso93-ae82kdl2 -n tick
$ k delete pods/influx-influxdb-j3ilso93-ae82kdl2 -n tick
```

### Describing, Editing, and Deleting

```bash
# delete deployment
kubectl delete deployments applications-service -n applications-dev

# shorthand
kubectl describe po/payment-service-3829aec9-cea2831af -n dev
kubectl describe svc/payment-service -n dev
kubectl describe deploy/payment-service -n dev
```

## Get logs

{{< callout icon="👉" text="See logs associated with a container" />}}

Logs can be grabbed from a pod or a service.

One advantage of service level logs is faster debugging: the service name is predictable while the pod name is randomly generated and there's an extra step to get the pod name. The service level logs will show logs from all pods it handles, which may or may not be desirable.

```bash
# get logs associated with a pod
kubectl logs CONTINER-NAME -n NAMESPACE
kubectl logs payment-service-39a03f2-ac624fe -n dev

# get logs associated with other services
kubectl logs RESOURCE/RESOURCE-NAME -n NAMESPACE
kubectl logs svc/payment-service -n dev
kubectl logs jobs/payment-service -n dev
kubectl logs deploy/payment-service -n dev

# tail/follow logs
kubectl logs svc/payment-service -n dev --follow
kubectl logs svc/payment-service -n dev -f

# limit logs based on time
kubectl logs svc/payment-service -n dev --since 10m
```

## Exec into running container

Exec into a running container to get shell access or run a command.

```bash
kubectl exec po/pod-name -n namespace-name -- bash
```

## Port forwarding

Use port forwarding to map a port on your local machine to a running kubernetes pod or service.

This is useful when an engineer is working on one service and needs to interact with another service for development or testing.

{{< callout icon="👉" text="Port fowarding maps a local port to a kubernetes pod" />}}

```bash
$ k port-forward services/applications-service -n dev 8080:80
```

## Namespaces

Namespaces are a way to segment and organize resources running in a cluster.

{{< callout icon="👉" text="Deleting a namespace destroys all resources within it." />}}

```bash
# create a namespace
kubectl create namespace finance-qa

# delete a namespace
kubectl delete namepsaces finance-qa # note extra "s"

# use "ns" shorthand for namespaces
kubectl delete ns finance-qa
```

## Add a user to EKS Config Map

Give an IAM user access to an EKS cluster by editing its Config Map.

```bash
kubectl edit -n kube-system configmap/aws-auth
```

## Add EKS cluster to your local kubeconfig

Add an AWS EKS to a machine's kubeconfig using the AWS CLI.

```bash
# add EKS cluster to local kubeconfig
aws eks --region us-east-1 update-kubeconfig --name cluster-name-in-eks-ce548264
```

### Unorganized commands

```bash
# Current meximum pods per node
kubectl get nodes -o yaml | grep pods

# Current pods per node
$ kubectl get pods --all-namespaces | grep Running | wc -l
```

```bash
# Get cluster info
$ kubectl cluster-info

$ kubectl component-status

# shorthand commands
$ k get services
$ k get svc
$ k get pods
$ k get po

# exec into container in pod
k exec --stdin --tty employer-lookup-service-78cb8c8c7d-hhltf -- sh

# see pod limit per node
$ kubectl get nodes -o yaml | grep pods
# see total running pods
$ kubectl get pods --all-namespaces | grep Running | wc -l



# Check node CPU and memory usage
kubectl get nodes --no-headers | awk '{print $1}' | xargs -I {} sh -c 'echo {}; kubectl describe node {} | grep Allocated -A 5 | grep -ve Event -ve Allocated -ve percent -ve -- ; echo'
```

[kubectl short hand](https://blog.heptio.com/kubectl-resource-short-names-heptioprotip-c8eff9fb7202)

```bash
$ k get all
k get pod/applications-service-6ffbc48d6d-bpchh
$ k get service/applications-ui
$ k get deployment.apps/token-service
$ k get replicaset.apps/applications-service-558c6b579d

```

### Generate admin token for dashboard

```bash
#!/bin/bash
EKS_ADMIN_TOKEN_KEY=$(kubectl -n kube-system get secret | grep eks-admin | awk '{print $1}')
kubectl -n kube-system describe secret ${EKS_ADMIN_TOKEN_KEY} | grep 'token:'
```
