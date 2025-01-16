
# Lab 0 - Setup kubectl context

## Collect all prerequisites

Check all prerequisites:

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [xl-cli 24.3.x](https://dist.xebialabs.com/public/xl-cli/24.3.2/)
  - [Install the XL CLI](https://docs.digital.ai/release/docs/xl-platform/operator/xl-kube)
- [yq](https://github.com/mikefarah/yq)
- Java 17 - keytool (only if you plan to use the generation of the keystore inside the xl-cli kube)
- A directory from where you will run `xl kube` commands
- License files for Release and Deploy (can be generated during installation)
- Username/password for Identity Server (in case you cannot use IS, skip the labs with IS )

Use one of the following options to get the kubectl context on your machine:

### With the Azure cluster

Run (customize the values according to your cluster settings):

```shell
export AZURE_USERNAME=...
export AZURE_PASSWORD=...
export RESOURCE_GROUP=...
export CLUSTER_NAME=...
az login -u $AZURE_USERNAME -p $AZURE_PASSWORD 
az aks get-credentials --resource-group $RESOURCE_GROUP --name $CLUSTER_NAME --overwrite-existing
```

### With Minikube

Run:

```shell
minikube start --driver=docker --memory 22000 --cpus 8 --disk-size 60000mb
minikube addons enable ingress
minikube addons enable ingress-dns
```

After minikube start, kubectl will use the minikube context.

### With Docker Desktop

Enable Kubernetes under **Preferences > Kubernetes**.

The demo runs comfortably with the following resources allocated under **Preferences > Resources**:

* CPUs: 8
* Memory: 22 GB
* Swap: 4 GB
* Disk image size: 60 GB

Check current list of contexts, in command line run to get all contexts.

```shell
kubectl config get-contexts
```

In the list there should be docker-desktop, to use it, run following:

```shell
kubectl config use-context docker-desktop
```


## Check cluster connection for any provider

Run

```shell
kubectl cluster-info
```

and 

```shell
kubectl version
```

For example, it will return for minikube:

```text
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"30", GitVersion:"v1.30.x", ...}
Kustomize Version: v5.0.1
Server Version: version.Info{Major:"1", Minor:"30", GitVersion:"v1.30.x", ...}
```

## Check `xl` command

The `xl`command version 24.3.x should be in the path. Check this with the following command:

```shell
xl version
```

Example of the response on linux:

```text
$ xl version
CLI version:             24.3.2
Git version:             v24.3.1-0-g946951e
Deploy API version:      xl-deploy/v1
Release API version:     xl-release/v1
Git commit:              946951e4d6fc9cbd8e9ce3cd79b7f0e466f3b0a1
Build date:              2024-11-27T04:32:48.298Z
GO version:              go1.21.8
OS/Arch:                 linux/amd64
```

## Monitor Kubernetes

Our favorite tool to check what's going on is [K9s](https://k9scli.io). Launch it with

```shell
k9s
```

You will see everything that is going in the cluster! That is probably too much information. Narrow down the scope by typing `:ns`. This will allow you to select a namespace. In the subsequent exercises you will create your own namespace. This will be your private spot on the cluster where all necessary components are installed.

---

[Next](../part-1/lab-1-install-release.md)

