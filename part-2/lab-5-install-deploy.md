
# Lab 5 - Install Deploy using a private image registry

In this lab we will do the installation of Digital.ai Deploy with a private image registry. Most large enterprises will not allow to download container images from just any public repositpory.

The installation will be done in three phases.
1. Generate all files
2. Modify generated files
3. Apply the files to the cluster

We will change the generated files to do the following:
- Use a private container registry at `azureakstestcluster.azurecr.io` for all images;
- Define Azure DNS settings (only in case you are working on Azure)

`xl kube` downloads blueprints from [https://dist.xebialabs.com/public/xl-op-blueprints/](https://dist.xebialabs.com/public/xl-op-blueprints/).
In case the working environment does not have access to Internet, we can download the blueprints to use them from local directory.
The zip versions of the needed blueprints are available on our [public Nexus repository](https://nexus.xebialabs.com/nexus/content/repositories/digitalai-public/ai/digital/xlclient/blueprints/xl-op-blueprints/).

## Installation

Download the blueprints for the current version:
[https://nexus.xebialabs.com/nexus/content/repositories/digitalai-public/ai/digital/xlclient/blueprints/xl-op-blueprints/24.1.0-1226.113/xl-op-blueprints-24.1.0-1226.113.zip](https://nexus.xebialabs.com/nexus/content/repositories/digitalai-public/ai/digital/xlclient/blueprints/xl-op-blueprints/24.1.0-1226.113/xl-op-blueprints-24.1.0-1226.113.zip).


Unzip the `xl-op-blueprints-24.10-1226.113.zip` to the `xl-op-blueprints` directory in your working directory. You can now use it with the `--local-repo` flag.

### Dry-run installation

Run the installation with `--dry-run`. With this flag, `xl` will just generate the files in the local directory and stop.


```shell
xl kube install --dry-run --local-repo ./xl-op-blueprints
```

⚠️ In order not to overstretch the cluster during our workshop, please make sure to use a maximum of two master and worker replicas, and tweak the rest of the resources also as indicated below.

For minikube / Docker Desktop choose `PlainK8s` for K8sSetup and use default storage classes.

When using minikube or Docker you can use any host name you want, for example `deploy-ns-yourname.local`. Otherwise, use the Azure host name you used in previous exercises but prefix it with `deploy`.

We are using here private registry so we need to add `azureakstestcluster.azurecr.io` to the repository name.

Questions for license, storage class and hostname will be similar as for Release in the previous exercises. Make sure you use the Deploy license!

For the other questions and answers details check [Installation Wizard for Digital.ai Deploy](https://docs.digital.ai/bundle/devops-deploy-version-v.24.1/page/deploy/operator/xl-op-install-wizard-deploy.html)

```text
? Following kubectl context will be used during execution: `minikube`? Yes
⚠️? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: PlainK8s [Plain multi-node K8s cluster]
⚠️? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
⚠️? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
⚠️? Product server you want to perform install for: dai-deploy [Digital.ai Deploy]
? Select type of image registry: default [Default (Uses various public image registries for the installation images)]
? Enter the repository name for the application and operator images (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): xebialabsunsupported
? Enter the Deploy server image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): xl-deploy
⚠️? Enter the application image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 24.1.0-1226.113
? Enter the deploy task engine image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): deploy-task-engine
? Enter the central configuration image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): central-configuration
? Select source of the license: generate [Generate the license (accepting EULA, this is only for temporary license)]
⚠️? Enter the deploy worker replica count: 1
? Enter PVC size for Deploy master (Gi): 1
? Select between supported Access Modes for the Deploy pods: ReadWriteOnce [ReadWriteOnce]
⚠️? Enter the deploy worker replica count: 1
? Enter PVC size for Deploy worker (Gi): 1
? Select between supported ingress types: nginx [NGINX]
? Do you want to enable an TLS/SSL configuration (if yes, requires existing TLS secret in the namespace): No
? Provide DNS name for accessing UI of the server: deploy-ns-yourname.local
? Provide administrator password: GOtujGAU2U6XBcR7
? Type of the OIDC configuration: no-oidc [No OIDC Configuration]
? Enter the operator image to use (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): deploy-operator
⚠️? Enter the operator image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 24.1.0-1226.113
? Select source of the repository keystore: generate [Generate the repository keystore during installation (you need to have keytool utility installed in your path)]
? Provide keystore passphrase: L5uEqIfNXag0gtL2
? Provide storage class for the server: standard
? Do you want to install a new PostgreSQL on the cluster: Yes
? Provide Storage Class to be defined for PostgreSQL: standard
⚠️? Provide PVC size for PostgreSQL (Gi): 1
? Do you want to install a new RabbitMQ on the cluster: Yes
? Replica count to be defined for RabbitMQ: 1
? Storage Class to be defined for RabbitMQ: standard
⚠️? Provide PVC size for RabbitMQ (Gi): 1
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | GOtujGAU2U6XBcR7                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240108-225255                                    |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageRegistryType              | default                                            |
	| ImageTag                       | 24.1.0-1226.113                                    |
	| IngressHost                    | deploy-ns-yourname.local                           |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| KeystorePassphrase             | L5uEqIfNXag0gtL2                                   |
	| LicenseSource                  | generate                                           |
	| Namespace                      | ns-yourname                                        |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeploy            | deploy-operator                                    |
	| OperatorImageTag               | 24.1.0-1226.113                                    |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | standard                                           |
	| ProcessType                    | install                                            |
	| PvcSizeDeploy                  | 1                                                 |
	| PvcSizeDeployTaskEngine        | 1                                                 |
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
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | standard                                           |
	| UseCustomNamespace             | true                                               |
	| XldMasterCount                 | 1                                                  |
	| XldWorkerCount                 | 1                                                  |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_install-20240108-225255.yaml 
Starting install processing.
Created keystore digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/repository-keystore.jceks
Skip creating namespace ns-yourname, already exists
Update CR with namespace... /Using custom resource name dai-xld-ns-yourname
Generated files successfully for PlainK8s installation.
Applying resources to the cluster!
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/controller-manager-metrics-service.yaml
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/custom-resource-definition.yaml
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/deployment.yaml
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/leader-election-role.yaml
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/leader-election-rolebinding.yaml
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/manager-clusterrole.yaml
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/manager-clusterrolebinding.yaml
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/manager-role.yaml
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/manager-rolebinding.yaml
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/proxy-role.yaml
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/proxy-rolebinding.yaml
Skipping apply the file /Users/admin/digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```

Dry run will generate the files in the working folder, somewhere like `digitalai/dai-deploy/digitalai/20221020-001911/kubernetes`. Check the command output for the exact location. 

## Updating the image repository

We will now configure access and update the generated files so when we start the installation, images will be downloaded from our custom repository,

On the private registry we have following images:

<img src="./images/images.png" width="300px" alt="Repositories"/>

### Configure credentials

First create a secret in your namespace with the registry and authentication data. This way we don't have to store passwords in the Yaml files.

```shell
kubectl create secret docker-registry regcred \
  --docker-server=azureakstestcluster.azurecr.io \
  --docker-username=test-pull \
  --docker-password=qgt4DFDV5ULAZ67UQBv9mtwz/TOhVbPu \
  -n ns-yourname
```

### Update the image repository in the generated files

We now need to adapt some files to point to our custom repository for all images.

* `kubernetes/dai-deploy_cr.yaml`
* `kubernetes/template/deployment.yaml`
<!--
* `kubernetes/template/postgresql-init-keycloak-db.yaml` - only in case of upgrade
-->

#### kubernetes/dai-deploy_cr.yaml

Update following in the file:

```yaml
spec:
  #add
  global:
    imagePullSecrets: ["regcred"]
    imageRegistry: "azureakstestcluster.azurecr.io/xebialabs/tiny-tools"

```

#### kubernetes/template/deployment.yaml

Update following in the file:

```yaml
spec:
  …
  template:
    …
    spec:
      imagePullSecrets:  # Add
        - name: regcred  # Add
      containers:
        - name: kube-rbac-proxy
          image: azureakstestcluster.azurecr.io/kubebuilder/kube-rbac-proxy:v0.8.0 # Change
          …
        - name: manager
          image: azureakstestcluster.azurecr.io/xebialabs/deploy-operator:22.3.1 # Change

```

The `spec.template.spec.containers[1].image` should have already correct value.

<!--
#### kubernetes/template/postgresql-init-keycloak-db.yaml - only in case of upgrade

Update following in the file:

- spec.template.spec.imagePullSecrets[0].name: regcred
- spec.template.spec.initContainers[0].image: azureakstestcluster.azurecr.io/xebialabs/tiny-tools:22.2.0
- spec.template.spec.containers[0].image: azureakstestcluster.azurecr.io/xebialabs/tiny-tools:22.2.0
-->

### Update the Azure DNS setup

Open `kubernetes/dai-deploy_cr.yaml`.

Update the yaml path of the CR file `spec.nginx-ingress-controller.service.annotations` with `deploy-ns-yourname`:

```yaml
spec:
  …
  nginx-ingress-controller:
    …
    service:
      …
      annotations:
        service.beta.kubernetes.io/azure-dns-label-name: deploy-ns-yourname
```

Save the changes in the file.


### Update Minikube Ingress setup to use NodePort

We will use NodePort option in following example, and will need to update the Custom Resource (CR) YAML and tell Kubernetes we changed it.
Edit CR, open the `digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/dai-deploy_cr.yaml`.

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

Save the changes in the file.


### Use `xl kube install` with changed files to apply everything to the cluster

Following command will apply the just changed files on the K8S cluster:

```shell
xl kube install --files 20221020-001911 --local-repo ./xl-op-blueprints
```

Under `--files` we are using the reference on the previous dry-run by using part of the unique name from the run.

```text
$ xl kube install --files 20240108-225255 --local-repo ./xl-op-blueprints
? Following kubectl context will be used during execution: `minikube`? Yes
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | GOtujGAU2U6XBcR7                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240108-230020                                    |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageRegistryType              | default                                            |
	| ImageTag                       | 24.1.0-1226.113                                    |
	| IngressHost                    | digitalai.deploy.local.mini                        |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| KeystorePassphrase             | L5uEqIfNXag0gtL2                                   |
	| LicenseSource                  | generate                                           |
	| Namespace                      | ns-yourname                                        |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeploy            | deploy-operator                                    |
	| OperatorImageTag               | 24.1.0-1226.113                                    |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 8                                                  |
	| PostgresqlStorageClass         | standard                                           |
	| ProcessType                    | install                                            |
	| PvcSizeDeploy                  | 10                                                 |
	| PvcSizeDeployTaskEngine        | 10                                                 |
	| RabbitmqPvcSize                | 8                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | standard                                           |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | false                                              |
	| RemoteRunnerInstallConfirm     | false                                              |
	| RemoteRunnerReleaseName        | remote-runner                                      |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabsunsupported                               |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | standard                                           |
	| UseCustomNamespace             | true                                               |
	| XldMasterCount                 | 1                                                  |
	| XldWorkerCount                 | 1                                                  |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20240108-230020/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_install-20240108-230020.yaml 
Starting 'install' processing and will use generated files from reference install 20240108-225255 with files from digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes.
? Do you want to apply resources from previous run '20240108-225255': Yes
Applying resources to the cluster!
Applied resource service/xld-operator-controller-manager-metrics-service from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/deployment.yaml
Applied resource role/xld-operator-leader-election from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xld-operator-leader-election from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrole/ns-yourname-xld-operator-manager from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/manager-clusterrole.yaml
Applied resource clusterrolebinding/ns-yourname-xld-operator-manager from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/manager-clusterrolebinding.yaml
Applied resource role/xld-operator-manager from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/manager-role.yaml
Applied resource rolebinding/xld-operator-manager from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/manager-rolebinding.yaml
Applied resource role/xld-operator-proxy from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/proxy-role.yaml
Applied resource rolebinding/xld-operator-proxy from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalaideploy/dai-xld-ns-yourname from the file digitalai/dai-deploy/ns-yourname/20240108-225255/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```

After everything is on the cluster, you will see operator other resources pods running on the cluster.


## Wait for resources with xl kube check

After xl-cli finishes all resources are not yet ready on the cluster, try to run following checks that are waiting for the resources to be fully running and ready on the cluster.

Check for details with `xl kube check -h` for details what each flag is here.

```shell
xl kube check --wait-for-ready 5
xl kube check --wait-for-ready 5 --skip-collecting
xl kube check --wait-for-ready 5 --zip-files
```

For example output for the second command (the helm info on the end will be displayed if you have helm in the path):
Example is on the Azure.


For example output for the second command (the helm info on the end will be displayed if you have helm in the path):
Example is on the Azure.

```text
$ xl kube check --wait-for-ready 5 --skip-collecting
? Following kubectl context will be used during execution: `xl-kube-workshop`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
? Product server you want to perform clean for: dai-deploy [Digital.ai Deploy]
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221101-001131                                    |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | ns-yourname                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| ProcessType                    | check                                              |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20221101-001131/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_check-20221101-001131.yaml
Collecting the CR data
Waiting for resources to be ready
Deployment deployment/xld-operator-controller-manager is available in the namespace ns-yourname
Deployment deployment/dai-xld-ns-yourname-nginx-ingress-controller is available in the namespace ns-yourname
Deployment deployment/dai-xld-ns-yourname-nginx-ingress-controller-default-backend is available in the namespace ns-yourname
PVC pvc/data-dai-xld-ns-yourname-rabbitmq-0 is bound in the namespace ns-yourname
Pod pod/dai-xld-ns-yourname-rabbitmq-0 is available in the namespace ns-yourname
PVC pvc/data-dai-xld-ns-yourname-postgresql-0 is bound in the namespace ns-yourname
Pod pod/dai-xld-ns-yourname-postgresql-0 is available in the namespace ns-yourname
PVC pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-cc-server-0 is bound in the namespace ns-yourname
Pod pod/dai-xld-ns-yourname-digitalai-deploy-cc-server-0 is available in the namespace ns-yourname
PVC pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-0 is bound in the namespace ns-yourname
Pod pod/dai-xld-ns-yourname-digitalai-deploy-master-0 is available in the namespace ns-yourname
PVC pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-1 is bound in the namespace ns-yourname
Pod pod/dai-xld-ns-yourname-digitalai-deploy-master-1 is available in the namespace ns-yourname
PVC pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-0 is bound in the namespace ns-yourname
Pod pod/dai-xld-ns-yourname-digitalai-deploy-worker-0 is available in the namespace ns-yourname
PVC pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-1 is bound in the namespace ns-yourname
Pod pod/dai-xld-ns-yourname-digitalai-deploy-worker-1 is available in the namespace ns-yourname
Check finished successfully!
```

## Discover how to open the page and login

We can access the public URL now on Azure: `http://deploy-ns-yourname.westus2.cloudapp.azure.com/`.

However, we can connect directly to the Deploy master via service port forwarding.
```shell
$ kubectl port-forward --namespace ns-yourname svc/dai-xld-ns-yourname-digitalai-deploy-lb 18080:4516
Forwarding from 127.0.0.1:18080 -> 4516
Forwarding from [::1]:18080 -> 4516
```

Now open [http://localhost:18080](http://localhost:18080) and log in as `admin`.

If you forgot the password, you can get it with the command from the helm info (username is as always `admin`):

```shell
## To get the admin password for xl-deploy, run:
kubectl get secret --namespace ns-yourname dai-xld-ns-yourname-digitalai-deploy -o jsonpath="{.data.deploy-password}" | base64 --decode; echo
```

## Set up 'DNS' on localhost for Docker Desktop

When using a local kube cluster, we need to edit the local `hosts` file and add your host name here.

The procedure is slightly different for Unix and Windows. For more detailed instructions than the ones below, see [How to Edit Your Hosts File on Windows, Mac, or Linux](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/)

For Docker Desktop, after adding the changes to the `hosts` file, go to [http://deploy-ns-yourname.local](http://deploy-ns-yourname.local) or for HTTPS [https://deploy-ns-yourname.local](https://deploy-ns-yourname.local)

Note: The browser will warn that the site is not secure because of the certificate. This happens because we are using a self-signed certificate and not a proper certificate. Ignore the warning and proceed to the site.

### Linux / Macos

```shell
sudo vi /etc/hosts
```

Add following line somewhere:

```text
127.0.0.1 deploy-ns-yourname.local
```

### Windows

The hosts file is located in `C:\Windows\System32\drivers\etc\hosts`. You need to edit it as an administrator and add the following line.

```text
127.0.0.1 deploy-ns-yourname.local
```

## Set up 'DNS' on localhost for Minikube

There are multiple ways to access the application on minikube. Check following document for details: [Accessing apps](https://minikube.sigs.k8s.io/docs/handbook/accessing/)

When using a local kube cluster, we need to edit the local `hosts` file and add your host name here.

The procedure is slightly different for Unix and Windows. For more detailed instructions than the ones below, see [How to Edit Your Hosts File on Windows, Mac, or Linux](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/)

Configure the `hosts` file.

### Linux / Macos

```shell
sudo vi /etc/hosts
```

Add following line somewhere:

```text
127.0.0.1 deploy-ns-yourname.local
```

### Windows

The hosts file is located in `C:\Windows\System32\drivers\etc\hosts`. You need to edit it as an administrator and add the following line.

```text
127.0.0.1 deploy-ns-yourname.local
```

### Exposing Minikube services

By default, Minikube will not assign external IP addresses to services so we will have to expose the services manually. We do this by executing:
```shell
minikube service dai-xld-ns-yourname-nginx-ingress-controller -n ns-yourname
```
which outputs:
```text
|-------------|----------------------------------------------|-------------|---------------------------|
|  NAMESPACE  |                     NAME                     | TARGET PORT |            URL            |
|-------------|----------------------------------------------|-------------|---------------------------|
| ns-yourname | dai-xld-ns-yourname-nginx-ingress-controller | http/80     | http://192.168.49.2:32534 |
|             |                                              | https/443   | http://192.168.49.2:32030 |
|-------------|----------------------------------------------|-------------|---------------------------|
🏃  Starting tunnel for service dai-xld-ns-yourname-nginx-ingress-controller.
|-------------|----------------------------------------------|-------------|------------------------|
|  NAMESPACE  |                     NAME                     | TARGET PORT |          URL           |
|-------------|----------------------------------------------|-------------|------------------------|
| ns-yourname | dai-xld-ns-yourname-nginx-ingress-controller |             | http://127.0.0.1:63965 |
|             |                                              |             | http://127.0.0.1:63966 |
|-------------|----------------------------------------------|-------------|------------------------|
```

Now you should be able to access http://deploy-ns-yourname.local:63965


### Using NodePort to connect to Deploy

We are using NodePort in the ingress service setup, so we need to get the node ports. 

Check the ports with following command:

```shell
$ kubectl get service dai-xld-ns-yourname-nginx-ingress-controller  -n ns-yourname
NAME                                            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
dai-xld-ns-yourname-nginx-ingress-controller   NodePort   10.97.133.203   <none>        80:32039/TCP,443:31948/TCP   52m
```

Now try to open [http://deploy-ns-yourname.local:32039/](http://deploy-ns-yourname.local:32039/) or for HTTPS: [http://deploy-ns-yourname.local:31948/](http://deploy-ns-yourname.local:31948/)

Ports _32039_, _31948_ are from above example, replace the port value with the correct value that you have in the response.

Note: The browser will warn that the site is not secure because of the certificate. This happens because we are using a self-signed certificate and not a proper certificate. Ignore the warning and proceed to the site.


---

[Next](../part-2/lab-6-clean-deploy.md)
