# KUBECTL

Kubectl is a command line tool to connect to kubrnetes cluster.  

## Download kubectl:

```
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/v1.5.1/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
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
