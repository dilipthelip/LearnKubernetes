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

```
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/v1.5.1/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

## What is a pod?

- A pod describes an application running in kubernetes.
- A pod can contain one or more tightly coupled containers that make up the app.

## Kubernetes Architecture:

![](https://github.com/dilipthelip/LearnKubernetes/blob/master/images/architecture.png)
