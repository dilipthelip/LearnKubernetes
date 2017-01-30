# LearnKubernetes

## What is Kubernetes?

- It is an opensource orchestration system for docker containers.
- Kubernetes is a platform that will manage the containers for you.
- Kubernetes can start with one node and can go up to thousands of nodes.

## How to SetUp Kubernetes?

- It should be able to run anywhere.
- **Minikube** to quickly spin up a local single machine with a Kubernetes Cluster. 

### MiniKube SetUp:
- It is a tool that makes it easy to run Kubernetes locally.
- It runs a single-node Kubernetes cluster inside a Linux VM.
- MiniKube is appropriae for development and testing.

Download Kubernetes from the below link.  

[Kubernetes Download](https://github.com/kubernetes/minikube/releases)

#### kubectl

- **kubectl** is a command line interface for running commands against Kubernetes clusters.

**Install kubectl** by running the below command in mac.  

```
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/v1.5.1/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

Kubectl commands are available in the below link.  

[KUBECTL](https://github.com/dilipthelip/LearnKubernetes/blob/master/kubectl.md)

#### KOPS:

- kops : stands for Kubernetes Operations.
- The tool allows you to do production grade Kubernetes installations, upgrades and management.
- We can use the **kops** to set up Kubernetes cluster on AWS.
- This only works on MAC/Linux.


## What is a pod?

- A pod describes an application running in kubernetes.
- A pod can contain one or more tightly coupled containers that make up the app.

## Kubernetes Architecture:

![](https://github.com/dilipthelip/LearnKubernetes/blob/master/images/architecture.png)

- Each node has a docker installed in it to run the docker images.  
- **kubelet :** It is responsible to launch the pods.It will connect to the master node to get this information.
- **kubeproxy :** This feeds information to the iptables about the pods.Whenever a new pod is added, kubeproxy updates the iptables with the new pod.  
- **iptable :** This is the firewall in linux to route traffic.
- **service :** The service layer acts as a load balancer and its publicly available. It routes the traffic to the kubernetes cluster. Service will have all the nodes listed in it. This forwards the request to the kubernetes cluster and then to the ip tables.IPtables then runs the rule and determines which pod and which container to connect to.

