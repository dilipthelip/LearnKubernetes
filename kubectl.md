# KUBECTL

Kubectl is a command line tool to connect to kubrnetes cluster.  

## Download kubectl:

```
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/v1.5.1/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

### Minikube Dashboard:

```
minikube dashboard
```

### How to start the cluster?

```
minikube start
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

### How to create a pod?

Approach 1:  

The below approach will have the whole configuration available in the yml file.In this case the docker image will be pulled from the docker hub and deployed in to kubernetes.  

```
kubectl create -f first-app/helloworld.yml
```

or  

```
kubectl apply -f first-app/helloworld.yml
```

```
Example 1 helloworld.yml: Create a single pod

apiVersion: v1
kind: Pod
metadata:
  name: nodehelloworld.example.com
  labels:
    app: helloworld
spec:
  containers:
  - name: k8s-demo
    image: wardviaene/k8s-demo
    ports:
    - name: nodejs-port
        containerPort: 3000
      env:
      - name: ENVIRONMENT
        value: "dev"
      - name: JDBC_PASSWORD
        value: "xyz"

Example 2 application-api: Create a deployment with Pod replication

apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: application-api
spec:
  replicas: 5
  template:
    metadata:
      labels:
        app: application-api
    spec:
      containers:
      - name:  application-api
        image: application:1.1.0-SNAPSHOT
        imagePullPolicy: Never
        resources:
          requests:
            cpu: 100m
            memory: 256Mi
          limits:
            cpu: 200m
            memory: 512Mi
        ports:
        - containerPort: 8080
          name: store-api
        readinessProbe:
          httpGet:
            path: /
            port: 8080
          initialDelaySeconds: 10
          timeoutSeconds: 10
          failureThreshold: 5
        env:
        - name: ENVIRONMENT
          valueFrom:
          secretKeyRef:
            name: env
            key: env
        - name: JDBC_PASSWORD
          valueFrom:
            secretKeyRef:
              name: xyz-db-password
              key: db-password
  strategy:
    rollingUpdate:
      maxUnavailable: 0
      maxSurge: 2
```

Approach 2:  

Have the image then run the below command.  

```
kubectl run <anyname> --image=<imagename>:<version> --port=8080 --env="ENVIRONMENT=dev" --env="JDBC_PASSWORD=xyz"
```


### How to get the list of pods?

```
kubectl get pod

NAME                              READY     STATUS    RESTARTS   AGE
hello-minikube-3015430129-x684t   1/1       Running   1          1h
nodehelloworld.example.com        1/1       Running   0          5m
```

### How to get the list of pods with ip ?

```
kubectl get pod -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP           NODE
hello-minikube-3015430129-x684t     1/1       Running   2          2d        172.17.0.2   minikube
nodehelloworld.example.com          1/1       Running   1          1d        172.17.0.5   minikube
```

### How to get the list of pods with lables ?

```
 kubectl get pods --show-labels

NAME                              READY     STATUS             RESTARTS   AGE       LABELS
application-api-1241476493-77m2b     1/1       Running            6          38m       app=store-api,pod-template-hash=1241476493

```

### How to view the logs of a Pod?

Using the below command we can view the application start up logs.  

```
kubectl logs <pod>
```

### How to get more information about a pod ?
```
kubectl describe pod nodehelloworld.example.com
```

### How to delete a deployment/service?

Approach 1:  

```
kubectl delete -f <deployment/service>.yaml

Example:

kubectl delete -f deployment.yaml

kubectl delete -f service.yaml

```
Approach 2:  

Type the below and press enter.  
```
kubectl delete deployment store-api

Note: This will not delete a service so you will still need to run 'kubectl delete service <servicename'
```

### How to forward the request to this pod?

The below command will forward the requests that are recieved in port 8081 to port 3000.  

Approach 1:  
```
kubectl port-forward nodehelloworld.example.com 8081:3000
```

Approach 2:  
Exposing the pod as an service.  

```
kubectl expose pod <pod> --port=444 --type=NodePort --name=frontend

Example:
kubectl expose pod nodehelloworld.example.com --type=NodePort --name=node-123

Approach 3:
Exposing a service if you completed a multi pod 'Deployment'.
kubectl expose rs store-api-1241476493 --port=8080 --target-port=8080 --type=NodePort

```
We need the url of the pod.  

```
minikube service node-123 --url
```

### How to get the list of services running in Kubernetes cluster?

```
kubectl get services
```

#### How to get service details ?

Using the **NodePort** and **Endpoint** we can connect to this pod from other pod.  

```
kubectl describe service node-123

Name:			node-123
Namespace:		default
Labels:			app=helloworld
Selector:		app=helloworld
Type:			NodePort
IP:			10.0.0.197
Port:			<unset>	3000/TCP
NodePort:		<unset>	32203/TCP
Endpoints:		172.17.0.5:3000
Session Affinity:	None
```




#### How to setup secrets to use in a deployment?

Type the below and press enter.  
```
1. Create a file with a secret
echo "apassword" > password.txt

2. Setup a secret
Syntax:
kubectl create secret generic <secret name> --from-file=<key name>=./password.txt

Example:
kubectl create secret generic store-dev-db-password --from-file=db-password=./password.txt

3. View secrets setup
kubectl get secrets
kubectl describe secrets/<secretname>

Example:
kubectl describe secrets/store-dev-db-password
```

### How to execute commands in the container?

To list the files inside the container.
```
kubectl exec nodehelloworld.example.com ls /app
```

To create a file inside the container.  

```
kubectl exec nodehelloworld.example.com touch /app/test.txt
```

#### How to connect to one pod from another pod?

The below command created a new pod with a new container.  

```
kubectl run -i --tty busybox --image=busybox --restart=Never -- sh
```
The above line will take you to the server.  
```
ls

telnet 172.17.0.5 3000
```

Type the below and press enter.  
```
GET /
```

## Deployment : 

### How to get the status of the deployment ?

```
kubectl rollout status deployment/<deployment-name>
```

### How to update the image ?

```
kubectl set image deployment/<deployment-name> <container-name>=imageName
```
### How to get the history of deploymentse ?

```
kubectl rollout history deployment/<deployment name>

Example:
kubectl rollout history deployment/helloworld-deployment

deployments "helloworld-deployment"
REVISION	CHANGE-CAUSE
1		<none>
```

### How to rollout to previous deployment ?

```
kubectl rollout undo deployment/<deployment-name>

Example:
kubectl rollout undo deployment/helloworld-deployment

```
