
# Recommended Kubernetes Best Practices for Production  

---

## High Availability  

- Configure Liveness and Readiness Probes (for monitoring)
  + Liveness probe is the Kubernetes equivalent of "have you tried turning it off and on again". Liveness probes detect containers that are not able to recover from failed states and restart them. It is a great tool to build-in auto recovery into production Kubernetes deployments. You can create liveness probes based on kubelet, http or tcp checks.  
  + Readiness probes detect whether a container is temporarily unable to receive traffic and will mitigate these situations by stopping traffic flow to it. Readiness probes will also detect whether new pods are ready to receive traffic, before allowing traffic flow, during deployment updates.  

- Provision at Least 3 Master Nodes
- Replicate Master Nodes in Odd Numbers
- Setup isolated `etcd` replicas on dedicated nodes
- Develop a Plan for Regular `etcd` Backups
- Distribute Master Nodes across Zones
- Distribute Worker Nodes across Zones
  + Worker nodes should also be distributed across availability zones.  
- Configure Autoscaling for Both Master and Worker Nodes
  + When using the cloud, the best practice is to place worker nodes in autoscaling groups. Autoscaling groups will automatically bring up a node in the event of termination. 
- Bake-in HA Load Balancing
- Configure Active-Passive Setup for Scheduler and Controller Manager
- Configure the Correct Number of Pod Replicas for High Availability
  + To ensure highly available Kubernetes workloads, pods should also be replicated using Kubernetes controllers like ReplicaSets, Deployments and Statefulsets.
  + Both deployments and stateful-sets are central to the concept of high availability and will ensure that the desired number of pods is always maintained. The number of replicas is usually dictated by application requirements.
  + Kubernetes does recommend using Deployments over Replicasets for pod replication since they are declarative and allow you to roll back to previous versions easily. However, if your use-case requires custom updates orchestration or does not require updates at all, you can still use Replicasets.
- Avoid Pods being placed into a single node.
- Avoid Spinning up Naked Pods
  Are all your pods part of a Replicaset or Deployment? Naked pods are not re-scheduled in case of node failure or shut down. Therefore, it is best practice to always spin up pods as part of a Replicaset or Deployment.
- Setup Federation for Multiple Clusters
- Configure Heartbeat and Election Timeout Intervals for `etcd` Members
- Setup Ingress controller and/or API Gateway
  + Ingress allows HTTP and HTTPS traffic from the outside internet to services inside the cluster. Ingress can also be used for load balancing, terminating SSL and to give services externally reachable URLs.
  + In order for ingress to work, your cluster needs an ingress controller. Kubernetes officially supports GCE and Nginx controller as of now.
	
---

## Resource Management  

- Segregate the Production Kubernetes Cluster from DEV/UA(physical or logical) and configure usage limits.
- Configure resource requests and limits for containers
  + Resource requests and limits help you manage resource consumption by individual containers. Resource requests are a soft limit on the amount of resources that can be consumed by individual containers. Limits are the maximum amount of resources that can be consumed.
  + Resource requests and limits can be set for CPU, memory and ephemeral storage resources. Setting resource requests and limits is a Kubernetes best practice and will help avoid containers getting throttled due to lack of resources or going berserk and hogging resources.
  + Memory limits and requests for all containers
  + CPU request set to 1 CPU or below
- Create separate namespaces for your business units and teams
  + Kubernetes namespaces are virtual partitions of your Kubernetes clusters. It is recommended best practice to create separate namespaces for individual teams, projects or customers. Examples include Dev, production, frontend etc. You can also create separate namespaces based on custom application or organizational requirements.
- Configure default resource requests and limits for namespaces
  + Default requests and limits specify the default values for memory and CPU resources for all containers inside a namespace. In situations where resource request and limit values are not specifically defined for a container created inside a namespace with default values, that container will automatically inherit the default values. Configuring default values on a namespace level is a best practice to ensure that all containers created inside that namespace get assigned both request and limit values.
- Configured limit ranges for namespaces
  + Limit ranges also work on the namespace level and allow us to specify the minimum and maximum CPU and memory resources that can be consumed by individual containers inside a namespace.
  + Whenever a container is created inside a namespace with limit ranges, it has to have a resource request value that is equal to or higher than the minimum value we defined in the limit range. The container also has to have both CPU and memory limits that are equal to lower than the max value defined in the limit range.
- Specified Resource Quotas for namespaces?
  + Resource quotas also work on the namespace level and provide another layer of control over cluster resource usage. Resource Quotas limit the total amount of CPU, memory and storage resources that can be consumed by all containers running in a namespace.
  + Consumption of storage resources by persistent volume claims can also be limited based on individual storage class. Kubernetes administrators can define storage classes based on the quality of service levels or backup policies.
- Configured pod and API Quotas for namespaces?
  + Pod quotas allow you to restrict the total number of pods that can run inside a namespace. API quotas let you set limits for other API objects like PersistentVolumeClaims, Services and ReplicaSets.
  + Pod and API quotas are a good way to manage resource usage on a namespace level.
