# kubescape

## Installation  

```bash
sudo curl -s https://raw.githubusercontent.com/armosec/kubescape/master/install.sh | /bin/bash
sudo mv ~/.local/bin/kubescape /usr/local/bin
```

**Check version:**  
```bash
kubescape version
```

Kubernetes connects to your cluster using standard Kubectl config files.  
Set the `KUBECONFIG` environment variable in your shell to reference the config file for the cluster you want to scan:

```bash
export KUBECONFIG=.kube/my-cluster.yaml
```

## Output Formats  

Kubescape outputs to your terminal by default but can also produce reports in JSON or Junit format. Add the -f flag to specify your desired mode:
```bash
kubescape scan framework nsa -f json
kubescape scan framework nsa -f junit
```

**Note:**  
- JSON format is in compressed JSON format.
- JUnit format is in compressed XML format.

Output is emitted to your terminal’s standard output stream irrespective of the report format you specify.  
Add the -o flag to supply a file path to save to:

```bash
kubescape scan framework nsa -f json -o report.json
```

**Note Generate Report using this format:**  

NAMSPACE-kubescape-scan-MMDDYYYY.json

*Example:*  

MyNamespace-kubescape-scan-11182021

**Example runs:**  

```bash
# This will scan all namespaces:
kubescape scan framework nsa

# This will scan a specific namespace:
kubescape scan framework nsa --enable-host-scan --include-namespaces <MYNAMESPACE> 

# This will scan a specific namespace and send the output in JSON format to the specified file:
kubescape scan framework nsa --enable-host-scan --include-namespaces <MYNAMESPACE> -f json -o MyNamespace-kubescape-scan-11182021.json

# This will scan a specific namespace and send the output in JUnit (XML) format to the specified file:
kubescape scan framework nsa --enable-host-scan --include-namespaces <MYNAMESPACE> -f junit -o MyNamespace-kubescape-scan-11182021.xml

# This will exclude namespaces from the scan:
kubescape scan framework nsa ---enable-host-scan -exclude-namespaces kube-system,kube-public

# This will exclude namespaces from the scan output in JSON format to the specified file:
kubescape scan framework nsa --enable-host-scan --exclude-namespaces kube-system,kube-public -f json -o MyNamespace-kubescape-scan-11182021.json

# This will exclude namespaces from the scan output in JUnit (XML) to the specified file:
kubescape scan framework nsa --enable-host-scan --exclude-namespaces kube-system,kube-public -f junit -o MyNamespace-kubescape-scan-11182021.xml
```

**References**  
[kubescape](https://github.com/armosec/kubescape)  
[How To Scan](https://www.cloudsavvyit.com/14299/how-to-scan-for-kubernetes-vulnerabilities-with-kubescape)  
