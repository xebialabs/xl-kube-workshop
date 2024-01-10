
# Lab 0 - Setup kubectl context

## Slack 

Connect to Slack channel `#xl-kube-workshop`

## Collect all prerequisites

Check all prerequisites:

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [xl-cli 24.1.0](https://dist.xebialabs.com/public/xl-cli/24.1.0/)
  - [Install the XL CLI](https://docs.digital.ai/bundle/devops-release-version-v.24.1/page/release/how-to/install-the-xl-cli.html)
- [yq](https://github.com/mikefarah/yq)
- Java 17 - keytool (only if you plan to use the generation of the keystore inside the xl-cli kube)
- A directory from where you will run `xl kube` commands
- License files for Release and Deploy (can be generated during installation)
- Username/password for Identity Server (it will be provided on the Slack channel `#xl-kube-workshop`)

Use one of the following options to get the kubectl context on your machine:

### With the Azure cluster

Real values of parameters will be provided on the Slack channel `#xl-kube-workshop`.

Run:

```shell
export AZURE_USERNAME=xl-test-azure...
export AZURE_PASSWORD=put_password_from_1pass
export RESOURCE_GROUP=xl-kube-workshop-group-X
export CLUSTER_NAME=xl-kube-workshop-X
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
Client Version: version.Info{Major:"1", Minor:"27", GitVersion:"v1.27.2", GitCommit:"7f6f68fdabc4df88cfea2dcf9a19b2b830f1e647", GitTreeState:"clean", BuildDate:"2023-05-17T14:20:07Z", GoVersion:"go1.20.4", Compiler:"gc", Platform:"darwin/amd64"}
Kustomize Version: v5.0.1
Server Version: version.Info{Major:"1", Minor:"27", GitVersion:"v1.27.4", GitCommit:"fa3d7990104d7c1f16943a67f11b154b71f6a132", GitTreeState:"clean", BuildDate:"2023-07-19T12:14:49Z", GoVersion:"go1.20.6", Compiler:"gc", Platform:"linux/amd64"}
```

## Check `xl` command

The `xl`command version 24.1.0 should be in the path. Check this with the following command:

```shell
xl version
```

Example of the response on linux:

```text
$ xl version
CLI version:             24.1.0-1226.113
Git version:             v24.1.0-1221.113-0-gc0a91cd-dirty
Deploy API version:      xl-deploy/v1
Release API version:     xl-release/v1
Git commit:              c0a91cd72f97c4696ab016c085225e7bd63a6eca
Build date:              2023-12-26T03:36:21.262Z
GO version:              go1.21.4
OS/Arch:                 darwin/amd64
```

## Monitor Kubernetes

Our favorite tool to check what's going on is [K9s](https://k9scli.io). Launch it with

```shell
k9s
```

You will see everything that is going in the cluster! That is probably too much information. Narrow down the scope by typing `:ns`. This will allow you to select a namespace. In the subsequent exercises you will create your own namespace. This will be your private spot on the cluster where all necessary components are installed.

---

[Next](../part-1/lab-1-install-release.md)

