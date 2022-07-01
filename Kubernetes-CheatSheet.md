
# Kubernetes (k8s) and (EKS/ECS) Cheat Sheet


**Kubernetes Downloads / Installs:**

https://kubernetes.io/docs/tasks/tools  
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux  
https://kubernetes.io/docs/tasks/tools/install-kubectl-windows  


Kubectx/Kubens:  
https://github.com/ahmetb/kubectx  
Download latest releases:  
https://github.com/ahmetb/kubectx/releases  

To change contexts:  

```bash
kubectx ARN-of-Context
```

Example:  

```bash
kubectx arn:aws:eks:us-west-2:123456789012:cluster/dev-abcd123
Switched to context "arn:aws:eks:us-west-2:123456789012:cluster/dev-abcd123".
```

AWS CLI:  
https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html  

aws-iam-authenticator:  
https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html  
Install - Linux:  
curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/aws-iam-authenticator  
Install - Windows:  
curl -o aws-iam-authenticator.exe https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/windows/amd64/aws-iam-authenticator.exe  


kubeadm:  

Install on Ubuntu Linux:  

```bash
# Update the apt package index and install packages needed to use the Kubernetes apt repository:
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl

# Download the Google Cloud public signing key:
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

# Add the Kubernetes apt repository:
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubeadm
# sudo apt-mark hold kubeadm
```

*Ref:*  
https://kubernetes.io/docs/reference/setup-tools/kubeadm  
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm  


## Get version of kubectl  

```bash
kubectl version
Client Version: version.Info{Major:"1", Minor:"22", GitVersion:"v1.22.3", GitCommit:"c92036820499fedefec0f847e2054d824aea6cd1", GitTreeState:"clean", BuildDate:"2021-10-27T18:41:28Z", GoVersion:"go1.16.9", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"21+", GitVersion:"v1.21.2-eks-0389ca3", GitCommit:"8a4e27b9d88142bbdd21b997b532eb6d493df6d2", GitTreeState:"clean", BuildDate:"2021-07-31T01:34:46Z", GoVersion:"go1.16.5", Compiler:"gc", Platform:"linux/amd64"}
# Or
kubectl version --short
Client Version: v1.22.3
Server Version: v1.21.2-eks-0389ca3
```

## Kubectl autocomplete

**BASH**  

```bash
# setup autocomplete in bash into the current shell, bash-completion package should be installed first.
source [(kubectl completion bash)

# add autocomplete permanently to your bash shell.
echo "source [(kubectl completion bash)" ]] ~/.bashrc
```

*You can also use a shorthand alias for `kubectl` that also works with completion:*  

```bash
alias k=kubectl
complete -F __start_kubectl k
```

**ZSH**  

```bash
# setup autocomplete in zsh into the current shell
source [(kubectl completion zsh)

# add autocomplete permanently to your zsh shell
echo "[[ $commands[kubectl] ]] && source [(kubectl completion zsh)" ]] ~/.zshrc
```

## Kubectl context and configuration  

Set which Kubernetes cluster `kubectl` communicates with and modifies configuration information. See [Authenticating Across Clusters with kubeconfig](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/) documentation for detailed config file information.  


Setting up the Config to a specific config file:  

```bash
# Examples:
export KUBECONFIG=$KUBECONFIG:~/.kube/config
export KUBECONFIG=$KUBECONFIG:~/.kube/config-dev
export KUBECONFIG=$KUBECONFIG:~/.kube/config-qa
export KUBECONFIG=$KUBECONFIG:~/.kube/config-stg
export KUBECONFIG=$KUBECONFIG:~/.kube/config-staging
export KUBECONFIG=$KUBECONFIG:~/.kube/config-prod
```

## Journalctl  

```bash
# The journalctl command returns events for the kubelet service for the node:
journalctl -u kubelet -r (on the relevant node)

```

## Kubectl command syntax  
`kubectl [command] [TYPE] [NAME] [flags]`  


- command : specifies the operation that you want to perform on one or more resources (create, get, describe, delete)
- type    : specifies the resource type. Resource types are case-insensitive and you can specify the singular, plural, or abbreviated forms
- name    : specifies the name of the resource. Names are case-sensitive. If the name is omitted, details for all resources are displayed
- flags   : specifies optional flags.


## Kubectl commands  

```bash
# Check that your Kubernetes cluster is running
kubectl cluster-info

# Get more detailed information about your Kubernetes cluster
kubectl cluster-info dump

# Get the Health Status of Cluster Components
kubectl get componentstatus

# Show Merged kubeconfig settings.
kubectl config view 

# use multiple kubeconfig files at the same time and view merged config
KUBECONFIG=~/.kube/config:~/.kube/kubconfig2 

kubectl config view

# get the password for the e2e user
kubectl config view -o jsonpath='{.users[?(@.name == "e2e")].user.password}'

# display the first user
kubectl config view -o jsonpath='{.users[].name}'

# get a list of users
kubectl config view -o jsonpath='{.users[*].name}'

# display list of contexts
kubectl config get-contexts

# display the current-context
kubectl config current-context

# set the default context to my-cluster-name
# Note for AWS EKS use Cluster ARN in place of "my-cluser-name"
kubectl config use-context my-cluster-name


# add a new user to your kubeconf that supports basic auth
kubectl config set-credentials kubeuser/foo.kubernetes.com --username=kubeuser --password=kubepassword

# permanently save the namespace for all subsequent kubectl commands in that context.
kubectl config set-context --current --namespace=ggckad-s2

# set a context utilizing a specific username and namespace.
kubectl config set-context gce --user=cluster-admin --namespace=foo \
  && kubectl config use-context gce

# delete user foo
kubectl config unset users.foo
```

