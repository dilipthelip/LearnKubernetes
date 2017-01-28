# KUBECTL

Kubectl is a command line tool to connect to kubrnetes cluster.  

## Download kubectl:

```
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/v1.5.1/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

### How to start the cluster?

```
minikube start
```

### Run command: 

```
kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080
```

### Expose command: 

```
kubectl expose deployment hello-minikube --type=NodePort`
```

### Service command:

```
minikube service hello-minikube --url
```

### How to stop the cluster?

```
minikube stop
```

## Pod Commands:

```
kubectl get pod : get information about the pod.

kubectl describe pod <pod> : Describe one pod.

kubectl expose pod <pod> --port=444 --name=frontend : Expose the port of a pod

kubectl port-forward <pod> 8080 : Port forward the exposed pod port to your local machine.

kubectl attach <podname> i : Attach to pod

kubectl exec <pod> --command : Execute a command on pod.

kubectl pods <pod> mylabel=awesome  : Add a new label to the pod.

kubectl run -i --tty busybox --image=busybox --restart=Never -- sh  : Run a shell in a pod - very useful for debugging.
```
