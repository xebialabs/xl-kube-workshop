
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
[https://nexus.xebialabs.com/nexus/content/repositories/digitalai-public/ai/digital/xlclient/blueprints/xl-op-blueprints/22.3.2/xl-op-blueprints-22.3.2.zip](https://nexus.xebialabs.com/nexus/content/repositories/digitalai-public/ai/digital/xlclient/blueprints/xl-op-blueprints/22.3.2/xl-op-blueprints-22.3.2.zip).


Unzip the `xl-op-blueprints-22.3.2.zip` to the `xl-op-blueprints` directory in your working directory. You can now use it with the `--local-repo` flag.

### Dry-run installation

Run the installation with `--dry-run`. With this flag, `xl` will just generate the files in the local directory and stop.


```shell
xl kube install --dry-run --local-repo ./xl-op-blueprints
```


Use the following answers (example on the Azure):

⚠️ In order not to overstretch the cluster during our workshop, please make sure to use a maximum of two master and worker replicas, and tweak the rest of the resources also as indicated below.

For minikube / Docker Desktop choose `PlainK8s` for K8sSetup and use default storage classes.

When using minikube or Docker you can use any host name you want, for example `deploy-ns-yourname.local`. Otherwise, use the Azure host name you used in previous exercises but prefix it with `deploy`.

We are using here private registry so we need to add `azureakstestcluster.azurecr.io` to the repository name.

Questions for license, storage class and hostname will be similar as for Release in the previous exercises. Make sure you use the Deploy license!

For the other questions and answers details check [Installation Wizard for Digital.ai Deploy](https://docs.digital.ai/bundle/devops-deploy-version-v.22.3/page/deploy/operator/xl-op-install-wizard-deploy.html)

```text
? Following kubectl context will be used during execution: `xl-kube-workshop-2`? 
» Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned:
» AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'):
»⚠️ Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: 
»⚠️ ns-yourname
? Product server you want to perform install for: 
»⚠️ dai-deploy [Digital.ai Deploy]
? Enter the repository name (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): 
»⚠️ azureakstestcluster.azurecr.io/xebialabs
? Enter the deploy server image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): 
» xl-deploy
? Enter the image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 
» 22.3.1
? Enter the deploy task engine image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): 
» deploy-task-engine
? Enter the central configuration image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): 
» central-configuration
? Enter the deploy master server replica count: 
»⚠️ 2
? Enter PVC size for Deploy master (Gi): 
»⚠️ 1
? Select between supported Access Modes: 
» ReadWriteOnce [ReadWriteOnce]
? Enter the deploy worker replica count: 
»⚠️ 2
? Enter PVC size for Deploy worker (Gi): 
»⚠️ 1
? Enter PVC size for Central Configuration (Gi): 
»⚠️ 0.500000
? Select between supported ingress types: 
» nginx [NGINX]
? Do you want to enable an TLS/SSL configuration (if yes, requires existing TLS secret in the namespace): 
» No
? Provide DNS name for accessing UI of the server: 
»⚠️ deploy-ns-yourname.westus2.cloudapp.azure.com
? Provide administrator password: 
» 30Q5utfMV6O9wnHF
? Type of the OIDC configuration: 
» no-oidc [No OIDC Configuration]
? Enter the operator image to use (eg: <repositoryName>/<imageName>:<tagName>): 
»⚠️ azureakstestcluster.azurecr.io/xebialabs/deploy-operator:22.3.1
? Select source of the license: 
» file [Path to the license file (the file can be in clean text or base64 encoded)]
? Provide license file for the server: 
» ./xld-license.lic
? Select source of the repository keystore: 
» generate [Generate the repository keystore during installation (you need to have keytool utility installed in your path)]
? Provide keystore passphrase: 
» 1uwAFCtUJEdwmaDi
? Provide storage class for the server: 
»⚠️ xl-kube-workshop-file-storage-class
? Do you want to install a new PostgreSQL on the cluster: 
» Yes
? Provide Storage Class to be defined for PostgreSQL: 
»⚠️ xl-kube-workshop-disk-storage-class
? Provide PVC size for PostgreSQL (Gi): 
»⚠️ 1
? Do you want to install a new RabbitMQ on the cluster: 
» Yes
? Replica count to be defined for RabbitMQ: 
»⚠️ 1
? Storage Class to be defined for RabbitMQ: 
»⚠️ xl-kube-workshop-file-storage-class
? Provide PVC size for RabbitMQ (Gi): 
»⚠️ 1

...

? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/digitalai/20221020-001911/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_digitalai_install-20221020-001911.yaml
Starting install processing.
...
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
  …
  ImagePullSecret: regcred
  …
  TinyToolsImageRepository: "azureakstestcluster.azurecr.io/xebialabs/tiny-tools"
```

The `spec.centralConfiguration.image.repository`, `spec.ServerImageRepository` and `spec.WorkerImageRepository` should have already correct value.

##### Ngninx

```yaml
spec:
  …
  nginx-ingress-controller:
    …
    defaultBackend:
      …
      image:
        …
        registry: azureakstestcluster.azurecr.io
    …
    global:
      …
      imagePullSecrets: [ "regcred" ]
    …
    image:
      …
      registry: azureakstestcluster.azurecr.io
```

<!--
##### If you are using haproxy

- spec.haproxy-ingress.controller.imagePullSecrets: [ "regcred" ]
- spec.haproxy-ingress.controller.image.repository: azureakstestcluster.azurecr.io/jcmoraisjr/haproxy-ingress

##### If you are using embedded keycloak

- spec.keycloak.imagePullSecrets: [ "regcred" ]
- spec.keycloak.image.repository: azureakstestcluster.azurecr.io/jboss/keycloak
- spec.keycloak.postgresql.image.registry: azureakstestcluster.azurecr.io
-->

##### PostgreSQL

```yaml
spec:
  …
  postgresql:
    …
    global:
      …
      imagePullSecrets: [ "regcred" ]
    …
    image:
      …
      registry: azureakstestcluster.azurecr.io
```

##### RabbitMQ

```yaml
spec:
  …
  rabbitmq:
    …
    global:
      …
      imagePullSecrets: [ "regcred" ]
    …
    image:
      …
      registry: azureakstestcluster.azurecr.io
    …
    volumePermissions:
      …
      image:
        …
        registry: azureakstestcluster.azurecr.io
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
$ xl kube install --files 20221020-001911 --local-repo ./xl-op-blueprints
? Following kubectl context will be used during execution: `xl-kube-workshop`? Yes
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | nwOgq4l9J5PxQtLy                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221020-001911                                    |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageTag                       | 22.3.1                                             |
	| IngressHost                    | ns-vedran-xld.westus2.cloudapp.azure.com    |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| KeystorePassphrase             | LZtrjXiW39rlDY37                                   |
	| License                        | LS0tIExpY2Vuc2UgLS0tCkxpY2Vuc2UgdmVyc2lvbjogMwpQ.. |
	| LicenseFile                    | ./xld-license.lic                                  |
	| LicenseSource                  | file                                               |
	| Namespace                      | ns-yourname                                       |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeployGeneric     | azureakstestcluster.azurecr.io/xebialabs/deploy-.. |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | xl-kube-workshop-disk-storage-class                |
	| ProcessType                    | install                                            |
	| PvcSizeCc                      | 0.500000                                           |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | xl-kube-workshop-file-storage-class                |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | azureakstestcluster.azurecr.io/xebialabs           |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | xl-kube-workshop-file-storage-class                |
	| UseCustomNamespace             | true                                               |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_install-20221020-001911.yaml
Starting 'install' processing and will use generated files from reference install 20221020-001911 with files from digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes.
? Do you want to apply resources from previos run '20221020-001911': Yes
Applying resources to the cluster!
Applied resource clusterrole/deploy-ns-yourname-operator-proxy-role from the file digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/template/cluster-role-digital-proxy-role.yaml
Applied resource clusterrole/deploy-ns-yourname-operator-manager-role from the file digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/template/cluster-role-manager-role.yaml
Applied resource clusterrole/deploy-ns-yourname-operator-metrics-reader from the file digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/template/cluster-role-metrics-reader.yaml
Applied resource service/xld-operator-controller-manager-metrics-service from the file digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/template/deployment.yaml
Applied resource role/xld-operator-leader-election-role from the file digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xld-operator-leader-election-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrolebinding/deploy-ns-yourname-operator-manager-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/template/manager-rolebinding.yaml
Applied resource clusterrolebinding/deploy-ns-yourname-operator-proxy-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalaideploy/dai-xld-ns-yourname from the file digitalai/dai-deploy/ns-yourname/20221020-001911/kubernetes/dai-deploy_cr.yaml
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
Checking helm installation status
Operator's dai-xld-ns-yourname helm status in the namespace ns-yourname for the installation:
NAME: dai-xld-ns-yourname
LAST DEPLOYED: Mon Oct 31 23:06:42 2022
NAMESPACE: ns-yourname
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
## To get the application URL, run:
http://deploy-ns-yourname.westus2.cloudapp.azure.com/

## To get the admin password for xl-deploy, run:
kubectl get secret --namespace ns-yourname dai-xld-ns-yourname-digitalai-deploy -o jsonpath="{.data.deploy-password}" | base64 --decode; echo
## To get the password for postgresql, run:
kubectl get secret --namespace  ns-yourname dai-xld-ns-yourname-postgresql -o jsonpath="{.data.postgresql-password}" | base64 --decode; echo

## To get the password for rabbitMQ, run:
kubectl get secret --namespace  ns-yourname dai-xld-ns-yourname-rabbitmq   -o jsonpath="{.data.rabbitmq-password}" | base64 --decode; echo

## To edit custom resource dai-xld-ns-yourname
kubectl edit digitalaideploys.xld.digital.ai dai-xld-ns-yourname -n ns-yourname

## To restart deploy central configuration pods use restart of the statefulset
kubectl rollout restart sts dai-xld-ns-yourname-digitalai-deploy-cc-server -n ns-yourname

## To restart deploy master pods use restart of the statefulset
kubectl rollout restart sts dai-xld-ns-yourname-digitalai-deploy-master -n ns-yourname

## To restart deploy worker pods use restart of the statefulset
kubectl rollout restart sts dai-xld-ns-yourname-digitalai-deploy-worker -n ns-yourname

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

First discover the IP of your minikube node with:
```shell
$ minikube ip
192.168.59.103
```

Use that IP from your response in setting the `hosts` file.

### Linux / Macos

```shell
sudo vi /etc/hosts
```

Add following line somewhere:

```text
192.168.59.103 deploy-ns-yourname.local
```

### Windows

The hosts file is located in `C:\Windows\System32\drivers\etc\hosts`. You need to edit it as an administrator and add the following line.

```text
192.168.59.103 deploy-ns-yourname.local
```

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
