# [Chapter 3. Deploying a Kubernetes Cluster](https://github.com/rusrushal13/Kubernetes-Up-and-Running-Notes/blob/master/Chapter3.md#chapter-3-deploying-a-kubernetes-cluster)

After building the applications in containers, it needs to deploy into a reliable, scalable distributed system(K8s cluster). For this, there are many services provided by cloud which are quite easy to get started but sometimes it could be costly. *Minikube*(single-node cluster) tool provides a way to run cluster on local computer or VM. *Kubeadm* tool helps in running k8s on bare metal(cluster of Raspberry Pi single board computers)

## Installing Kubernetes on a Public Cloud Provider

In Google Container Service:

```bash
gcloud config set compute/zone asia-south1-b # default zone

gcloud container clusters create kuar-cluster # cluster creation

gcloud auth applicaton-default login # credentials
```
In Azure Container Service:

```bash
az group create --name=kuar --location=westus # resource-group creation

az acs create --orchestrator-type=kubernetes --resource-group=kuar --name=kuar-cluster # cluster creation

az acs kubernetes get-credentials --resource-group=kuar --name=kuar-cluster # credentials

az acs kubernetes install-cli # installing kubectl
```
In AWS(Amazon Web Services):

Till the release of book, AWS does not offer any hosted Kubernetes services, but there are many ways to do that. But now there is on Amazon Elastic Container Service for Kubernetes which does the same. 

Check out every solution here: [Pick Right Solution](https://kubernetes.io/docs/setup/pick-right-solution/)

## Installing Kubernetes Locally Using Minikube

*For local development(single-node cluster), learning, and experimentation.*

```bash
minikube start # starting the cluster locally, creates a local VM, creates a local kubectl config

minikube stop

minikube delete
```

There are other options too to run Kubernetes on Raspberry Pi if wanted to run K8s on bare metal cluster without paying a lot.

## The Kubernetes Client(*Kubectl*)

A command line tool to interact with Kubernetes API and used to manage most K8s objects such as pods, ReplicaSets, and services as well as used for exploration and verification of cluster.

Some commands for usage of Kubectl on the running cluster(most of commands are self-explanatory):

```bash
kubectl version

kubectl get componentstatuses # controller-manager regulates controller behavior; scheduler used for placing pods onto different nodes; etcd is the API object storage for the cluster

kubectl get nodes # listing nodes in the cluster

kubectl describe nodes <node name>
```

## Cluster Components

Components run in the kube-system namespace(folder in a filesystem)

* **Kubernetes Proxy**: It is responsible for routing network traffic to load-balanced services in the cluster. K8s uses *DaemonSet* which helps in achieving proxies on every node of the cluster. 

```bash
kubectl get daemonSets --namespace=kube-system kube-proxy
```

* **Kubernetes DNS**: K8s runs a DNS server for providing names and discovery of the services that are defined in the cluster. It runs as a Deployment and service in K8s for managing replicas and load-balancing.

```bash
kubectl get deployments --namespace=kube-system kube-dns

kubectl get services --namespace=kube-system kube-dns
```

* **Kubernetes UI**: Kubernetes also consists of UI dashboard(a GUI).

```bash
kubectl get deployments --namespace=kube-system kubernetes-dashboard

kubectl get services --namespace=kube-system kubernetes-dashboard

kubectl proxy # starts up a server on localhost:8001
```

This chapter was mostly about the components and object kubernetes provides.