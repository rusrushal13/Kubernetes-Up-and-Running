# [Chapter 4. Common Kubectl Commands](https://github.com/rusrushal13/Kubernetes-Up-and-Running-Notes/blob/master/Chapter4.md#chapter-4-common-kubectl-commands)

Kubectl is a command line tool used for creating objects and interacting with the Kubernetes API.

## Namespaces

Kubernetes uses *namespaces* to organize objects in the cluster. By default, kubectl interacts with *default* namespace but can change by using --namespace flag.

## Contexts

Setting a permanent namespace in kubectl configuration file located in `$HOME/.kube/config`. The configuration file also stores the information of cluster for authentication and finding it. They can also be used for managing different clusters and users using `--users` and `--clusters` flags

```bash
kubectl config set-context my-context --namespace=mystuff

kubectl config use-context my-context
```

## Viewing Kubernetes API Objects

Everything contained in Kubernetes is represented by a RESTful resource, these resources are *Kubernetes objects*. Each object can be called using a unique HTTP path; for example, `https://your-k8s.com/api/v1/namespaces/default/pods/my-pod` leads to the representation of a pod in the default namespace named *my-pod*. The kubectl command makes HTTP requests to these URLs to access the Kubernetes objects that reside at these paths.

```bash
kubectl get <resource-name> # listing of all resources

kubectl get <resource-name> <object-name> # specific resource, for other option use -o wide, -o json, -o yaml, --no-headers

kubectl get pods my-pod -o jsonpath --template={.status.podIP} # IP address of the pod

kubectl describe <resource-name> <object-name> # human-readable detailed  description
```

## Creating, Updating, and Destroying Kubernetes Objects

Objects are represented as JSON or YAML files which could be used to create, update, or delete objects on the kubernetes server.

```bash
kubectl apply -f obj.yaml # everything declared in the yaml file

kubectl edit <resource-name> <object-name> # for interactive edits

kubectl delete -f obj.yaml # remove the declared resources

kubectl delete <resource-name> <object-name> # another way
```

## Labeling and Annotating Objects

Labels and Annotation are tags for the objects which can be updated using `label` and `annotate` commands.

```bash
kubectl label pods bar color=red # label added to pod bar, for overwriting use --overwrite flag

kubectl label pods bar -color # will remove the label
```

## Debugging Commands

```bash
kubectl logs <pod-name> # -c for particular container if pods contains multiple containers; -f for streaming logs

kubectl exec -it <pod-name> -- bash

kubectl cp <pod-name>:/path/to/remote/file /path/to/local/file # copying around host and pod
```

`kubectl help`

or

`kubectl help command`