## Kubectl apply  

`apply` manages applications through files defining Kubernetes resources. It creates and updates resources in a cluster through running `kubectl apply`. 
This is the recommended way of managing Kubernetes applications on production. See [Kubectl Book](https://kubectl.docs.kubernetes.io/).

## Creating objects  

Kubernetes manifests can be defined in YAML or JSON. The file extension `.yaml`, `.yml`, and `.json` can be used.

```bash
# create resource(s)
kubectl apply -f ./my-manifest.yaml

# create from multiple files
kubectl apply -f ./my1.yaml -f ./my2.yaml

# create resource(s) in all manifest files in dir
kubectl apply -f ./dir

# create resource(s) from url
kubectl apply -f https://git.io/vPieo

# start a single instance of nginx
kubectl create deployment nginx --image=nginx

# create a Job which prints "Hello World"
kubectl create job hello --image=busybox -- echo "Hello World" 

# List cron jobs in a namespace
kubectl get cronjobs
Or
kubectl get cronjobs --namespace my-namespace

#List the jobs on the cron
kubectl get job

# Find the related pod for a job by using the below command.
# It will show the number of times the job has been restarted, full config of the job and the pod created.
kubectl describe job name-of-job

# This will show similar information as above for the cronjob
kubectl describe cronjob my-cronjob

# To edit a cron job
kubectl edit cronjob my-cronjob

# To delete a job in cron
kubectl delete job job-id

# Delete kubernetes cron job
kubectl delete cronjob my-cronjob

# Create a CronJob that prints "Hello World" every minute
kubectl create cronjob hello --image=busybox   --schedule="*/1 * * * *" -- echo "Hello World"    

# Create a job
kubectl create job --from=cronjob/[cronjob-name] job-name

# Create a duplicat of cronjob
kubectl get cronjob [cronjob-name] -o json ] /tmp/[cronjob-name].json && sed -i 's/[cronjob-name]/[cronjob-name]-duplicate/' /tmp/[cronjob-name].json && kubectl create -f /tmp/[cronjob-name].json

# get the documentation for pod manifests
kubectl explain pods

# Create multiple YAML objects from stdin
cat [[EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "1000000"
---
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep-less
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "1000"
EOF

# Create a secret with several keys
cat [[EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  password: $(echo -n "s33msi4" | base64 -w0)
  username: $(echo -n "jane" | base64 -w0)
EOF
```

## Viewing, finding resources  

```bash
# Get commands with basic output

# Get a list of all API services running
kubectl get apiservices

# See if a sepeific API service is running
kubectl get apiservices | grep v1beta1.apiextensions.k8s.io

# Get all namespaces
kubectl get ns

# Get a specific namespace
kubectl get ns [MYNAMSPACE]

# Get a specific namespace in JSON
kubectl get ns [MYNAMSPACE] -o json

# Fetch information about everything in current namespace
kubectl get all

# Fetch information about everything in a specific namespace
kubectl get all -n [MYNAMSPACE]

# Fetch information about everything in a specific namespace in JSON
kubectl get all -n [MYNAMSPACE] -o json

# Fetch information about everything in all namespaces
kubectl get all -A

# List a service in the namespace
kubectl get service

# List all services in the namespace
kubectl get services

# View detail information about a pod in YAML format in a specific namespace
kubectl get pod my-pod --output=yaml --namespace=my-namespace

# List all pods in all namespaces
kubectl get pods --all-namespaces

# List all pods in the current namespace
kubectl get pods

# List pods in the current namespace with output in json
kubectl get pods --output json

# List pods in the current namespace with output in json and pipe the output to jq (the json parser)
kubectl get pods --output json | jq

# List pods by a node name:
kubectl get pods -o wide --all-namespaces | grep [YOUR-NODE]

# List/Sort pods by node name:
kubectl get pods -o wide --sort-by="{.spec.nodeName}"

# List/Sort pods by number of restarts:
kubectl get pods --sort-by="{.status.containerStatuses[:1].restartCount}"

# Watch the pods in the namespace and wait for the pod's status to change to Running.
kubectl get pods --watch

# List all pods in Evicted Status
kubectl get pods | grep Evicted

# List all pods in CreateContainerError Status
kubectl get pods | grep CreateContainerError

# List all pods in the current namespace, with more details
kubectl get pods -o wide

# List all pods with columns
kubectl get pods -o=custom-columns=NameSpace:.metadata.namespace,POD_NAME:.metadata.name,POD_STATUS:.status.phase
kubectl get pods -o=custom-columns=NameSpace:.metadata.namespace,POD_NAME:.metadata.name,POD_STATUS:.status.phase,NODE:.spec.nodeName
kubectl get pods -o=custom-columns=NameSpace:.metadata.namespace,POD_NAME:.metadata.name,POD_STATUS:.status.phase,NODE:.spec.nodeName --sort-by=.spec.nodeName
kubectl get pods -o=custom-columns=NAME:.metadata.name,STATUS:.status.phase,NODE:.spec.nodeName

# Get list of secrets
kubectl get secret
kubectl get secrets

# Describe secret (will show the secret)
kubectl describe secret [SECRET]

# List deployments
kubectl get deployments
-or-
kubectl get deployments [NAME]

# List deployments in a namespace
kubectl get deployments -n kube-system

# Show Deployment Image
kubectl get deployments -o wide

# Get deployment details
kubectl describe deployments
-or-
kubectl describe deployment [NAME]

# Get the Deployment details in YAML format:
kubectl get deployments -o yaml
- or -
kubectl get deployments [NAME] -o yaml

# Get Deployment details on a particualr app
kubectl get deployments.apps [YOUR-APP-NAME]


# List a particular deployment
kubectl get deployment my-dep

# Determine the DNS provider for your cluster
kubectl get deployments -l k8s-app=kube-dns -n kube-system

# List all pods in the namespace
kubectl get pods

# Get a pod's YAML
kubectl get pod my-pod -o yaml

# Describe commands with verbose output

# Describe Nodes
kubectl describe nodes [node]

# Look for Disk Pressure on Nodes:
kubectl describe nodes 2]&1 | grep -i Disk

# Describe a single Node:
kubectl describe node [node]

# Describe a Node and show Taints:
kubectl describe node [node] | grep Taints

# Describe a Pod
kubectl describe pods my-pod
kubectl describe pod my-pod

# List Services Sorted by Name
kubectl get services --sort-by=.metadata.name

# List pods Sorted by Restart Count
kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'

# List PersistentVolumes sorted by capacity
kubectl get pv --sort-by=.spec.capacity.storage

# Get the version label of all pods with label app=cassandra
kubectl get pods --selector=app=cassandra -o jsonpath='{.items[*].metadata.labels.version}'

# Get all worker nodes (use a selector to exclude results that have a label
# named 'node-role.kubernetes.io/master')
kubectl rd --selector='!node-role.kubernetes.io/master'

# Get all running pods in the namespace
kubectl get pods --field-selector=status.phase=Running

# Get Status of your Nodes (for a Context)
kubectl get nodes

# List Tait of Nodes (using jq):
kubectl get nodes -o json | jq '.items[].spec.taints'
Example Output:
[
  {
    "effect": "NoSchedule",
    "key": "node.kubernetes.io/disk-pressure",
    "timeAdded": "2022-05-28T01:20:44Z"
  }
]

# Get External IPs of all nodes
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'

# List Names of Pods that belong to Particular RC
# "jq" command useful for transformations that are too complex for jsonpath, it can be found at https://stedolan.github.io/jq/
sel=${$(kubectl get rc my-rc --output=json | jq -j '.spec.selector | to_entries | .[] | "\(.key)=\(.value),"')%?}
echo $(kubectl get pods --selector=$sel --output=jsonpath={.items..metadata.name})

# Show labels for all pods (or any other Kubernetes object that supports labelling)
kubectl get pods --show-labels

# Check which nodes are ready
JSONPATH='{range .items[*]}{@.metadata.name}:{range @.status.conditions[*]}{@.type}={@.status};{end}{end}' \
 && kubectl get nodes -o jsonpath="$JSONPATH" | grep "Ready=True"

# Output decoded secrets without external tools
kubectl get secret my-secret -o go-template='{{range $k,$v := .data}}{{"### "}}{{$k}}{{"\n"}}{{$v|base64decode}}{{"\n\n"}}{{end}}'

# List all Secrets currently in use by a pod (using jq)
kubectl get pods -o json | jq '.items[].spec.containers[].env[]?.valueFrom.secretKeyRef.name' | grep -v null | sort | uniq

# List all containerIDs of initContainer of all pods
# Helpful when cleaning up stopped containers, while avoiding removal of initContainers.
kubectl get pods --all-namespaces -o jsonpath='{range .items[*].status.initContainerStatuses[*]}{.containerID}{"\n"}{end}' | cut -d/ -f3

# Init Container:
kubectl get pods POD_NAME -o jsonpath={.spec.initContainers[*].name}

kubectl get pods POD_NAME -n NAMESPACE -o jsonpath='{.spec.containers[*].name}*'
kubectl get pods -n NAMESPACE -o jsonpath='{.spec.containers[*].name}*'

# Get Activities in Your Cluster
kubectl get events 

# List Events sorted by timestamp
kubectl get events --sort-by=.metadata.creationTimestamp

# Compares the current state of the cluster against the state that the cluster would be in if the manifest was applied.
kubectl diff -f ./my-manifest.yaml

# Produce a period-delimited tree of all keys returned for nodes
# Helpful when locating a key within a complex nested JSON structure
kubectl get nodes -o json | jq -c 'path(..)|[.[]|tostring]|join(".")'

# Produce a period-delimited tree of all keys returned for pods, etc
kubectl get pods -o json | jq -c 'path(..)|[.[]|tostring]|join(".")'

# Produce ENV for all pods, assuming you have a default container for the pods, default namespace and the `env` command is supported.
# Helpful when running any supported command across all pods, not just `env`
for pod in $(kubectl get po --output=jsonpath={.items..metadata.name}); do echo $pod && kubectl exec -it $pod env; done

# List all service accounts in all namesapces
kubectl get serviceaccount --all-namespaces

# List a specific service account
kubectl -n default get serviceaccount myserviceaccount

# List ClusterRole, ClusterRoleBindings, RoleBindings
kubectl -n default get clusterrole,clusterrolebindings,rolebindings

# Get ClusterNode Storage Classe:
kubectl get sc


# Get API Resources:
kubectl api-resources 

# Get API Resources storage info:
kubectl api-resources | grep "storage.k8s.io/v1"

```

## Config Maps

```bash

# View current configmaps in all namespaces
kubectl get configmap

# View a specific configmap
kubectl get configmap myconfigmap

# Retrieve the value of a key with dots, e.g. 'ca.crt'
kubectl get configmap myconfigmap -o jsonpath='{.data.ca\.crt}'

# View a specific configmap and pipe the output as json to jq (the json parser)
kubectl get configmap myconfigmap -o json | jq

# View a configmap
kubectl describe configmaps myconfigmap

# View a configmap as YAML:
kubectl get configmaps myconfigmap -o yaml

# Create a configmap
kubectl create configmap [name] [data-source]

# ConfigMap Data Source
# Example kubectl command
# Single file
kubectl create configmap app-settings --from-file=app-container/settings/app.properties

# Multiple files
kubectl create configmap app-settings --from-file=app-container/settings/app.properties--from-file=app-container/settings/backend.properties

# Env-file
kubectl create configmap app-env-file--from-env-file=app-container/settings/app-env-file.properties

# Directory
kubectl create configmap app-settings --from-file=app-container/settings/

```

## Daemonset  

```bash

# Get a daemonset
kubectl get daemonset my-daemonset

# Describe a daemonset
kubectl describe daemonset my-daemonset

# Create a daemonset
kubectl create -f daemonset-example.yaml

# Delete a daemonset
kubectl delete daemonset my-daemonset
```


## Updating resources  

```bash

# Rolling update "www" containers of "frontend" deployment, updating the image
kubectl set image deployment/frontend www=image:v2

# Check the history of deployments including the revision
kubectl rollout history deployment/frontend

# Rollback to the previous deployment
kubectl rollout undo deployment/frontend

# Rollback to a specific revision
kubectl rollout undo deployment/frontend --to-revision=2

# Watch rolling update status of "frontend" deployment until completion
kubectl rollout status -w deployment/frontend

# Rolling restart of the "frontend" deployment
kubectl rollout restart deployment/frontend

# Replace a pod based on the JSON passed into std
cat pod.json | kubectl replace -f -

# Force replace, delete and then re-create the resource. Will cause a service outage.
kubectl replace --force -f ./pod.json

# Create a service for a replicated nginx, which serves on port 80 and connects to the containers on port 8000
kubectl expose rc nginx --port=80 --target-port=8000

# Update a single-container pod's image version (tag) to v4
kubectl get pod my-pod -o yaml | sed 's/\(image: myimage\):.*$/\1:v4/' | kubectl replace -f -

# Add a Label
kubectl label pods my-pod new-label=awesome

# Add an annotation
kubectl annotate pods my-pod icon-url=http://goo.gl/XXBTWq

# Auto scale a deployment "foo"
kubectl autoscale deployment foo --min=2 --max=10

# Scale CoreDNS Deployent:
kubectl scale deployments/coredns --replicas=2 -n kube-system

# Scale a Auto-Scaler if using
kubectl scale deployments/cluster-autoscaler --replicas=0 -n kube-system

```

## Patching resources

```bash
# Partially update a node
kubectl patch node k8s-node-1 -p '{"spec":{"unschedulable":true}}'

# Update a container's image; spec.containers[*].name is required because it's a merge key
kubectl patch pod valid-pod -p '{"spec":{"containers":[{"name":"kubernetes-serve-hostname","image":"new image"}]}}'

# Update a container's image using a json patch with positional arrays
kubectl patch pod valid-pod --type='json' -p='[{"op": "replace", "path": "/spec/containers/0/image", "value":"new image"}]'

# Disable a deployment livenessProbe using a json patch with positional arrays
kubectl patch deployment valid-deployment  --type json   -p='[{"op": "remove", "path": "/spec/template/spec/containers/0/livenessProbe"}]'

# Add a new element to a positional array
kubectl patch sa default --type='json' -p='[{"op": "add", "path": "/secrets/1", "value": {"name": "whatever" } }]'

# Update a deployment's replica count by patching its scale subresource
kubectl patch deployment nginx-deployment --subresource='scale' --type='merge' -p '{"spec":{"replicas":2}}'

```

## Editing resources  

Edit any API resource in your preferred editor.

```bash
# Edit the service named docker-registry
kubectl edit svc/docker-registry

# Use an alternative editor
KUBE_EDITOR="nano" kubectl edit svc/docker-registry 
```

## Scaling resources  

```bash
# Scale a replicaset named 'foo' to 3
kubectl scale --replicas=3 rs/foo

# Scale a resource specified in "foo.yaml" to 3
kubectl scale --replicas=3 -f foo.yaml

# If the deployment named mysql's current size is 2, scale mysql to 3
kubectl scale --current-replicas=2 --replicas=3 deployment/mysql

# Scale multiple replication controllers
kubectl scale --replicas=5 rc/foo rc/bar rc/baz
```

## Deleting resources  

```bash
# Delete a service account
kubectl -n my-namespace delete serviceaccount kubewatch

# Delete a pod using the type and name specified in pod.json
kubectl delete -f ./pod.json

# Delete pods and services with same names "baz" and "foo"
kubectl delete pod,service baz foo

# Forceuflly Delete a pod
kubectl delete pod [PODNAME] --grace-period=0 --force --namespace [NAMESPACE]

# Delete pods and services with label name=myLabel
kubectl delete pods,services -l name=myLabel

# Delete all pods and services in namespace my-ns
kubectl -n my-ns delete pod,svc --all

# Delete All pods in a namespace
kubectl delete --all pods --namespace=my-namespace

# Below command to delete all pods from particular namespace if stuck in terminating state 
# or you are not able to delete it then you can delete pods forcefully.
kubectl delete pods --all --grace-period=0 --force --namespace namespace
Or
for p in $(kubectl get pods | grep Terminating | awk '{print $1}'); do kubectl delete pod $p --grace-period=0 --force;done
Other Options for deleting pods:
for p in $(kubectl get pods | grep CreateContainerError | awk '{print $1}'); do kubectl delete pod $p --grace-period=0 --force;done
for p in $(kubectl get pods | grep ErrImagePull | awk '{print $1}'); do kubectl delete pod $p --grace-period=0 --force;done


# If you have multiple pod which are crashing or error and you want to delete them
kubectl delete pods --all -n | gep

# Delete all pods matching the awk pattern1 or pattern2
kubectl get pods -n mynamespace --no-headers=true | awk '/pattern1|pattern2/{print $1}' | xargs  kubectl delete -n mynamespace pod

# Delete all deployments in a namespace
kubectl delete --all deployments --namespace=my-namespace

# Delete all namespaces
kubectl delete --all namespaces

# This command will delete all the namespaces except kube-system, which might be useful:
for each in $(kubectl get ns -o jsonpath="{.items[*].metadata.name}" | grep -v kube-system);
do
  kubectl delete ns $each
done

# Delete EVERYTHING (BE CAREFUL!)
  # The first all means the common resource kinds (pods, replicasets, deployments, ...)
  #   kubectl get all == kubectl get pods,rs,deployments, ...
  # The second --all means to select all resources of the selected kinds
  # Note that all does not include:
  # non namespaced resourced (e.g., clusterrolebindings, clusterroles, ...)
  #  configmaps
  #  rolebindings
  #  roles
  #  secrets
  #  ...

kubectl delete all --all --all-namespaces


```

## Interacting with running Pods  

```bash
# dump pod logs (stdout)
kubectl logs my-pod

# dump pod logs (stdout) for a specific namespace
kubectl logs pod-name -n mynamespace

# dump pod logs, with label name=myLabel (stdout)
kubectl logs -l name=myLabel

# dump pod logs (stdout) for a previous instantiation of a container
kubectl logs my-pod --previous

# dump pod container logs (stdout, multi-container case)
kubectl logs my-pod -c my-container

# dump pod logs, with label name=myLabel (stdout)
kubectl logs -l name=myLabel -c my-container

# dump pod container logs (stdout, multi-container case) for a previous instantiation of a container
kubectl logs my-pod -c my-container --previous

# stream pod logs (stdout)
kubectl logs -f my-pod

# stream pod container logs (stdout, multi-container case)
kubectl logs -f my-pod -c my-container

# stream all pods logs with label name=myLabel (stdout)
kubectl logs -f -l name=myLabel --all-containers

# Run pod as interactive shell
kubectl run -i --tty busybox --image=busybox -- sh

# Run pod nginx in a specific namespace
kubectl run nginx --image=nginx -n mynamespace

# Run pod nginx and write its spec into a file called pod.yaml
kubectl run nginx --image=nginx --dry-run=client -o yaml ] pod.yaml

# Attach to Running Container
kubectl attach my-pod -i

# Run command in existing pod (1 container case)
kubectl exec my-pod -- ls /

# Get an interactive shell in a pod. 
# The -it makes the execution interactive or gives you an interactive interface.
kubectl exec -it my-pod -- /bin/bash

# Run command in existing pod (multi-container case)
kubectl exec my-pod -c my-container -- ls /

# Interactive shell access to a running pod (1 container case)
kubectl exec --stdin --tty my-pod -- /bin/sh

# Interactive shell access to a running pod (multi-containercase)
kubectl exec --stdin --tty my-pod -c my-container -- /bin/sh
Or:
kubectl exec --stdin --tty my-pod -c my-container -- bash
Or:
# Run a Shell in an existing container in an existing pod (-it = Pass stdin to the container, Stdin is a TTY)
kubectl exec -c my-container -it my-pod -- bash

# Run command in existing pod (multi-container case)
kubectl exec my-pod -c my-container -- ls /

# Show metrics for a pod in a specific namespace
kubectl top pod my-pod --namespace=my-namespace

# Show metrics for a given pod and its containers
kubectl top pod my-pod --containers

# Show metrics for a given pod and sort it by 'cpu' or 'memory'
kubectl top pod my-pod --sort-by=cpu
kubectl top pod my-pod --sort-by=memory

```

## Copy files and directories to and from containers
```bash
# Copy /tmp/foo_dir local directory to /tmp/bar_dir in a remote pod in the current namespace
kubectl cp /tmp/foo_dir my-pod:/tmp/bar_dir

# Copy /tmp/foo local file to /tmp/bar in a remote pod in a specific container
kubectl cp /tmp/foo my-pod:/tmp/bar -c my-container

# Copy /tmp/foo local file to /tmp/bar in a remote pod in namespace my-namespace
kubectl cp /tmp/foo my-namespace/my-pod:/tmp/bar

# Copy /tmp/foo from a remote pod to /tmp/bar locally
kubectl cp my-namespace/my-pod:/tmp/foo /tmp/bar

```

## Interacting with Deployments and Services  

```bash
# Dump Pod logs for a Deployment (single-container case)
kubectl logs deploy/my-deployment

# Dump Pod logs for a Deployment (multi-container case)
kubectl logs deploy/my-deployment -c my-container

# Dump logs for a pod
kubectl logs my-pod

# Dump all container logs for a pod
kubectl logs -f my-pod --all-containers=true

# Listen on port 5000 on the local machine and forward to port 6000 on my-pod
kubectl port-forward my-pod 5000:6000

# Listen on local port 5000 and forward to port 5000 on Service backend
kubectl port-forward svc/my-service 5000

# Listen on local port 5000 and forward to Service target port with name [my-service-port]
kubectl port-forward svc/my-service 5000:my-service-port

# Listen on local port 5000 and forward to port 6000 on a Pod created by [my-deployment]
kubectl port-forward deploy/my-deployment 5000:6000

# To access a Database from a Database client (Forwarding to your local Database client)
kubectl port-forward db-proxy-8c4589d89c-tx8qo 8080:8080

# Run command in first Pod and first container in Deployment (single- or multi-container cases)
kubectl exec deploy/my-deployment -- ls
```

## Interacting with Nodes and cluster  

```bash
# Mark [node] as unschedulable
kubectl cordon [node]

# Drain [node] in preparation for maintenance
kubectl drain [node]
# OR:
kubectl drain --delete-local-data --ignore-daemonsets [node]

# Mark [node] as schedulable
kubectl uncordon [node]

# Show metrics for a given node
kubectl top node [node]

# Display addresses of the master and services
kubectl cluster-info

# Dump current cluster state to stdout
kubectl cluster-info dump

# Dump current cluster state to /path/to/cluster-state
kubectl cluster-info dump --output-directory=/path/to/cluster-state

# If a taint with that key and effect already exists, its value is replaced as specified.
kubectl taint nodes foo dedicated=special-user:NoSchedule
```

## Resource types  

List all supported resource types along with their shortnames, [API group](https://kubernetes.io/docs/concepts/overview/kubernetes-api/#api-groups-and-versioning), whether they are [namespaced](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces), and [Kind](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects):

```bash
kubectl api-resources
```

## Other operations for exploring API resources  

```bash
# All namespaced resources
kubectl api-resources --namespaced=true

# All non-namespaced resources
kubectl api-resources --namespaced=false

# All resources with simple output (only the resource name)
kubectl api-resources -o name

# All resources with expanded (aka "wide") output
kubectl api-resources -o wide

# All resources that support the "list" and "get" request verbs
kubectl api-resources --verbs=list,get

# All resources in the "extensions" API group
kubectl api-resources --api-group=extensions
```

## Formatting output  

To output details to your terminal window in a specific format, add the -o (or --output) flag to a supported kubectl command.

| Output format | Description |
|---------------|-------------|
| -o=custom-columns=[spec] | Print a table using a comma separated list of custom columns |
| -o=custom-columns-file=[filename] | Print a table using the custom columns template in the [filename] file |
| -o=json | Output a JSON formatted API object |
| -o=jsonpath=[template] | Print the fields defined in a jsonpath expression |
| -o=jsonpath-file=[filename] | Print the fields defined by the jsonpath expression in the [filename] file |
| -o=name | Print only the resource name and nothing else |
| -o=wide | Output in the plain-text format with any additional information, and for pods, the node name is included |
| -o=yaml | Output a YAML formatted API object |

*Examples using -o=custom-columns:*  

```bash
# All images running in a cluster
kubectl get pods -A -o=custom-columns='DATA:spec.containers[*].image'

# All images running in namespace: default, grouped by Pod
kubectl get pods --namespace default --output=custom-columns="NAME:.metadata.name,IMAGE:.spec.containers[*].image"

 # All images excluding "k8s.gcr.io/coredns:1.6.2"
kubectl get pods -A -o=custom-columns='DATA:spec.containers[?(@.image!="k8s.gcr.io/coredns:1.6.2")].image'

# All fields under metadata regardless of name
kubectl get pods -A -o=custom-columns='DATA:metadata.*'
```

More examples in the kubectl [reference documentation](https://kubernetes.io/docs/reference/kubectl/overview/#custom-columns).

## Kubectl output verbosity and debugging  

Kubectl verbosity is controlled with the -v or --v flags followed by an integer representing the log level. General Kubernetes logging conventions and the associated log levels are described here.  

| Verbosity | Description |
|-----------|-------------|
| --v=0 | Generally useful for this to always be visible to a cluster operator. |
| --v=1 | A reasonable default log level if you don't want verbosity. |
| --v=2 | Useful steady state information about the service and important log messages that may correlate to significant changes in the system. This is the recommended default log level for most systems. |
| --v=3 | Extended information about changes. |
| --v=4 | Debug level verbosity. |
| --v=5 | Trace level verbosity. |
| --v=6 | Display requested resources. |
| --v=7 | Display HTTP request headers. |
| --v=8 | Display HTTP request contents. |
| --v=9 | Display HTTP request contents without truncation of contents. |



---


**Deploying the Kubernetes (k8s) Dashboard UI**  

This section will outline how to install the Kubernetes Dashboard on AWS EKS/ECS and Non AWs EKS/ECS deployments.  


## Deploying the Kubernetes Dashboard UI for AWS EKS/ECS  

### Step 1: Deploy the Kubernetes dashboard   

**First steps:**  

- Authenticate to AWS on the command line in whatever manner you choose.

**Change to the context you want to deploy dashbaord to:**  

```bash
kubectl config use-context [ARN_OF_CONTEXT]
```

For Regions other than Beijing and Ningxia China, apply the Kubernetes dashboard.  
**Note:** Not putting in instructions for Beijing and Ningxia China here.  

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
namespace/kubernetes-dashboard created
serviceaccount/kubernetes-dashboard created
service/kubernetes-dashboard created
secret/kubernetes-dashboard-certs created
secret/kubernetes-dashboard-csrf created
secret/kubernetes-dashboard-key-holder created
configmap/kubernetes-dashboard-settings created
role.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrole.rbac.authorization.k8s.io/kubernetes-dashboard created
rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
deployment.apps/kubernetes-dashboard created
service/dashboard-metrics-scraper created
deployment.apps/dashboard-metrics-scraper created
```

**Check if Namespace was created:**  

```bash
kubens

appmesh
aws-observability
myapp1
default
external-dns
kube-node-lease
kube-public
kube-system
kubernetes-dashboard
```

**Switch to `kubernetes-dashboard` namespace:**  

```bash
kubens kubernetes-dashboard
Context "[ARN_OF_CONTEXT]" modified.
Active namespace is "kubernetes-dashboard".
```

**Check for the pods:**  

```bash
kubectl get pods
NAME                                        READY   STATUS    RESTARTS   AGE
dashboard-metrics-scraper-c45b7869d-pr5dn   1/1     Running   0          5m41s
kubernetes-dashboard-576cb95f94-9lmxp       1/1     Running   0          5m41s
```


### Step 2: Create an eks-admin service account and cluster role binding  

By default, the Kubernetes Dashboard user has limited permissions. In this section, you create an `eks-admin` service account and cluster role binding that you can use to securely connect to the dashboard with admin-level permissions. For more information, see [Managing Service Accounts](https://kubernetes.io/docs/admin/service-accounts-admin/) in the Kubernetes documentation.  

**To create the eks-admin service account and cluster role binding**  

1) Create a file called `eks-admin-service-account.yaml` with the text below. This manifest defines a service account and cluster role binding called `eks-admin`. 

```bash
vim eks-admin-service-account.yaml
```

```yml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: eks-admin
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: eks-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: eks-admin
  namespace: kube-system
```

2) Apply the service account and cluster role binding to your cluster.

```bash
kubectl apply -f eks-admin-service-account.yaml
serviceaccount/eks-admin created
Warning: rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
clusterrolebinding.rbac.authorization.k8s.io/eks-admin created
```

### Step 3: Connect to the dashboard  

Now that the Kubernetes Dashboard is deployed to your cluster, and you have an administrator service account that you can use to view and control your cluster, you can connect to the dashboard with that service account.

**To connect to the Kubernetes dashboard**  

1) Retrieve an authentication token for the `eks-admin` service account. Copy the `[authentication_token]` value from the output. You use this token to connect to the dashboard. 

```bash
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep eks-admin | awk '{print $1}')
Name:         eks-admin-token-q6cxk
Namespace:    kube-system
Labels:       [none]
Annotations:  kubernetes.io/service-account.name: eks-admin
              kubernetes.io/service-account.uid: fba717be-b152-48c2-bb71-f4a56aff653c

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1066 bytes
namespace:  11 bytes
token:     
[TOKEN]
```

2) Start the kubectl proxy.

```bash
kubectl proxy
Starting to serve on 127.0.0.1:8001
```

3) To access the dashboard endpoint, open the following link with a web browser:  

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#!/login

4) Choose **Token**, paste the `[authentication_token]` output from the "Retrieve an authentication token" command from above into the **Token** field, and choose **SIGN IN**. 

### Step 4: Next steps  

After you have connected to your Kubernetes Dashboard, you can view and control your cluster using your eks-admin service account.  
For more information about using the dashboard, see the [project documentation on GitHub](https://github.com/kubernetes/dashboard).  

**References:**  
https://aws.amazon.com/premiumsupport/knowledge-center/eks-cluster-kubernetes-dashboard/  

https://docs.aws.amazon.com/eks/latest/userguide/dashboard-tutorial.html  

https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/  

*Monitoring and Alerting*  

https://www.tigera.io/learn/guides/kubernetes-monitoring/  

https://sysdig.com/blog/alerting-kubernetes/  

---

## Deploying the Kubernetes Dashboard UI (Non AWS EKS/ECS)  

The Dashboard UI is not deployed by default. To deploy it, run the following command:  

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
```

### Accessing the Dashboard UI  

To protect your cluster data, Dashboard deploys with a minimal RBAC configuration by default. Currently, Dashboard only supports logging in with a Bearer Token. 
To create a token for this demo, you can follow our guide on creating a [sample user](https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md).

### Command line proxy  
You can enable access to the Dashboard using the kubectl command-line tool, by running the following command:  

```bash
kubectl proxy
```

Kubectl will make Dashboard available at:  

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/.

**Important Note:**  
The UI can only be accessed from the machine where the command is executed. See `kubectl proxy --help` for more options.

### Steps to run once Dashboard is deployed and for normal operations  

**First steps:**  

- Authenticate to AWS (like with awsauth.sh or awsvault)

```bash
source ~/awsauth.sh [TOKEN_CODE_FROM_1PASSWORD]
```

- Change to the context you want to get to dashbaord for:

```bash
kubectl config use-context [ARN_OF_CONTEXT]
```

1) Retrieve an authentication token for the eks-admin service account. Copy the [authentication_token] value from the output. You use this token to connect to the dashboard. 
**Run:**  

```bash
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep eks-admin | awk '{print $1}')
```

2) **Run:**  

