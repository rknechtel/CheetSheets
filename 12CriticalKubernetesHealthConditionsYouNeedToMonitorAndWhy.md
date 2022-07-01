
# 12 Critical Kubernetes Health Conditions You Need to Monitor and Why

Kubernetes can generate millions of new metrics every day. One of the most challenging aspects of monitoring your cluster’s health is filtering through which of these metrics are important to collect and pay attention to.

The best way to approach this challenge is to first understand which high-value signals just about every Kubernetes operator should be concerned with. This is the approach one of our customers, [Major League Baseball, took](https://thenewstack.io/lessons-from-major-league-baseball-on-deploying-and-monitoring-kubernetes/). By focusing on the core, actionable issues they care about, they filter out about 90% of the Kubernetes data they generate.

In this article, I will define the 12 critical Kubernetes health conditions that you should monitor and create alerts for. Your organization’s list may differ slightly, but these 12 are a good starting point when developing your organization’s Kubernetes monitoring strategy.

## 1. Crash Loops  

A crash loop is when a pod starts, crashes, and then keeps trying to restart but can’t (it keeps crashing and restarting in a loop). This is obviously bad, because when this happens your application cannot run. This can be caused by an application within the pod crashing, or it can be caused by a misconfiguration in the pod or the deployment process, which makes debugging a crash loop rather tricky. When a crash loop happens, you need to know immediately — so that you can figure out what’s happening and whether you need to take emergency measures to keep your application available.  

## 2. CPU Utilization  

CPU utilization is simply how many CPU cycles your nodes are using. This is important to monitor for two reasons: first, you don’t want to run out of processing resources for your application. If your application becomes CPU-bound, you need to increase your CPU allocation or add more nodes to the cluster. Second, you don’t want your CPU to sit there unused. If your CPU usage is consistently low, you may have over-allocated your resources and are potentially wasting money.  

## 3. Disk Pressure  

Disk pressure is a condition indicating that a node is using too much disk space or is using disk space too fast, according to the thresholds you have set in your Kubernetes configuration. This is important to monitor because it might mean that you need to add more disk space, if your application legitimately needs more space. Or it might mean that an application is misbehaving and filling up the disk prematurely in an unanticipated manner. Either way, it’s a condition which needs your attention.  

## 4. Memory Pressure  

Memory pressure is another resourcing condition indicating that your node is running out of memory. Similar to CPU resourcing, you don’t want to run out of memory — but you also don’t want to over-allocate memory resources and waste money. You especially need to watch for this condition because it could mean there’s a memory leak in one of your applications.  

## 5. PID Pressure  

PID pressure is a rare condition where a pod or container spawns too many processes and starves the node of available process IDs. Each node has a limited number of process IDs to distribute amongst running processes; and if it runs out of IDs, no other processes can be started. Kubernetes lets you set PID thresholds for pods to limit their ability to perform runaway process-spawning, and a PID pressure condition means that one or more pods are using up their allocated PIDs and need to be examined.  

## 6. Network Unavailable  

All your nodes need network connections, and this status indicates that there’s something wrong with a node’s network connection. Either it wasn’t set up properly (due to route exhaustion or a misconfiguration), or there’s a physical problem with the network connection to your hardware.  

## 7. Job Failures  

Jobs are designed to run pods for a limited amount of time and tear them down when they’ve completed their intended functions. If a job doesn’t complete successfully due to a node crashing or being rebooted, or due to resource exhaustion, you need to know that the job failed. That’s why you need to monitor job failures — they don’t usually mean your application is inaccessible, but if left unfixed it can lead to problems down the road.  

## 8. Persistent Volume Failures  

Persistent Volumes are storage resources that are specified on the cluster and are available as persistent storage to any pod which requests it. During their lifecycle, they are bound to a pod and then reclaimed when no longer needed by that pod. If that reclamation fails for whatever reason, you need to know that there’s something wrong with your persistent storage.  

## 9. Pod Pending Delays  

During a pod’s lifecycle, its state is “pending” if it’s waiting to be scheduled on a node. If it’s stuck in the “pending” state, it usually means there aren’t enough resources to get the pod scheduled and deployed. You will need to either update your CPU and memory allocations, remove pods, or add more nodes to your cluster.  

## 10. Deployment Glitches  

Deployments are used to manage stateless applications — where the pods are interchangeable and don’t need to be able to reach any specific single pod, but rather just a particular type of pod. You need to keep an eye on your Deployments to make sure they finish properly. The best way is to make sure the number of Deployments observed matches the number of Deployments desired. If there’s a mismatch, then one or more Deployments failed.  

## 11. StatefulSets Not Ready  

StatefulSets are used to manage stateful applications, where the pods have specific roles and need to reach other specific pods; rather than just needing a particular type of pod, as with deployments. Monitoring is the same, though — you need to make sure that the number of StatefulSets observed matches the number of StatefulSets desired. If there’s a mismatch, then one or more StatefulSets has failed.  

## 12. DaemonSets Not Ready  


DaemonSets are used to manage the services or applications that need to be run on all nodes in a cluster. If you have a log collection daemon or monitoring service that you want to run on every node, you’ll want to use a DaemonSet. Monitoring is the same as for deployments: you need to make sure that the number of DaemonSets observed matches the number of DaemonSets desired. If there’s a mismatch, then one or more DaemonSets has failed.  

Like most aspects of Kubernetes, monitoring Kubernetes health can be a complex and challenging process. It’s easy to get overwhelmed. By understanding the high-value health conditions you need to be most concerned with, you can at least begin to put a strategy in place that enables you to filter out a lot of the data noise your clusters create and feel more confident that you’re tackling the issues most important to ensuring a good experience.  
