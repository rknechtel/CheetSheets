
# Basic Helm Concepts

Helm is a Kubernetes deployment tool for automating creation, packaging, configuration, and deployment of applications and services to Kubernetes clusters.  Think of it as the package manager for Kubernetes (K8s).  

Helm commands work with several Helm-related concepts. Understanding them makes the syntax easier to follow.


- The most important Helm concept is a chart. A chart is a set of Kubernetes yaml manifests packaged together for easy manipulation.  
  Helm charts make it possible to deploy a containerized application using a single command.
- Charts are grouped in online collections called repositories. Each repository has a name and URL, making the charts easy to locate,  
  download, and install.
- Helm Hub is an online collection of distributed repositories available on the internet. It works as an information center,  
  where you can find apps and their repository addresses. As of today, it is not possible to install an app directly from Helm Hub.
- A release is a single instance of a chart deployed in a Kubernetes cluster.

## List of Helm Commands

Use the commands listed below as a quick reference when working with Helm inside Kubernetes.  

### Get Help and Version Information

**Display the general help output for Helm:**  

```bash
helm --help
```

**Show help for a particular helm command:**  

```bash
helm [command] --help
```

**See the installed version of Helm:**  

```bash
helm version
```

### Install and Uninstall Apps

The main function of Helm is Kubernetes app management. Besides the basic operations of installing and uninstalling apps, Helm enables you to perform test installations and customize the installation process.  

**Install an app:**  

```bash
helm install [app-name] [chart]
```

**Install an app in a specific namespace:**  

```bash
helm install [app-name] [chart] --namespace [namespace]
```

**Override the default values with those specified in a file of your choice:**  

```bash
helm install [app-name] [chart] --values [yaml-file/url]
```

**Run a test installation to validate and verify the chart:**  

```bash
helm install [app-name] --dry-run --debug
```

**Uninstall a release:**  

```bash
helm uninstall [release]
```

### Perform App Upgrade and Rollback