```bash
kubectl proxy
```

3) Put this URL in Browser:
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#!/login  

3) Choose Token, paste the [authentication_token] output from the  command in Step 1 into the Token field, and choose SIGN IN. 

**Important Note:**  
The UI can only be accessed from the machine where the command is executed. See `kubectl proxy --help` for more options.


**References:**  

https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard  

https://github.com/kubernetes/dashboard  

https://www.aquasec.com/cloud-native-academy/kubernetes-101/kubernetes-dashboard  

https://docs.aws.amazon.com/eks/latest/userguide/dashboard-tutorial.html  

https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html  

https://aws.amazon.com/premiumsupport/knowledge-center/eks-cluster-kubernetes-dashboard/  

Install Kubernetes Dashboard in Kubernetes "master" node (Non-AWS EKS):  
https://adamtheautomator.com/kubernetes-dashboard  
 
---

## kubens  

kubens is a utility to switch between Kubernetes namespaces.  

USAGE:  
  kubens                    : list the namespaces  
  kubens [NAME]             : change the active namespace  
  kubens -                  : switch to the previous namespace  
  kubens -c, --current      : show the current namespace  



### List Namespaces  

```bash
kubens

appmesh
aws-observability
my-app
default
external-dns
kube-node-lease
kube-public
kube-system
```