- Attach labels to Kubernetes objects
  + Resource Tagging - Defined labels for : technical, business, security
  + Labels allow Kubernetes objects to be queried and operated upon in bulk. They can also be used to identify and organize Kubernetes objects into groups. As such defining labels should figure right at the top of any Kubernetes best practices list. 
  + [Here](https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/#labels) is a list of recommended Kubernetes labels that should be defined for every deployment.
- Limit the number of pods that can run on a node
  + This will help avoid scenarios where rogue or misconfigured jobs create pods in such large numbers as to overwhelm system pods.
- Reserve compute resources for system daemons
  + Another best practice is to reserve resources for system daemons that are needed by both the OS and Kubernetes itself to run. All three resource types CPU, memory and ephemeral storage resources can be reserved for system daemons. Once reserved these resources are deducted from node capacity and are exposed as node allocable resources. Below are kubelet flags that can be used to reserve resources for system daemons:
  + `–kube-reserved`: allows you to reserve resources for Kubernetes system daemons like the kubelet, container runtime and node problem detector.
  + `–system-reserved`: allows you to reserve resources for OS system daemons like sshd and udev.
- Configure out of resource handling
  + Make sure you configure out of resource handling to prevent unused images and dead pods and containers taking up too much unnecessary space on the node.
  + Out of resource handling specifies Kubelet behaviour when the node starts to run low on resources. In such cases, the Kubelet will first try to reclaim resources by deleting dead pods (and their containers) and unused images. If it cannot reclaim sufficient resources, it will then start evicting pods.
  + You can influence when the Kubelet kicks into action by configuring eviction thresholds for eviction signals.
  + Thresholds can be configured for `nodefs.available`, `nodefs.inodesfree`, `imagefs.available` and `imagefs.inodesfree` eviction signals in the pod spec.
- Namespace(s) have a LimitRange.

---
## Storage Management  

- Use Cloud provider recommended settings for Persistent Volumes
- Include Persistent Volume Claims in the config and never use Persistent Volumes
- Create a default storage class
- Give the user the option of providing a storage class name
- Enable log rotation

---

## Security  

- Use the latest Kubernetes stable GA version
  + Kubernetes regularly pushes out new versions with critical bug fixes and new security features. Make sure you have upgraded to the latest version to take advantage of these features.
- Enable RBAC (Role-Based Access Control)
  + Kubernetes RBAC allows you to regulate access to your Kubernetes environment. Using RBAC Kubernetes admins can define policies authorizing user access as well as the extent of this access to resources. Make sure to define all four high-level API objects including Role, Cluster role, Role binding and Cluster role binding to secure your Kubernetes environment.
- Follow user access best practices
  + Make sure you limit the scope of user permissions by chopping up your Kubernetes environment into separate namespaces. This will isolate resources and help contain the damage in case of security misconfigurations or malicious activity. You can do this by using Roles (which grant access to resources within a single namespace) as opposed to ClusterRoles (which grant access to resources cluster-wide).
  + Another best practice is to limit user permissions based on the resources they need access to. For example, you can limit a Role to a specific namespace and a set of resources e.g. pods.
  + Avoid giving admin access as much as possible.
- Enable audit logging
  + Kubernetes audits are performed by the kube-api server and allow you to record a sequence of the activities that users or other system components perform. You should also be monitoring audit logs to proactively react to malicious activity or authorization failures.
  + You can define rules about which events to record and what data to log in an audit policy. Here is a minimal audit policy which will log all metadata related to requests
- Enable Kubernetes logging
  + Log all requests at the Metadata level. `apiVersion: audit.k8s.io/v1kind: Policyrules:- level: Metadata`
  + You can implement logging at Request level which will log both metadata and request body as well as RequestResponse level which will log response body in addition to request metadata and request body.
  + Kubernetes logs will help you understand what is happening inside your cluster as well as debug problems and monitor activity. Logs for containerized applications are usually written to the standard output and standard error streams.
  + A best practice when implementing Kubernetes logging is to configure a separate lifecycle and storage for logs from pods, containers and nodes. 
  + You can do this by implementing a [cluster level logging architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/#cluster-level-logging-architectures).
- Set Up a Bastion host for controlled access
- Choose a Network plugin and configured network policies
- Enable data encryption at rest
  + Encrypting data at rest is another security best practice. In Kubernetes, data can be encrypted using either of these four providers: `aescbc`, `secretbox`, `aesgcm` or `kms`.
  + Encryption can be enabled by passing the `–encryption-provider-config` flag to `kube-apiserver` process.
- Disable default service account
  + All newly created pods and containers without a service account are automatically assigned the default service account.
  + The default service account has a very wide range of permissions in the cluster and should therefore be disabled.
  + You can do this by setting `automountServiceAccountToken: false` on the service account.
- Scan containers for security vulnerabilities
  + Another security best practice is to scan your container images for known security vulnerabilities.
  + You can do this using open source tools like Anchore and Clair which will help you identify common vulnerabilities and exposures (CVEs) and mitigate them.
- Kubscape Scan(s)
  + Keep security vulnerabilities and attack surfaces to a minimum.
  + Ensure that the applications you are running are secure and that the data you are storing, whether it's your own or your clients, is secured against attack. In addition to this, be aware of security breaches within your own development environments. And because Kubernetes is a rapidly growing open source project, you’ll also need to be on top of the updates and patches so that they can be applied in a timely manner.
  + Lockdown the pods and nodes, with traceable break-glass policies 
- Configure security context for pods, containers and volumes
  + Security context specifies privilege and access control settings for pods and containers. Pod security context can be defined by including the securityContext field in the pod specification. Once a security context has been specified for a pod it automatically propagates to all the containers that belong to the pod.
  + A best practice when setting the security context for a pod is to set both `runAsNonRoot` and `readOnlyRootFileSystem` fields to true and `allowPriviligeEscalation` to false. This will introduce more layers into your container and Kubernetes environment and prevent privilege escalation
- Configured Kubernetes secrets?
  + Sensitive information related to your Kubernetes environment like a password, token or a key should always be stored in a Kubernetes secrets object.
- Provide secret/keystore with self-service provisioning & updates for infrastructure and applications

---

## Scalability  

* Configure the horizontal autoscaler for deployed pods and replicasets
  + The horizontal pod auto-scaler (HPA) automatically scales the number of pods in a deployment or replica set based on CPU utilization or custom metrics.
- Configure vertical pod autoscaler
  + The vertical pod auto-scaler (VPA) automatically sets resource requests and limits for containers and pods based on resource utilization metrics.
  + The VPA can change resource limits and requests and can do this for new pods as well as existing pods.
- Configure cluster autoscaler
  + The cluster autoscaler (CA) automatically scales cluster size based on two signals; whether there are any pending pods as well as the utilization of nodes.
  + If the CA detects any pending pods during its periodic checks, it requests more nodes from the cloud provider. The CA will also downscale the cluster and remove idle nodes if they are underutilized.

---

## Monitoring, Alerting, Logging & Analysis  

- Set up a monitoring pipeline for Kubernetes infrastructure and deployed pods
  + There are a number of open source monitoring tools that you can use to monitor your Kubernetes clusters.
  + Prometheus + Grafana is one of the most widely used monitoring toolsets among DevOps.
- Select a list of metrics to monitor
  + Setting up a metrics pipeline also involves identifying a list of metrics that you want to track.
  + In the context of resource management, most useful metrics to track include usage and utilization metrics for CPU, memory and filesystem. These can be tracked on many different levels of abstraction from clusters and namespaces, to pods and nodes.
- Integrate with other Enterprise tools sets, if any
- Setup alerting and self-healing with threshold.
- Store both infrastructure and application logs in centralized logging framework with indexing and RBAC
- Setup alerting, report/summary generation and archival based on the logs collected
- Setup Log rotation at application level to reduce the storage growth and avoid performance issues

---

## CI/CD  

- Implement Secure CI/CD pipelines for Continuous Delivery
- Enable GitOps with approval workflow to have traceability
- Test, integrate and scan for vulnerabilities (Kubescape, Clair)
- Build and deposit container artifacts to the Enterprise registry
- Tag the artifacts with Git commit SHA to enable auditability
- Adopt rolling and/or blue-green deployment models to avoid downtime

---

## Other Items  

- Containerized Application(s) do not shut down on SIGTERM, but gracefully terminate connections.
- Run an end-to-end (e2e) test
  + End-to-end tests are a great way to ensure that your Kubernetes environment will behave in a consistent and reliable manner when pushed into production. End-to-end tests will also enable developers to identify bugs before pushing their application out to end users.
  + You can run an e2e test by installing kubetest:  
    `go get -u k8s.io/test-infra/kubetest kubetest –build –up –test –down`
- Mapped external services
  + Another best practice is to provision a DNS server as a cluster add-on. A DNS server is the recommended method for service discovery in Kubernetes. The DNS server will monitor theKubernetes API for new Services and will create a set of DNS entries for each. Kubernetes recommends CoreDNS to be installed as a DNS server
  + The DNS add-on makes it easier for pods to connect to services by doing a DNS query for the servicename or for the servicename.namespacename.
	
---

## References  

https://learnk8s.io/production-best-practices  
https://www.replex.io/blog/kubernetes-in-production-readiness-checklist-and-best-practices  
https://www.ecloudcontrol.com/kubernetes-production-readiness-checklist  
https://www.weave.works/blog/production-ready-checklist-kubernetes  
https://www.ecloudcontrol.com/kubernetes-production-readiness-checklist-for-enterprise  
https://www.umamahesh.net/kubernetes-production-readiness-and-best-practices-checklist  
https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/#labels  
https://kubernetes.io/docs/concepts/cluster-administration/logging/#cluster-level-logging-architectures  
https://www.redhat.com/en/topics/containers/what-is-clair  
https://aws.amazon.com/blogs/compute/scanning-docker-images-for-vulnerabilities-using-clair-amazon-ecs-ecr-aws-codepipeline  
https://docs.aws.amazon.com/AmazonECR/latest/userguide/image-scanning.html  
https://github.com/quay/clair  
https://github.com/quay/clair/releases  
https://github.com/armosec/kubescape  
https://portal.armo.cloud  
