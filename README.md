# kubernetes-tutorial
There are three ways to work with Kubernetes:<br>
1. Play with Kubernetes (https://labs.play-with-k8s.com/)
2. Kubernetes Adm (Master & Worker node)
3. Minikube

# Minikube Installation on Ubuntu
Minikube is an open source tool that allows you to set up a single-node Kubernetes cluster on your local machine. The cluster is run inside a virtual machine and includes Docker, allowing you to run containers inside the node.

# Minikube Installation Guide for Ubuntu

This guide provides step-by-step instructions for installing Minikube on Ubuntu. Minikube allows you to run a single-node Kubernetes cluster locally for development and testing purposes.

## Pre-requisites

* Ubuntu OS
* sudo privileges
* Internet access
* Virtualization support enabled (Check with `egrep -c '(vmx|svm)' /proc/cpuinfo`, 0=disabled 1=enabled) 

## Step 1: Update System Packages
Update your package lists to make sure you are getting the latest version and dependencies.

```bash
sudo apt update
```
## Step 2: Install Required Packages
Install some basic required packages.

```bash
sudo apt install -y curl wget apt-transport-https
```
## Step 3: Install Docker
Minikube can run a Kubernetes cluster either in a VM or locally via Docker. This guide demonstrates the Docker method.

```bash
sudo apt install -y docker.io
```

Start and enable Docker.

```bash
sudo systemctl enable --now docker
```

Add current user to docker group (To use docker without root)

```bash
sudo usermod -aG docker $USER && newgrp docker
```
Now, logout (use `exit` command) and connect again.

## Step 4: Install Minikube
First, download the Minikube binary using `curl`:

```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```

Make it executable and move it into your path:
```bash
chmod +x minikube
sudo mv minikube /usr/local/bin/
```

## Step 5: Install kubectl
Download kubectl, which is a Kubernetes command-line tool.

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
**Check above image **
Make it executable and move it into your path:

```bash
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

## Step 6: Start Minikube
Now, you can start Minikube with the following command:

```bash
minikube start --driver=docker
```

This command will start a single-node Kubernetes cluster inside a Docker container.

## Step 7: Check Cluster Status
Check the cluster status with:

```bash
minikube status
```
You can also use `kubectl` to interact with your cluster:

```bash
kubectl get nodes
```

## Step 8: Stop Minikube
When you are done, you can stop the Minikube cluster with:

```bash
minikube stop
```

## Optional: Delete Minikube Cluster
If you wish to delete the Minikube cluster entirely, you can do so with:

```bash
minikube delete
```

That's it! You've successfully installed Minikube on Ubuntu, and you can now start deploying Kubernetes applications for development and testing

# Cluster Management and Context
Cluster management refers to querying information about the K8S cluster itself.

kubectl cluster-info – Display endpoint information about the master and services in the cluster.

kubectl version – Display the Kubernetes version running on the client and server.

kubectl config view – Get the configuration of the cluster.

kubectl config view -o jsonpath='{.users[*].name}' – Get a list of users.

kubectl config current-context – Display the current context.

kubectl config get-contexts – Display a list of contexts.

kubectl config use-context <cluster name> – Set the default context.

kubectl api-resources – List the API resources that are available.

kubectl api-versions – List the API versions that are available.

-A – List pods, services, daemonsets, deployments, replicasets, statefulsets, jobs, and CronJobs in all namespaces, not custom resource types. Note the alias for --all-namespaces is -A

kubectl get all --all-namespaces

# DaemonSet
A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

kubectl get daemonset – List one or more daemonsets.

kubectl edit daemonset <daemonset_name> – Edit and update the definition of one or more daemonset.

kubectl delete daemonset <daemonset_name> – Delete a daemonset.

kubectl create daemonset <daemonset_name> – Create a new daemonset.

kubectl rollout daemonset – Manage the rollout of a daemonset.

kubectl describe ds <daemonset_name> -n <namespace_name> – Display the detailed state of daemonsets within a namespace

# Deployment
A Deployment provides declarative updates for Pods and ReplicaSets. You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments. See StatefulSet vs. Deployment.

kubectl get deployment – List one or more deployments.

kubectl describe deployment <deployment_name> – Display the detailed state of one or more deployments.

kubectl edit deployment <deployment_name> – Edit and update the definition of one or more deployments on the server.

kubectl create deployment <deployment_name> – Create a new deployment.

kubectl delete deployment <deployment_name> – Delete deployments.

kubectl rollout status deployment <deployment_name> – See the rollout status of a deployment.

kubectl set image deployment/<deployment name> <container name>=image:<new image version> – Perform a rolling update (K8S default), set the image of the container to a new version for a particular deployment.

kubectl rollout undo deployment/<deployment name> – Rollback a previous deployment.

kubectl replace --force -f <configuration file> – Perform a replace deployment — Force replace, delete and then re-create the resource.

Read more about Kubernetes deployment strategies: Different Types of Kubernetes Deployment Strategies (Examples)

# Events
kubectl get events – List recent events for all resources in the system.

kubectl get events --field-selector type=Warning – List Warnings only.

kubectl get events --sort-by=.metadata.creationTimestamp – List events sorted by timestamp.

kubectl get events --field-selector involvedObject.kind!=Pod – List events but exclude Pod events.

kubectl get events --field-selector involvedObject.kind=Node, involvedObject.name=<node_name> – Pull events for a single node with a specific name.

kubectl get events --field-selector type!=Normal – Filter out normal events from a list of events.

# Logs
Logs – System component logs record events happening in cluster, which can be very useful for debugging. You can configure log verbosity to see more or less detail. Logs can be as coarse-grained as showing errors within a component, or as fine-grained as showing step-by-step traces of events (like HTTP access logs, pod state changes, controller actions, or scheduler decisions).

kubectl logs <pod_name> – Print the logs for a pod.

kubectl logs --since=6h <pod_name> – Print the logs for the last 6 hours for a pod.

kubectl logs --tail=50 <pod_name> – Get the most recent 50 lines of logs.

kubectl logs -f <service_name> [-c <$container>] – Get logs from a service and optionally select which container.

kubectl logs -f <pod_name> – Print the logs for a pod and follow new logs.

kubectl logs -c <container_name> <pod_name> – Print the logs for a container in a pod.

kubectl logs <pod_name> pod.log – Output the logs for a pod into a file named ‘pod.log’.

kubectl logs --previous <pod_name> – View the logs for a previously failed pod.

# Namespaces
Namespaces – In Kubernetes, namespaces provides a mechanism for isolating groups of resources within a single cluster. Names of resources need to be unique within a namespace, but not across namespaces. Namespace-based scoping is applicable only for namespaced objects (e.g. Deployments, Services, etc) and not for cluster-wide objects (e.g. StorageClass, Nodes, PersistentVolumes, etc).

kubectl create namespace <namespace_name> – Create a namespace.

kubectl get namespace <namespace_name> – List one or more namespaces.

kubectl describe namespace <namespace_name> – Display the detailed state of one or more namespaces.

kubectl delete namespace <namespace_name> – Delete a namespace.

kubectl edit namespace <namespace_name> – Edit and update the definition of a namespace.

kubectl top namespace <namespace_name> – Display Resource (CPU/Memory/Storage) usage for a namespace.

# Nodes
Nodes – Kubernetes runs your workload by placing containers into Pods to run on Nodes. A node may be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods. Typically you have several nodes in a cluster; in a learning or resource-limited environment, you might have only one node. The components on a node include the kubelet, a container runtime, and the kube-proxy.

kubectl taint node <node_name> – Update the taints on one or more nodes.

kubectl get node – List one or more nodes.

kubectl delete node <node_name> – Delete a node or multiple nodes.

kubectl top node <node_name> – Display Resource usage (CPU/Memory/Storage) for nodes.

kubectl get pods -o wide | grep <node_name> – Pods running on a node.

kubectl annotate node <node_name> – Annotate a node.

kubectl cordon node <node_name> – Mark a node as unschedulable.

kubectl uncordon node <node_name> – Mark node as schedulable.

kubectl drain node <node_name> – Drain a node in preparation for maintenance.

kubectl label node – Add or update the labels of one or more nodes.

# Pods
Pods – Pods are the smallest deployable units of computing that you can create and manage in Kubernetes. A Pod (as in a pod of whales or pea pod) is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers. A Pod’s contents are always co-located and co-scheduled, and run in a shared context. A Pod models an application-specific “logical host”: it contains one or more application containers which are relatively tightly coupled. In non-cloud contexts, applications executed on the same physical or virtual machine are analogous to cloud applications executed on the same logical host. As well as application containers, a Pod can contain init containers that run during Pod startup. You can also inject ephemeral containers for debugging if your cluster offers this.

kubectl get pod – List one or more pods.

kubectl get pods --sort-by='.status.containerStatuses[0].restartCount' – List pods Sorted by Restart Count.

kubectl get pods --field-selector=status.phase=Running – Get all running pods in the namespace.

kubectl delete pod <pod_name> – Delete a pod.

kubectl describe pod <pod_name> – Display the detailed state of a pods.

kubectl create pod <pod_name> – Create a pod.

kubectl exec <pod_name> -c <container_name> <command> – Execute a command against a container in a pod. Read more: Using Kubectl Exec: Connect to Your Kubernetes Containers

kubectl exec -it <pod_name> /bin/sh – Get an interactive shell on a single-container pod.

kubectl top pod – Display Resource usage (CPU/Memory/Storage) for pods.

kubectl annotate pod <pod_name> <annotation> – Add or update the annotations of a pod.

kubectl label pods <pod_name> new-label=<label name> – Add or update the label of a pod.

kubectl get pods --show-labels – Get pods and show labels.

kubectl port-forward <pod name> <port number to listen on>:<port number to forward to> – Listen on a port on the local machine and forward to a port on a specified pod.

# Replication Controllers
Replication Controllers – Note: A Deployment that configures a ReplicaSet is now the recommended way to set up replication. A ReplicationController ensures that a specified number of pod replicas are running at any one time. In other words, a ReplicationController makes sure that a pod or a homogeneous set of pods is always up and available.

kubectl get rc – List the replication controllers.

kubectl get rc --namespace=”<namespace_name>” – List the replication controllers by namespace.

# ReplicaSets
ReplicaSets – A ReplicaSet’s purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods.

kubectl get replicasets – List ReplicaSets.

kubectl describe replicasets <replicaset_name> – Display the detailed state of one or more ReplicaSets.

kubectl scale --replicas=[x] – Scale a ReplicaSet.

# Secrets
Secrets – A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Pod specification or in a container image. Using a Secret means that you don’t need to include confidential data in your application code.

kubectl create secret – Create a secret.

kubectl get secrets – List secrets.

kubectl describe secrets – List details about secrets.

kubectl delete secret <secret_name> – Delete a secret.

# Services
Services – An abstract way to expose an application running on a set of Pods as a network service. With Kubernetes you don’t need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.

kubectl get services – List one or more services.

kubectl describe services – Display the detailed state of a service.

kubectl expose deployment [deployment_name] – Expose a replication controller, service, deployment, or pod as a new Kubernetes service.

kubectl edit services – Edit and update the definition of one or more services.

# Service Accounts
Service Accounts – A service account provides an identity for processes that run in a Pod.

Note: This document is a user introduction to Service Accounts and describes how service accounts behave in a cluster set up as recommended by the Kubernetes project. Your cluster administrator may have customized the behavior in your cluster, in which case this documentation may not apply.

When you (a human) access the cluster (for example, using kubectl), you are authenticated by the apiserver as a particular User Account (currently this is usually admin, unless your cluster administrator has customized your cluster). Processes in containers inside pods can also contact the apiserver. When they do, they are authenticated as a particular Service Account (for example, default).

kubectl get serviceaccounts – List service accounts.

kubectl describe serviceaccounts – Display the detailed state of one or more service accounts.

kubectl replace serviceaccount – Replace a service account.

kubectl delete serviceaccount <service_account_name> – Delete a service account.

# StatefulSet
StatefulSet – StatefulSet is the workload API object used to manage stateful applications. Manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods.

Like a Deployment, a StatefulSet manages Pods that are based on an identical container spec. Unlike a Deployment, a StatefulSet maintains a sticky identity for each of their Pods. These pods are created from the same spec, but are not interchangeable: each has a persistent identifier that it maintains across any rescheduling.

If you want to use storage volumes to provide persistence for your workload, you can use a StatefulSet as part of the solution. Although individual Pods in a StatefulSet are susceptible to failure, the persistent Pod identifiers make it easier to match existing volumes to the new Pods that replace any that have failed.

kubectl get statefulset – List StatefulSet

kubectl delete statefulset/[stateful_set_name] --cascade=false – Delete StatefulSet only (not pods).