### Show namespace context

```bash
kubens appmesh
Context "arn:aws:eks:us-west-2:123456789012:cluster/dev-abcd123" modified.
Active namespace is "appmesh".
```

### Switch to namespace  

```bash
kubens [MY_NAMESPACE]
```


---

## AWS Elastic Kubernetes Service (EKS) and Elastic Container Service (ECS)  

**Note:**  

If get this error interacting with clusters:  
`error: You must be logged in to the server (Unauthorized)`

*Run:*  
```bash
aws eks --region [REGTION] list-clusters

#Example:
aws eks --region us-west-2 list-clusters
```

*Then run for each cluster:*  

```bash
aws eks --region [REGION] update-kubeconfig --name [NAME_OF_CLUSTER]

# Example:
aws eks --region us-west-2 update-kubeconfig --name dev-abcd1232
```


### List clusters  

```bash
aws eks --region [REGION] list-clusters
```

**Exampe:**  

```bash
aws eks --region us-west-2 list-clusters
{
    "clusters": [
        "dev-abcd123",
        "qa-abcd123"
    ]
}
```


### Add Cluster to K8s config  

```bash
aws eks --region [REGION] update-kubeconfig --name [NAME_OF_CLUSTER]
```

**Example:**  

```bash
aws eks --region us-west-2 update-kubeconfig --name dev-abcd1232
Updated context arn:aws:eks:us-west-2:123456789012:cluster/dev-abcd1232 in /home/user1/.kube/config
```

