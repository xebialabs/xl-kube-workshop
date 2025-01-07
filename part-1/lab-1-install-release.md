
# Lab 1 - Install Digital.ai Release 24.3.2

This lab will use `xl kube install` to install Digital.ai Release to the K8s cluster.
After installation we will check if everything is running properly and then configure the hostname in DNS.

Installation on Kubernetes is operator-based. We first install our custom Digital.ai Release or Deploy Operator into Kubernetes. Then we configure the application and the yaml configuration files are applied. The Operator takes care of creating the necessary resources and configuring Release. Currently the Operator uses Helm charts under the hood, but this may change in the future.

## Introducing `xl kube`

Throughout this workshop , we will use the new `xl kube` command. It can do the following:

* Install a Release or Deploy in a Kubernetes cluster
* Upgrade Release or Deploy from a previous version
* Check if installation was successful and gather log files
* Clean the installation.

To investigate all possible options, please use the `--help` command. For example `xl kube --help`, or `xl kube install --help`. 

Also check [XL Kube Command Reference](https://docs.digital.ai/release/docs/xl-platform/operator/xl-kube) for more details.


## Installation

Installing Release or Deploy in Kubernetes is as easy as running this command:

```shell
xl kube install
```

This command will take care of the asking for the relevant configuration using an interative wizard; producing Kubernetes yaml files and applying them to perform the installation.

Before we kick it off, let's get our ducks in a row

- We are installing 24.3.2 version of Release. Later in the workshop we will upgrade it to 24.3.3
- The license files will be provided during the workshop and needs to be saved in the working directory.
- Both Kubernetes namespace and hostname need to be unique. For this workshop, we will refer to the namespace as `ns-yourname`. Every time you encounter `ns-yourname`, replace it with your own namespace, for example `ns-alice`. The namespace total length needs to be below 12 characters. The namespace will be created during the installation.  
- When installing on Azure, you will [create a DNS label](https://learn.microsoft.com/en-us/azure/aks/static-ip#apply-a-dns-label-to-the-service) for 
  - `release-ns-yourname.westus2.cloudapp.azure.com`.  
  ⚠️ The host name depends on the region the cluster is in. Refer to [Confluence](https://digitalai.atlassian.net/wiki/spaces/Labs/pages/76855148558/xl+kube+workshop+--+Passwords+Clusters) to find out which region you are in,
- When using minikube or Docker you can use any host name you want, for example
  - `release-ns-yourname.local`
  - Also make sure this host name has a entry in /etc/hosts file.
- On minikube we can use the standard storage class, also the default.  
- On Azure we use two custom storage classes. They already exist on the cluster:
  - `xl-kube-workshop-file-storage-class` based on [Azure Files Dynamic](https://docs.microsoft.com/en-us/azure/aks/azure-files-dynamic-pv)
  - `xl-kube-workshop-disk-storage-class` based on [Azure Disk Dynamic](https://docs.microsoft.com/en-us/azure/aks/azure-disks-dynamic-pv)
  - or other provided classes, it is for Release requirement to use RWX based storage class.

Now let's get started!

Kick off the `xl kube install` command and look closely at the answers below. Note that sometimes you can take the default, sometimes you need to give the value as prompted below and sometimes you need to give a custom value. Please take it slow -- this wizard is a very concentrated form of all needed parameters for a Kubernetes based installation and doesn't lend itself very well to rush through it with a next-next-next approach. Kubernetes itself can be bewildering if everything is not specified exactly as it should, so you can save some time from debugging by putting in the values with care. 

We've marked some of the questions where you need to pay extra attention with a warning sign.

In order not to overstretch the cluster during our workshop, please make sure to use a maximum of two Release replicas, and tweak the rest of the resources as indicated below. 

For more details on the questions and answers, check our documentation:  [Installation Wizard for Digital.ai Release](https://docs.digital.ai/release/docs/xl-platform/operator/xl-op-install-wizard-release)

The following example is for Minikube / Docker Desktop setup. For minikube / Docker Desktop choose 'PlainK8s' for K8sSetup and use default storage classes.
When using minikube or Docker you can use any host name you want, for example `release-ns-yourname.local`.

```text
$ xl kube install
? Following kubectl context will be used during execution: `minikube`? Yes
⚠️? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: PlainK8s [Plain multi-node K8s cluster]
⚠️? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
⚠️? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
? Do you want to create custom Kubernetes namespace ns-yourname, it does not exist: Yes
? Product server you want to perform install for: dai-release [Digital.ai Release with optional Remote Runner]
? Select type of image registry: default [Default (Uses various public image registries for the installation images)]
? Enter the repository name for the application and operator images (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): xebialabsunsupported
? Enter the Release image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): xl-release
⚠️? Enter the application image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 24.3.2
⚠️? Select source of the license: generate [Generate the license (accepting EULA, this is only for temporary license)]
⚠️? Enter the release server replica count: 2
⚠️? Enter PVC size for Release (Gi): 1
? Select between supported Access Modes for the Release pods (use ReadWriteMany if you plan to use multiple pods): ReadWriteMany [ReadWriteMany]
? Do you want to enable http2 for release: No
? Select between supported ingress types: nginx [NGINX]
? Do you want to enable an TLS/SSL configuration (if yes, requires existing TLS secret in the namespace): No
⚠️? Provide DNS name for accessing UI of the server: release-ns-yourname.local
? Provide administrator password: PQCWFYRK2I7HM3rI
? Type of the OIDC configuration: no-oidc [No OIDC Configuration]
? Enter the operator image to use (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): release-operator
⚠️? Enter the operator image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 24.3.2
? Select source of the repository keystore: generate [Generate the repository keystore during installation (you need to have keytool utility installed in your path)]
? Provide keystore passphrase: v1Btc6GSTd33Hb2Z
⚠️? Provide storage class for the server: standard
? Do you want to install a new PostgreSQL on the cluster: Yes
⚠️? Provide Storage Class to be defined for PostgreSQL: standard
⚠️? Provide PVC size for PostgreSQL (Gi): 1
? Do you want to install a new RabbitMQ on the cluster: Yes
? Replica count to be defined for RabbitMQ: 1
⚠️? Storage Class to be defined for RabbitMQ: standard
⚠️? Provide PVC size for RabbitMQ (Gi): 1
? Install Remote Runner for Digital.ai Release: No
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeRelease              | ReadWriteMany                                      |
	| AdminPassword                  | PQCWFYRK2I7HM3rI                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240105-094650                                    |
	| Http2EnabledRelease            | false                                              |
	| ImageNameRelease               | xl-release                                         |
	| ImageRegistryType              | default                                            |
	| ImageTag                       | 24.3.2                                             |
	| IngressHost                    | release-ns-yourname.local                          |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| KeystorePassphrase             | v1Btc6GSTd33Hb2Z                                   |
	| LicenseSource                  | generate                                           |
	| Namespace                      | ns-yourname                                        |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageRelease           | release-operator                                   |
	| OperatorImageTag               | 24.3.2                                             |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | standard                                           |
	| ProcessType                    | install                                            |
	| PvcSizeRelease                 | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | standard                                           |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | false                                              |
	| RemoteRunnerInstallConfirm     | false                                              |
	| RemoteRunnerReleaseName        | remote-runner                                      |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabsunsupported                               |
	| ServerType                     | dai-release                                        |
	| ShortServerName                | xlr                                                |
	| StorageClass                   | standard                                           |
	| UseCustomNamespace             | true                                               |
	| XlrReplicaCount                | 2                                                  |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values?
```

`xl kube install` gives a moment to breathe and shows the values we have entered. Before we say yes and start the installation, here's an overview of what will happen

1. Store answers in a file for reuse.
2. Generate Kubernetes yaml and other files needed.
3. Use `kubectl` to apply files in bulk and start the installation.

Note that subsequently, we can pick up the process at any time by using command line flags on `xl kube install`

* We can edit the answers file and use it as input for `xl kube install`, avoiding the interactive questions
* We can edit the Kubernetes Yaml file, and directly apply them using `kubectl`
* We can edit the Kubernetes Yaml files, and apply them in bulk using `xl kube install`

Now let's say **Yes** to the question and see what happens

```
For current process files will be generated in the: digitalai/dai-release/ns-yourname/20240105-125343/kubernetes
```

All files of this installation run will be stored in a directory that has the timestamp in it. We will use these files later so it is useful to note that we get a reference to this directory here.

```
Generated answers file successfully: digitalai/generated_answers_dai-release_ns-yourname_install-20240105-125343.yaml
```

The answers are stored in this file and can be reused later.

```
Starting install processing.
Created keystore digitalai/dai-release/ns-yourname/20240105-125343/kubernetes/repository-keystore.jceks
Skip creating namespace ns-yourname, already exists
Using custom resource name dai-xlr-ns-yourname
Generated files successfully for PlainK8s installation.
```

All needed Yaml files are being created, as well as a keystore that will be used by the Release server.
On Kubernetes, the namespace is configured automatically.

```
Applying resources to the cluster!
```

This indicates that `xl kube install` will now start calling `kubectl` with the generated yaml. 

The custom resource definition is shared by everybody in the cluster. During installation  you may get this question. In that case, answer "No"

```
Do you want to replace the resource customresourcedefinition/digitalaireleases.xlr.digital.ai with specification from file
digitalai/dai-release/ns-yourname/20240105-125343/kubernetes/template/custom-resource-definition.yaml: 
»⚠️ No
```

You will now see the list of all files being applied.

```
Applied resource customresourcedefinition/digitalaireleases.xlr.digital.ai from the file digitalai/dai-release/ns-yourname/20240105-125343/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xlr-operator-controller-manager from the file digitalai/dai-release/ns-yourname/20240105-125343/kubernetes/template/deployment.yaml
Applied resource clusterrole/ns-yourname-xlr-operator-manager from the file digitalai/dai-release/ns-yourname/20240105-125343/kubernetes/template/manager-clusterrole.yaml
Applied resource clusterrolebinding/ns-yourname-xlr-operator-manager from the file digitalai/dai-release/ns-yourname/20240105-125343/kubernetes/template/manager-clusterrolebinding.yaml
Applied resource role/xlr-operator-manager from the file digitalai/dai-release/ns-yourname/20240105-125343/kubernetes/template/manager-role.yaml
Applied resource rolebinding/xlr-operator-manager from the file digitalai/dai-release/ns-yourname/20240105-125343/kubernetes/template/manager-rolebinding.yaml
Applied resource role/xlr-operator-proxy from the file digitalai/dai-release/ns-yourname/20240105-125343/kubernetes/template/proxy-role.yaml
Applied resource rolebinding/xlr-operator-proxy from the file digitalai/dai-release/ns-yourname/20240105-125343/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalairelease/dai-xlr-ns-yourname from the file digitalai/dai-release/ns-yourname/20240105-125343/kubernetes/dai-release_cr.yaml
Install finished successfully!
```

And we are done!

Sort of...

This is Kubernetes. We have only told the cluster what the 'desired state' is and Kubernetes has acknowledged that it has gotten the configuration. It will now _start_ the work on making it so. In other words, we have only shipped over the necessary files to K8s and now need to trust that the system will do the work. And that no errors will occur in the process. By design, Kubernetes will _not_ tell you when it is done and will _not_ tell you if an error occured. You need to ask for it and know where to look.

That's why we added the `xl kube check` command. It knows what to check for and will give you timely status updates.

## Wait for resources with xl kube check

The `xl kube check` command will query Kubernetes if the installation is successful or not, and download all necessary configuration and log files for troubleshooting.

Use `xl kube check --help` to see all options and some example invocations.

To see how our installation is doing, invoke the command

```shell
xl kube check --wait-for-ready 5 --skip-collecting
```

After asking the questions of what you want to check, this will wait 5 minutes for successful installation, and will not download all configuration and logs.

⚠️ Note that for the last part of the check you need to have `helm` installed. 

```text
$ xl kube check --wait-for-ready 5 --skip-collecting
? Following kubectl context will be used during execution: `minikube`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: PlainK8s [Plain multi-node K8s cluster]
⚠️? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
⚠️? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
? Product server you want to perform check for: dai-release [Digital.ai Release with optional Remote Runner]
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240105-142459                                    |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| Namespace                      | ns-yourname                                        |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| ProcessType                    | check                                              |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | false                                              |
	| RemoteRunnerInstallConfirm     | false                                              |
	| RemoteRunnerReleaseName        | remote-runner                                      |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| ServerType                     | dai-release                                        |
	| ShortServerName                | xlr                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-release/ns-yourname/20240105-142459/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release_ns-yourname_check-20240105-142459.yaml 
Collecting the CRD and CR data
Collecting the CR data
Waiting for resources to be ready
Deployment deployment/xlr-operator-controller-manager is available in the namespace ns-yourname
Deployment deployment/dai-xlr-ns-yourname-nginx-ingress-controller is available in the namespace ns-yourname
Deployment deployment/dai-xlr-ns-yourname-nginx-ingress-controller-default-backend is available in the namespace ns-yourname
PVC pvc/data-dai-xlr-ns-yourname-rabbitmq-0 is bound in the namespace ns-yourname
Pod pod/dai-xlr-ns-yourname-rabbitmq-0 is available in the namespace ns-yourname
PVC pvc/data-dai-xlr-ns-yourname-postgresql-0 is bound in the namespace ns-yourname
Pod pod/dai-xlr-ns-yourname-postgresql-0 is available in the namespace ns-yourname
PVC pvc/dai-xlr-ns-yourname-digitalai-release is bound in the namespace ns-yourname
Pod pod/dai-xlr-ns-yourname-digitalai-release-0 is available in the namespace ns-yourname
Pod pod/dai-xlr-ns-yourname-digitalai-release-1 is available in the namespace ns-yourname
Skipping checking helm installation status, there is no helm in the path on the local system
Check finished successfully!
```

**Exercise**: find out what combination of command line flags you need to give in order to avoid any prompt.

<!-- xl kube check --answers digitalai/generated_answers_dai-release_kube-hes_install-20221101-132633.yaml --skip-collecting -w 5 -S -->

## Discover how to open the page and login

We have not configured the DNS, so we can't access the public URL yet: `http://release-ns-yourname.local/` (same is true for other setups).

However, we can connect directly to the Release via service port forwarding.  
```shell
$ kubectl port-forward --namespace ns-yourname svc/dai-xlr-ns-yourname-digitalai-release 18080:80
Forwarding from 127.0.0.1:18080 -> 5516
Forwarding from [::1]:18080 -> 5516
```

Now open [http://localhost:18080](http://localhost:18080) and log in as `admin`. 

If you forgot the password, you can get it with the command from the helm info (username is as always `admin`):

```shell
## To get the admin password for xl-release, run:
kubectl get secret --namespace ns-yourname dai-xlr-ns-yourname-digitalai-release -o jsonpath="{.data.releasePassword}" | base64 --decode; echo
```

Check the version in **(Gear icon) > About Digital.ai Release**. We should be running **Version 24.3.2**

## Set up DNS on Azure

To configure the DNS we will need to update the Custom Resource (CR) YAML and tell Kubernetes we changed it.


Open the file `dai-release_cr.yaml` that can be found in the directory `digitalai/dai-release/ns-yourname/20240105-142459/kubernetes/`.

Check the installation console output to find the correct timestamp. It's in the line `For current process files will be generated in the: digitalai/dai-release/ns-yourname/20240105-142459/kubernetes`.

Add your hostname in the yaml path of the CR file under spec > nginx-ingress-controller > service > annotations

```yaml
spec:
  …  
  nginx-ingress-controller:
    …
    service:
      …
      annotations:
        service.beta.kubernetes.io/azure-dns-label-name: release-ns-yourname        
```

Save the file and apply the changes to Kubernetes with the command:

```shell
kubectl apply -n ns-yourname -f digitalai/dai-release/ns-yourname/20240105-142459/kubernetes/dai-release_cr.yaml
```
The output should say 

```shell
digitalairelease.xlr.digital.ai/dai-xlr-ns-yourname configured
```

Now try to open [http://release-ns-yourname.westus2.cloudapp.azure.com/](http://release-ns-yourname.westus2.cloudapp.azure.com/)

Note: it may take a while for the DNS changes to come through and you may get a 'server not found' page for a while.
Note: The browser will warn that the site is not secure because of the certificate. This happens because we are using a self-signed certificate and not a proper certificate. Ignore the warning and proceed to the site. 


## Set up 'DNS' on localhost for Docker Desktop

When using a local kube cluster, we need to edit the local `hosts` file and add your host name here.

The procedure is slightly different for Unix and Windows. For more detailed instructions than the ones below, see [How to Edit Your Hosts File on Windows, Mac, or Linux](https://www.howtogeek.com/27350/beginner-geek-how-to-edit-your-hosts-file/)

For Docker Desktop, after adding the changes to the `hosts` file, go to [http://release-ns-yourname.local](http://release-ns-yourname.local) or for HTTPS [https://release-ns-yourname.local](https://release-ns-yourname.local)

Note: The browser will warn that the site is not secure because of the certificate. This happens because we are using a self-signed certificate and not a proper certificate. Ignore the warning and proceed to the site.

### Linux / Macos

```shell
sudo vi /etc/hosts
```

Add following line somewhere:

```text
127.0.0.1 release-ns-yourname.local
```

### Windows

The hosts file is located in `C:\Windows\System32\drivers\etc\hosts`. You need to edit it as an administrator and add the following line. 

```text
127.0.0.1 release-ns-yourname.local
```

## Set up 'DNS' on localhost for Minikube

There are multiple ways to access the application on minikube. Check following document for details: [Accessing apps](https://minikube.sigs.k8s.io/docs/handbook/accessing/)

When using a local kube cluster, we need to edit the local `hosts` file and add your host name here.

The procedure is slightly different for Unix and Windows. For more detailed instructions than the ones below, see [How to Edit Your Hosts File on Windows, Mac, or Linux](https://www.howtogeek.com/27350/beginner-geek-how-to-edit-your-hosts-file/)

Configure the `hosts` file.

### Linux / Macos

```shell
sudo vi /etc/hosts
```

Add following line somewhere:

```text
127.0.0.1 release-ns-yourname.local
```

### Windows

The hosts file is located in `C:\Windows\System32\drivers\etc\hosts`. You need to edit it as an administrator and add the following line.

```text
127.0.0.1 release-ns-yourname.local
```

### Exposing Minikube services

By default, Minikube will not assign external IP addresses to services so we will have to expose the services manually. We do this by executing:
```shell
minikube service dai-xlr-ns-yourname-nginx-ingress-controller -n ns-yourname
```
which outputs:
```text
|-------------|----------------------------------------------|-------------|---------------------------|
|  NAMESPACE  |                     NAME                     | TARGET PORT |            URL            |
|-------------|----------------------------------------------|-------------|---------------------------|
| ns-yourname | dai-xlr-ns-yourname-nginx-ingress-controller | http/80     | http://192.168.49.2:32534 |
|             |                                              | https/443   | http://192.168.49.2:32030 |
|-------------|----------------------------------------------|-------------|---------------------------|
🏃  Starting tunnel for service dai-xlr-ns-yourname-nginx-ingress-controller.
|-------------|----------------------------------------------|-------------|------------------------|
|  NAMESPACE  |                     NAME                     | TARGET PORT |          URL           |
|-------------|----------------------------------------------|-------------|------------------------|
| ns-yourname | dai-xlr-ns-yourname-nginx-ingress-controller |             | http://127.0.0.1:63965 |
|             |                                              |             | http://127.0.0.1:63966 |
|-------------|----------------------------------------------|-------------|------------------------|
```

Now you should be able to access http://release-ns-yourname.local:63965

### Using NodePort to connect to Release

We will use NodePort option in following example, and will need to update the Custom Resource (CR) YAML and tell Kubernetes we changed it.

Open the file `dai-release_cr.yaml` that can be found in the directory `digitalai/dai-release/ns-yourname/20240105-142459/kubernetes/`.

Check the installation console output to find the correct timestamp. It's in the line `For current process files will be generated in the: digitalai/dai-release/ns-yourname/20240105-142459/kubernetes`.

Change from `LoadBalancer` value in the yaml path of the CR file under spec > nginx-ingress-controller > service > type

```yaml
spec:
  …  
  nginx-ingress-controller:
    …
    service:
      …
      type: NodePort
```

Save the file and apply the changes to Kubernetes with the command:

```shell
kubectl apply -n ns-yourname -f digitalai/dai-release/ns-yourname/20221031-131244/kubernetes/dai-release_cr.yaml
```
The output should say

```shell
digitalairelease.xlr.digital.ai/dai-xlr-ns-yourname configured
```

Note: it may take a while for the service changes to come through.


Check now the changes with following command:

```shell
$ kubectl get service dai-xlr-ns-yourname-nginx-ingress-controller  -n ns-yourname
NAME                                            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
dai-xlr-ns-yourname-nginx-ingress-controller   NodePort   10.97.133.203   <none>        80:32039/TCP,443:31948/TCP   52m
```

Wait until _Type_ has value _"NodePort"_, like it is in above example.

Now try to open [http://release-ns-yourname.local:32039/](http://release-ns-yourname.local:32039/) or for HTTPS: [http://release-ns-yourname.local:31948/](http://release-ns-yourname.local:31948/)

Ports _32039_, _31948_ are from above example, replace the port value with the correct value that you have in the response.

Note: The browser will warn that the site is not secure because of the certificate. This happens because we are using a self-signed certificate and not a proper certificate. Ignore the warning and proceed to the site.


---

[Next](../part-1/lab-2-upgrade-release.md)