Helm offers users multiple options for app upgrades, such as automatic rollback and upgrading to a specific version. Rollbacks can also be executed on their own. For detailed instructions on how to perform a rollback, check out [How to Roll Back Changes with Helm](##-How-to-Roll-Back-to-the-Previous-Release-in-Helm).  

**Upgrade an app:**  

```bash
helm upgrade [release] [chart]
```

**Instruct Helm to rollback changes if the upgrade fails:**  

```bash
helm upgrade [release] [chart] --atomic
```

**Upgrade a release. If it does not exist on the system, install it:**  

```bash
helm upgrade [release] [chart] --install
```

**Upgrade to a specified version:**  

```bash
helm upgrade [release] [chart] --version [version-number]
```

**Roll back a release:**  

```bash
helm rollback [release] [revision]
```

### Download Release Information

The helm get command lets you download information about a release.  

**Download all the release information:**  

```bash
helm get all [release]
```

**Download all hooks:**  

```bash
helm get hooks [release]
```

**Download the manifest:**  

```bash
helm get manifest [release]
```

***Download the notes:***  

```bash
helm get notes [release]
```

**Download the values file:**  

```bash
helm get values [release]
```

**Fetch release history:**  

```bash
helm history [release] 
```

### Add, Remove, and Update Repositories

The command helm repo helps you manipulate chart repositories.  

**Add a repository from the internet:**  

```bash
helm repo add [repository-name] [url]
```

**Remove a repository from your system:**  

```bash
helm repo remove [repository-name]
```

**Update repositories:**  

```bash
helm repo update
```

### List and Search Repositories

Use the helm repo and helm search commands to list and search Helm repositories. helm search also enables you to find apps and repositories in Helm Hub.  

**List chart repositories:**  

```bash
helm repo list
```

**Generate an index file containing charts found in the current directory:**  

```bash
helm repo index
```

**Search charts for a keyword:**  

```bash
helm search [keyword]
```

**Search repositories for a keyword:**  

```bash
helm search repo [keyword]
```

**Search Helm Hub:**  

```bash
helm search hub [keyword]
```

### Release Monitoring

The helm list command enables listing releases in a Kubernetes cluster according to several criteria, including using regular (Pearl compatible) expressions to filter results. Commands such as helm status and helm history provide more details about releases.  

**List all available releases in the current namespace:**  

```bash
helm list
```

**List all available releases across all namespaces:**  

```bash
helm list --all-namespaces
```

**List all releases in a specific namespace:**  

```bash
helm list --namespace [namespace]
```

**List all releases in a specific output format:**  

```bash
helm list --output [format]
```

**Apply a filter to the list of releases using regular expressions:**  

```bash
helm list --filter '[expression]'
```

**See the status of a specific release:**  

```bash
helm status [release]
```

**Display the release history:**  

```bash
helm history [release]
```

**See information about the Helm client environment:**  

```bash
helm env
```

### Plugin Management

Install, manage and remove Helm plugins by using the helm plugin command.  

**Install plugins:**  

```bash
helm plugin install [path/url1] [path/url2] ...
```

**View a list of all installed plugins:**  

```bash
helm plugin list
```

**Update plugins:**  

```bash
helm plugin update [plugin1] [plugin2] ...
```

**Uninstall a plugin:**  

```bash
helm plugin uninstall [plugin]
```

### Chart Management

Helm charts use Kubernetes resources to define an application. To find out more about their structure and requirements for their creation, refer to [How to Create a Helm Chart](##-How-To-Create-A-Helm-Chart---Introduction).  

**Create a directory containing the common chart files and directories (chart.yaml, values.yaml, charts/ and templates/):**  

```bash
helm create [name]
```

**Package a chart into a chart archive:**  

```bash
helm package [chart-path]
```

**Run tests to examine a chart and identify possible issues:**  

```bash
helm lint [chart]
```

**Inspect a chart and list its contents:**  

```bash
helm show all [chart] 
```

**Display the chart’s definition:**  

```bash
helm show chart [chart]
```

**Display the chart’s values:**  

```bash
helm show values [chart]
```

**Download a chart:**  

```bash
helm pull [chart]
```

**Download a chart and extract the archive’s contents into a directory:**  

```bash
helm pull [chart] --untar --untardir [directory]
```

**Display a list of a chart’s dependencies:**  

```bash
helm dependency list [chart]
```

---

## How to Roll Back to the Previous Release in Helm

**Helm uses the rollback command to return to a previous revision:**  

**Use the ls command to find the name of the current Helm release:**  

```bash
helm ls
```

```bash
helm ls -A

NAME                            	NAMESPACE  	REVISION	UPDATED                                	STATUS  	CHART                                  	APP VERSION
appmesh-controller              	appmesh    	1       	2021-09-25 20:22:36.370733055 +0000 UTC	deployed	appmesh-controller-1.2.1               	1.2.1      
appmesh-crd                     	kube-system	1       	2021-09-25 20:22:32.948564081 +0000 UTC	deployed	appmesh-crds-1.2.1                     	1.2.1      
aws-load-balancer-controller    	kube-system	1       	2021-09-25 20:22:35.609165403 +0000 UTC	deployed	aws-load-balancer-controller-1.0.8     	v2.0.1     
aws-load-balancer-controller-crd	kube-system	1       	2021-09-25 20:22:32.948606503 +0000 UTC	deployed	aws-load-balancer-controller-crds-1.0.8	v2.0.1     
external-dns                    	default    	2       	2021-09-30 00:17:19.958086361 +0000 UTC	deployed	external-dns-1.0.0                     	v1.0.0
```

**Use the history command to find the current revision number:**  

```bash
helm history [release]
```

**Example:**  

```bash
helm history libreoffice-online
REVISION	UPDATED                 	STATUS  	CHART                   	APP VERSION	DESCRIPTION     
1       	Fri Oct  1 10:33:40 2021	deployed	libreoffice-online-0.1.0	1.16.0     	Install complete
```

**Roll back to a previous release by using the helm rollback command. The rollback command uses the following syntax:**  

```
helm rollback [release] [revision] [flag]
```

*Where:*  
- [release]: The release name you want to roll back to.
- [revision]: The revision number you want to roll back to.
- [flag]: Optional command flags, such as --dry-run or --force.

For example, to roll back to the libreoffice-online release 1, revision 1, enter:

```bash
helm rollback libreoffice-online-01 1
```

### How to Roll Back Using kubectl

**The rollout undo command allows you to roll back your deployment using kubectl:**  

```bash
kubectl rollout undo deployment/[release]
```

**To roll back to a specific revision, use:**  

```bash
kubectl rollout undo deployment/[release] --to-revision=[revision]
```

---

## Helm Repositories - Introduction

Helm chart repositories are remote servers containing a collection of Kubernetes resource files. Charts are displayed in directory trees and packaged into Helm chart repositories.  
To deploy applications with Helm, you need to know how to manage repositories.  

### How to Add Helm Repositories

**The general syntax for adding a Helm chart repository is:**  

```bash
helm repo add [NAME] [URL] [flags]
```

**To add official stable Helm charts, enter the following command:**  

```bash
helm repo add stable https://charts.helm.sh/stable
```

**The terminal prints out a confirmation message when adding is complete:**  

```bash
"stable" has been added to your repositories
```

**List the contents of the repository using the search repo command:**  

```bash
helm search repo stable
```

The terminal prints out the list of all available charts.  

```bash
NAME                                 	CHART VERSION	APP VERSION            	DESCRIPTION                                       
stable/acs-engine-autoscaler         	2.2.2        	2.1.1                  	DEPRECATED Scales worker nodes within agent pools 
stable/aerospike                     	0.3.5        	v4.5.0.5               	DEPRECATED A Helm chart for Aerospike in Kubern...
stable/airflow                       	7.13.3       	1.10.12                	DEPRECATED - please use: https://github.com/air...
stable/ambassador                    	5.3.2        	0.86.1                 	DEPRECATED A Helm chart for Datawire Ambassador   
stable/anchore-engine                	1.7.0        	0.7.3                  	Anchore container analysis and policy evaluatio...
stable/apm-server                    	2.1.7        	7.0.0                  	DEPRECATED The server receives data from the El...
stable/ark                           	4.2.2        	0.10.2                 	DEPRECATED A Helm chart for ark                   
stable/artifactory                   	7.3.2        	6.1.0                  	DEPRECATED Universal Repository Manager support...
stable/artifactory-ha                	0.4.2        	6.2.0                  	DEPRECATED Universal Repository Manager support...
stable/atlantis                      	3.12.4       	v0.14.0                	DEPRECATED A Helm chart for Atlantis https://ww...
stable/auditbeat                     	1.1.2        	6.7.0                  	DEPRECATED A lightweight shipper to audit the a...
stable/aws-cluster-autoscaler        	0.3.4        	                       	DEPRECATED Scales worker nodes within autoscali...
stable/aws-iam-authenticator         	0.1.5        	1.0                    	DEPRECATED A Helm chart for aws-iam-authenticator 
stable/bitcoind                      	1.0.2        	0.17.1                 	DEPRECATED Bitcoin is an innovative payment net...
stable/bookstack                     	1.2.4        	0.27.5                 	DEPRECATED BookStack is a simple, self-hosted, ...
stable/buildkite                     	0.2.4        	3                      	DEPRECATED Agent for Buildkite                    
stable/burrow                        	1.5.4        	0.29.0                 	DEPRECATED Burrow is a permissionable smart con...
stable/centrifugo                    	3.2.2        	2.4.0                  	DEPRECATED Centrifugo is a real-time messaging ...
stable/cerebro                       	1.9.5        	0.9.2                  	DEPRECATED A Helm chart for Cerebro - a web adm...
stable/cert-manager                  	v0.6.7       	v0.6.2                 	A Helm chart for cert-manager                     
stable/chaoskube                     	3.3.2        	0.21.0                 	DEPRECATED Chaoskube periodically kills random ...
stable/chartmuseum                   	2.14.2       	0.12.0                 	DEPRECATED Host your own Helm Chart Repository    
stable/chronograf                    	1.1.1        	1.7.12                 	DEPRECATED Open-source web application written ...
stable/clamav                        	1.0.7        	1.6                    	DEPRECATED An Open-Source antivirus engine for ...
....
```

### How to Update Helm Repositories

**Update all Helm repositories in your system by running the following command:**  

```bash
helm repo update
```

The output contains a list of all updated repositories.

```bash
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "stable" chart repository
Update Complete. *Happy Helming!*
```

### How to Remove Helm Repositories

**The general syntax for removing Helm repositories is:**  

```bash
helm repo remove [REPO1 [REPO2 ...]] [flags]
```

**Remove a Helm repository by entering:**  

```bash
helm repo remove stable
```

The terminal prints out a confirmation message once the repository is removed

```bash
"stable" has been removed from your repositories

```

---

## How To Create A Helm Chart - Introduction

Helm charts are Helm packages consisting of YAML files and templates which convert into Kubernetes manifest files. Charts are reusable by anyone for any environment, which reduces complexity and duplicates.  

Helm charts are one of the best practices for building efficient clusters in Kubernetes. It is a form of packaging that uses a collection of Kubernetes resources. Helm charts use those resources to define an application.  

Helm charts use a template approach to deploy applications. Templates give structure to projects and are suitable for any type of application.  

### Create Helm Chart

Creating a Helm chart involves creating the chart itself, configuring the image pull policy, and specifying additional details in the `values.yaml` file.

#### Create a New Helm Chart

**To create a new Helm chart, use:**  

```bash
helm create <chart name>
```

*For example:*  

```bash
helm create libreoffice-online
```

**Using the ls command, list the chart structure:**  

```bash
ls <chart name>
```

**The Helm chart directory contains:**  

- **Directory** *charts* – Used for adding dependent charts. Empty by default.
- **Directory** *templates* – Configuration files that deploy in the cluster.
- **YAML file** – Outline of the Helm chart structure.
- **YAML file** – Formatting information for configuring the chart.

*Helm Chart Folders have the following structure - Example:*

```bash
libreoffice-online

  charts
    charts.yaml

  templates
    tests
    deployments.yaml
    _helpers.tpl
    hpa.yaml
    ingress.yml
    NOTES.TXT
    service.yasml
    serviceaccount.yaml

  .helmignore
  Chart.yaml
  values.yaml
```

How Do Helm Charts Work?

The three basic concepts of Helm charts are:

- **Chart** – Pre-configured template of Kubernetes resources.
- **Release** – A chart deployed to a Kubernetes cluster using Helm.
- **Repository** – Publicly available charts.

The workflow is to search through repositories for charts and install them to Kubernetes clusters, creating releases.

### Helm Chart Structure

**The files and directories of a Helm chart each have a specific function:**  


| Name | Type | Function |
| ---- | ---- | -------- |
| charts/ | Directory | Directory for manually managed chart dependencies.|
| templates/ | Directory | Template files are written in Golang and combined with configuration values from the values.yaml file to generate Kubernetes manifests.|
| Chart.yaml | File | Metadata about the chart, such as the version, name, search keywords, etc. |
| LICENSE (optional) | File | License for the chart in plaintext format. |
| README.md (optional) | File | Human readable information for the users of the chart. |
| requirements.yaml (optional) | File | List of chart’s dependencies. |
| values.yaml | File | Default configuration values for the chart. |


You can create Helm charts manually or collect publicly available charts from repositories.  

### Helm Chart Repositories

The repositories contain charts which can be installed or shared with other users. Helm provides a command to search directly from the client.   

**There are two general types of searches:** 

- `helm search hub` – Searches through the Artifact Hub from dozens of repositories.
- `helm search repo` – Searches through repositories added in the local helm client using helm repo add.

Without any filters, all the available charts show in the search result. Add a search term to refine the query. 

*For example:* 

```bash
helm search hub libreoffice-online
```

When you find a suitable chart, install it using `helm install`.  

### Helm Chart Releases

**Installing a chart creates a release of the new package. The helm install command takes two arguments:**  

```bash
helm install <release name> <chart name>
```

*Note:*  

Running helm install prints out useful information and whether you should take any actions for installation. Charts are customizable and easily configured before installation. Helm releases are easy to maintain and roll back in case of any unwanted changes.  

### Configure Helm Chart Image Pull Policy

Open the `values.yaml` file in a text editor. Locate the image values:

There are three possible values for the `pullPolicy`:

- **IfNotPresent** – Downloads a new version of the image if one does not exist in the cluster.
- **Always** – Pulls the image on every restart or deployment.
- **Latest** – Pulls the most up-to-date version available.

Change the image `pullPolicy` from `IfNotPresent` to `Always`:

```yaml
image:
  repository: libreoffice-online
  pullPolicy: Always
  # Overrides the image tag whose default is the chart appVersion.
  tag: ""
```

### Helm Chart Name Override

To override the chart name in the `values.yaml` file, add values to the `nameOverride` and `fullnameOverride`:  

```yaml
imagePullSecrets: []
nameOverride: "libreoffice-online"
fullnameOverride: "libreoffice-online"
```

Overriding the Helm chart name ensures configuration files also change.  

### Specify Service Account Name

The service account name for the Helm chart generates when you run the cluster. However, it is good practice to set it manually.  
The service account name makes sure the application is directly associated with a controlled user in the chart.  

Locate the `serviceAccount` value in the `values.yaml` file and specify the name of the service account:

```yaml
serviceAccount:
  # Specifies whether a service account should be created
  create: true
  # Annotations to add to the service account
  annotations: {}
  # The name of the service account to use.
  # If not set and create is true, a name is generated using the fullname template
  name: "libreoffice"
```


Change Networking Service Type

Set your Networking Service Type to either: `ClusterIP` or `NodePort`.

To change the networking service type, locate the service value:
Default service type in the `values.yaml` file is `ClusterIP`.

```yaml
service:
  type: ClusterIP
  port: 80
```

After configuring the `values.yaml` file, check the status of your Kubernetes cluster and deploy the application using Helm commands.  

### Install the Helm Chart

**Install the Helm chart using the helm install command:**  

```bash
helm install <full name override> <chart name>/ --values <chart name>/values.yaml
```

*For example:*  

```bash
helm install libreoffice-online libreoffice-online/ --values libreoffice-online/values.yaml
```

```bash
NAME: libreoffice-online
LAST DEPLOYED: Fri Oct  1 10:33:40 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services libreoffice-online)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
```

The `helm install` command deploys the app. The next steps are printed in the NOTES section of the output.  

**Export the Pod Node Port and IP Address**  


Copy the two `export` commands from the helm install output.  

**Run the commands to get the Pod node port and IP address:  **  

```bash
export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services libreoffice-online)
export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
```

**View the Deployed Application**  

Copy and paste the `echo` command and run it in the terminal to print the IP address and port:

```bash
echo http://$NODE_IP:$NODE_PORT

http://10.0.90.16:30025
```

Copy the link and paste it into your browser, or press CTRL+click to view the deployed application.  

---

## How to Use Environment Variables with Helm

**There are two methods of using environment variables with Helm charts:**  

- Using the secret object in Kubernetes to mount environment variables in a deployment.
- Writing a custom helper in a Helm chart.
	

### Mounting Environment Variables in a Kubernetes Deployment

**Add the following lines to the values.yaml file in your Helm chart:**  

```
username: root
password: password
```

**Create a new file called secret.yaml and add it to the template folder. Add the following content to the file:**  

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: {{ .Release.Name }}-auth
data:
  password: {{ .Values.password | b64enc }}
  username: {{ .Values.username | b64enc }}
 ```

**Edit the env section of your Kubernetes deployment to include the new variables defined in the secret.yaml file:**  

```yaml
...
      containers:
        - name: {{ .Chart.Name }}
          securityContext:
            {{- toYaml .Values.securityContext | nindent 12 }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          env:          
            - name: "USERNAME"
              valueFrom:
                secretKeyRef:
                  key:  username
                  name: {{ .Release.Name }}-auth
            - name: "PASSWORD"
              valueFrom:
                secretKeyRef:
                  key:  password
                  name: {{ .Release.Name }}-auth
...
```

**Set the environment variables to your desired values. For example, set the USERNAME variable to hello_user:**  

```bash
export USERNAME=hello_user
```

**Apply the variables to the Helm chart by combining them with the helm install command:**  

```bash
helm install --set username=$USERNAME [chart name] [chart path]
```

*Where:*  
- [chart name] is the name of the Helm chart you are using.
- [chart path] is the path to the Helm chart you are using.

**If you want to test the new settings out before applying them, use the dry run mode:**  

```bash
helm install --dry-run --set username=$USERNAME --debug [chart name] [chart path]
```

### Adding a Custom Helper in Helm

**Use the env section of the values.yaml file to define sensitive and non-sensitive variables. Use the normal and secret categories to list the appropriate variables:**  

```yaml
secret:
  name: app-env-var-secret
env:
  normal:
    variable1: value1
    variable2: value2
    variable3: value3
  secret:
    variable4: value4
    variable5: value5
    variable6: value6
```

**Using this method, we then add the USERNAME and PASSWORD variables to the secret category:**  

```yaml
…
  secret:
    USERNAME: [username]
    PASSWORD: [password]
```

*Where:*  
- [username] is the value you want to set for the USERNAME variable.
- [password] is the value you want to set for the PASSWORD variable.

**Add the path to the values.yaml file to the bottom of your .gitignore file:**  

```yaml
charts/values.yaml
```

**Create a file called secrets.yaml in the templates folder and add the following content:**  

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: {{ .Values.secret.name }}
type: Opaque
data:
  {{- range $key, $val := .Values.env.secret }}
  {{ $key }}: {{ $val | b64enc }}
  {{- end}}
```

**Find the helpers.tpl file in the templates folder. Add the following to the bottom of the file to write a new helper function:**  

```yaml
{{- define "helpers.list-env-variables"}}
{{- range $key, $val := .Values.env.secret }}
- name: {{ $key }}
  valueFrom:
    secretKeyRef:
      name: app-env-secret
      key: {{ $key }}
{{- end}}
{{- end }}
```

**Call on the helper you created by adding the following to your pod.yaml file:**  

```yaml
…
spec:
  containers:
  - name: mycontainer
    image: redis
    env:
    {{- include "helpers.list-env-variables" . | indent 6 }}
  restartPolicy: Never
```


## Kubernetes Secrets – How to Create, Use and Access Secrets - Introduction


Most applications deployed through Kubernetes require access to databases, services, and other resources located externally. The easiest way to manage the login information necessary to access those resources is using Kubernetes secrets. Secrets help organize and distribute sensitive information across a cluster.  

*What Are Kubernetes Secrets?*  

A Kubernetes secret is an object storing sensitive pieces of data such as usernames, passwords, tokens, and keys. Secrets are created by the system during an app installation or by users whenever they need to store sensitive information and make it available to a pod.  
If passwords, tokens, or keys were simply part of a pod definition or container image, they could be accidentally exposed during Kubernetes operations. Therefore, the most important function of the secret is to prevent accidental exposure of the information stored in it while at the same time making it available wherever the user needs it.  

### Kubernetes Secret Types

**Kubernetes features two categories of secrets:**  
- The system’s service accounts automatically create built-in secrets and associate them with containers together with API credentials.
- You can also create customized secrets for credentials you need to make available to pods.

**Built-in secrets come in several types, corresponding to popular usage scenarios:**  

| Built-in Type | Description |
| ------------- | ----------- |
|Opaque | This is the default type of secret. The secrets whose configuration file does not contain the type statement are all considered to be of this type. Opaque secrets are designed to store arbitrary user data. |
| kubernetes.io/service-account-token | Service account token secrets store tokens identifying service accounts. Upon creation of a pod, Kubernetes automatically creates this secret and associates it with the pod, enabling secure access to the API. This behavior can be disabled. |
| kubernetes.io/dockercfg | Accessing a Docker registry for images requires valid Docker credentials. This type of secret is used to store a serialized ~/.dockercfg legacy format for Docker command-line configuration. It contains the base64-encoded .dockercfg key. |
| kubernetes.io/dockerconfigjson | This type of secret features a .dockerconfigjson key, which is a base64-encoded version of the ~/.docker/config.json file, a new version of the deprecated .dockercfg. |
| kubernetes.io/basic-auth | The secret for storing basic authentication data. It must contain two keys – username and password. |
| kubernetes.io/ssh-auth | For storing data necessary for establishing an SSH connection, use ssh-auth type. This type’s data field must contain an ssh-privatekey key-value pair. |
| kubernetes.io/tls | This type is used to store TLS certificates and keys. The most common usage scenario is Ingress resource termination, but the tls type is also sometimes used with other resources. |
| bootstrap.kubernetes.io/token | Tokens used during the node bootstrap process are stored using the token secret type. This type is usually created in the kube-system namespace. |


To define a customized type of secret, assign a non-empty string as a value in the type field of the secret file. Leaving the field empty tells Kubernetes to assume the Opaque type. The customized type frees the secret of constraints posed by built-in types.  

### Using Kubernetes Secrets

**When you create a secret, it needs to be referenced by the pod that will use it. To make a secret available for a pod:**  

- Mount the secret as a file in a volume available to any number of containers in a pod.
- Import the secret as an environment variable to a container.
- Use kubelet, and the imagePullSecrets field.

The below sections will explain how to create Kubernetes secrets, as well as how to decode and access them.

### Create Kubernetes Secrets

**To create a Kubernetes secret, apply one of the following methods:**

- Use kubectl for a command-line based approach.
- Create a configuration file for the secret.
- Use a generator, such as Kustomize to generate the secret.

**Create Secrets Using kubectl**  

To start creating a secret with `kubectl`, first create the files to store the sensitive information:**  

```bash
echo -n '[username]' > [file1]
echo -n '[password]' > [file2]
```

The `-n` option tells echo not to append a new line at the end of the string. The new line is also treated as a character, so it would be encoded together with the rest of the characters, producing a different encoded value.

Now, use `kubectl` to create a secret using the files from the previous step. Use the generic subcommand to create an `Opaque` secret. Also, add the `--from-file` option for each of the files you want to include:

```bash
kubectl create secret generic [secret-name] \  
--from-file=[file1] \
--from-file=[file2]
```

**To provide keys for values stored in the secret, use the following syntax:**  

```bash
kubectl create secret generic [secret-name] \  
--from-file=[key1]=[file1] \  
--from-file=[key2]=[file2]
```

**Check that the secret has been successfully created by typing:**  

```bash
kubectl get secrets
```

**The command shows the list of available secrets – their names, types, number of data values they contain, and their age:**  

```bash
NAME                                       TYPE                                  DATA   AGE
default-token-5x26f                        kubernetes.io/service-account-token   3      5d20h
libreoffice-token-6tz64                    kubernetes.io/service-account-token   3      69m
sh.helm.release.v1.external-dns.v1         helm.sh/release.v1                    1      5d20h
sh.helm.release.v1.external-dns.v2         helm.sh/release.v1                    1      40h
sh.helm.release.v1.libreoffice-online.v1   helm.sh/release.v1                    1      69m
```

### Create Secrets in a Configuration File

**To create a secret by specifying the necessary information in a configuration file, start by encoding the values you wish to store:**  

```bash
echo -n '[value1]' | base64
echo -n '[value2]' | base64
```

**Now create a yaml file using a text editor. The file should look like this:**  

```yaml
apiVersion: v1
kind: Secret
metadata:  
  name: newsecret
type: Opaque
data:
  username: dXNlcg==
  password: NTRmNDFkMTJlOGZh
```

**Save the file and use the kubectl apply command to create the secret:**  

```bash
kubectl apply -f [file]
```

---

## Create Kubernetes Secret with Generators

### Generators such as Kustomize help quickly generate secrets.

**To create a secret with Kustomize, create a file named kustomization.yaml and format it as follows:**  

```yaml
secretGenerator:
- name: db-credentials 
  files:
  - username.txt
  - password.txt
 ```

The example above states `db-credentials` as the name of the secret and uses two previously created files, `username.txt`, and `password.txt`, as data values.  

**Alternatively, to provide the unencrypted, literal version of the data values, include the literals section with key-value pairs you wish to store:**  

```yaml
secretGenerator:
- name: db-credentials
  literals:
  - username=user
  - password=54f41d12e8fa
```

**Save the file and use the following command in the folder where kustomization.yaml is located:**  

```bash
kubectl apply -k .
```

### Use kubectl describe to See Created Secrets

The kubectl describe command shows basic information about Kubernetes objects. Use it to view the description of a secret.  

```bash
kubectl describe secrets/[secret]
```

### Decode Secrets

**To decode the values in a secret, access them by typing the following command:**  

```bash
kubectl get secret [secret] -o jsonpath='{.data}'
```

**Use the echo command to type the encoded string and pipe the output to the base64 command:**

```bash
echo '[encoded-value]' | base64 --decode
```

### Access Secrets Loaded in a Volume

To access secrets mounted to a pod in a separate volume, modify the definition of the pod to include a new volume. Choose any volume name you want, but make sure that it is the same as the name of the secret object.  

**Be sure to specify readOnly as true. For example, the pod definition may look like this:**  

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pod
  spec:
    containers:
      - name: test-pod
        image: redis
        volumeMounts:
        - name: newsecret
          mountPath: “/etc/newsecret”
          readOnly: true
    volumes:
    - name: newsecret
      secret:
        secretName: newsecret
```

**Open another terminal instance and use the kubectl exec command to access the pod’s bash shell:**  

```bash
kubectl exec -it [pod] -- /bin/bash
```

cd into `/etc/newsecret`, and find the files contained in the secret:

```bsh
cd /etc/newsecret
```

### Project Secrets into a Container Using Environment Variables

Another way to access secrets in a Kubernetes pod is to import them as environment variables by modifying the pod definition to include references to them.  

For example:  

```yaml
apiVersion: v1 
kind: Pod 
metadata: 
  name: secret-env-pod 
spec: 
  containers: 
  - name: secret-env-pod
    image: redis 
    env: 
      - name: SECRET_USERNAME 
        valueFrom: 
          secretKeyRef: 
            name: newsecret 
            key: username 
      - name: SECRET_PASSWORD 
        valueFrom: 
          secretKeyRef: 
            name: newsecret 
            key: password 
  restartPolicy: Never
```

Use `kubectl exec` again to bash into a pod.

**Test the environment variable using the echo command:**  

```bash
echo $[VARIABLE]
```

### Use Secrets to Pull Docker Images from Private Docker Registries

**To use private Docker registries, first, you need to log in to Docker:**  

```bash
sudo docker login
```

When prompted, provide your login credentials:

```bash
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: YOUR_ID_HERE
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

If the login is successful, Docker updates the `config.json` file with your data. Use the cat command to view the file:

```bash
cat ~/.docker/config.json
```

The `auths` section contains the `auth` key, which is an encoded version of the Docker credentials.  

**Use kubectl to create a secret, providing the location of the config.json file and the type of the secret:**  

```bash
kubectl create secret generic [secret] \
--from-file=.dockerconfigjson=./.docker/config.json \
--type=kubernetes.io/dockerconfigjson
```

**Alternatively, perform all the steps above, including logging in to Docker, on the same line:**  

```bash
kubectl create secret docker-registry [secret] --docker-server:[address] --docker-username=[username] --docker-password=[password] --docker-email=[email]
```

**To create a pod that has access to this secret, create a yaml file that defines it. The file should look like this:**  

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: private-reg
spec: 
  containers:
  - name: private-reg-container
    image:   
  imagePullSecrets:  
  - name: regcred
```

**Finish creating the pod by activating it with kubectl apply:** 

```bash
kubectl apply -f [file]
```

### Kubernetes Secrets Considerations

**Kubernetes secrets are a secure way to store sensitive information. However, before you decide on the best method for your usage scenario, you should consider the following points:**  

- Usernames and passwords in Secrets are encoded with base-64. This text-encoding technique obscures data and prevents accidental exposure, but it is not secure against malicious cyber attacks.
- Secrets are only available in the cluster in which they are located.
- Secrets usually rely on a master key which is used to unlock them all. While there are methods to secure the master key, using them only creates another master key scenario.

**To mitigate these problems, apply some of the solutions below:**  

- Integrate a secrets management tool that uses the Kubernetes Service account to authenticate users who need access to the secret vault.
- Integrate an IAM (Identity and Access Management) tool to allow the system to use tokens from a Secure Token Service.
- Integrate a third-party secrets manager into pods.