### Security Policies

**Check pod security policies:**  

```bash
kubectl get psp eks.privileged
```

## Troubleshooting  

When a pod is in `CrashLoopBackOff` (status) endless loop **or** if pod is stuck in `Terminating`- delete the pod and let K8's recreate one.  

```bash
kubectl get pods

NAME                                            READY   STATUS             RESTARTS   AGE
aws-load-balancer-controller-6f687c6cf8-pmwzg   1/1     Running            0          41h
aws-node-72svh                                  1/1     Running            227        21d
aws-node-x4d2w                                  0/1     CrashLoopBackOff   418        21d
aws-node-zv87v                                  1/1     Running            215        21d
coredns-f47955f89-j8j9p                         1/1     Running            0          5h17m
coredns-f47955f89-nbx2h                         1/1     Running            0          41h
kube-proxy-hqdjf                                1/1     Running            0          21d
kube-proxy-htc4c                                1/1     Running            0          21d
kube-proxy-zw4zz                                1/1     Running            0          21d

kubectl get pods

NAME                                            READY   STATUS             RESTARTS   AGE
myapp-webserver-27429085-kt4dc                  0/1     Terminating         0          33m

```

```bash
kubectl delete pod -n=NAMESPACE POD_NAME

# Example:
kubectl delete pod -n=kube-system aws-node-x4d2w
```

```bash
# When a pod or pods are stuck in Terminating status

kubectl get pods

NAME                                    READY   STATUS        RESTARTS   AGE
myapp1-9f44c778f-bgrqs            0/2     Pending       0          3m41s
myapp1-9f44c778f-j4shp            0/2     Pending       0          3m44s
myapp1-9f44c778f-r98ft            0/2     Pending       0          3m48s
myapp1-webserver-27581249-smhx4   0/1     Terminating   0          2d21h
myapp1-webserver-27581258-v9gm8   0/1     Terminating   0          2d21h
myapp1-webserver-27581262-gg59d   0/1     Terminating   0          2d21h


# When pod is stuck in Terminating status - you have to Forceuflly Delete the pod
kubectl delete pod [PODNAME] --grace-period=0 --force --namespace [NAMESPACE]
```

---

## References  

https://kubernetes.io/docs/reference/kubectl/cheatsheet/  

https://kubernetes.io/docs/concepts/architecture/nodes/  

https://kubernetes.io/docs/setup/best-practices/enforcing-pod-security-standards/  

https://github.com/kubernetes-sigs/metrics-server  

https://devtech0101.medium.com/create-your-own-3-nodes-k8s-cluster-kubernetes-v-1-20-on-cri-o-1-17-and-ubuntu-20-10-bddbe715d44f  

https://aveuiller.github.io/kubernetes_apprentice_cookbook.html  

https://github.com/honey336/-aws-eks-kubernetes-masterclass  
