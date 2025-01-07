
# Lab 7 - Fix errors - Troubleshoot

We will now try to detect and correct problems that can happen during installation.

## Installation with few errors

Do installation with following errors:
- not existing operator image: xebialabs/deploy-operator:24.3.2
- wrong storage class on the postgresql PVC, we will use xl-kube-workshop-file-storage-class StorageClass on the Azure, that StorageClass is not providing required block storage 
  - on Minikube and Docker Desktop we don't have this option so we will use broken StorageClass called `broken-storage`
- wrong storage class on the RabbitMQ, we will use broken StorageClass called `broken-storage`


### Prepare broken StorageClass

Download file [broken-storage-class.yaml](./files/broken-storage-class.yaml) and apply it to your cluster:

```shell
? kubectl apply -f ./broken-storage-class.yaml
storageclass.storage.k8s.io/broken-storage created
```

### Do installation

```shell
xl kube install
```

Following are answers for the Azure. 

For minikube / Docker Desktop choose 'PlainK8s' for K8sSetup and use default storage classes.
In case of question `? Provide Storage Class to be defined for PostgreSQL: xl-kube-workshop-file-storage-class` for minikube / Docker Desktop choose _broken-storage_.

```text
$ xl kube install
? Following kubectl context will be used during execution: `minikube`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: PlainK8s [Plain multi-node K8s cluster]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): No
? Product server you want to perform install for: dai-deploy [Digital.ai Deploy]
? Select type of image registry: default [Default (Uses various public image registries for the installation images)]
? Enter the repository name for the application and operator images (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): xebialabsunsupported
? Enter the Deploy server image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): xl-deploy
? Enter the application image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 24.3.2
? Enter the deploy task engine image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): deploy-task-engine
? Enter the central configuration image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): central-configuration
? Select source of the license: generate [Generate the license (accepting EULA, this is only for temporary license)]
? Enter the deploy master replica count: 2
? Enter PVC size for Deploy master (Gi): 1
? Select between supported Access Modes for the Deploy pods: ReadWriteOnce [ReadWriteOnce]
? Enter the deploy worker replica count: 2
? Enter PVC size for Deploy worker (Gi): 1
? Select Deploy server protocol: http [HTTP - not encrypted]
? Select between supported ingress types: nginx [NGINX]
? Select ingress protocol: http [HTTP - not encrypted]
? Provide DNS name for accessing UI of the server: deploy-ns-yourname.local
? Provide administrator password: C9KOewb2Yr4Ujy7c
? Type of the OIDC configuration: no-oidc [No OIDC Configuration]
? Enter the operator image to use (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): deploy-operator
? Enter the operator image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 24.3.2
? Select source of the repository keystore: generate [Generate the repository keystore during installation (you need to have keytool utility installed in your path)]
? Provide repository keystore passphrase: IwZZ3X9DtMfmwxdF
? Provide storage class for the server: standard
? Do you want to install a new PostgreSQL on the cluster: Yes
? Provide Storage Class to be defined for PostgreSQL: broken-storage
? Provide PVC size for PostgreSQL (Gi): 1
? Do you want to install a new RabbitMQ on the cluster: Yes
? Replica count to be defined for RabbitMQ: 1
? Storage Class to be defined for RabbitMQ: broken-storage
? Provide PVC size for RabbitMQ (Gi): 1
     -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | C9KOewb2Yr4Ujy7c                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240429-173358                                    |
	| HttpProtocolDeploy             | http                                               |
	| HttpProtocolIngress            | http                                               |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageRegistryType              | default                                            |
	| ImageTag                       | 24.3.2                                             |
	| IngressHost                    | deploy-ns-yourname.local                           |
	| IngressKeystoreSource          | generate                                           |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| KeystorePassphrase             | IwZZ3X9DtMfmwxdF                                   |
	| LicenseSource                  | generate                                           |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeploy            | deploy-operator                                    |
	| OperatorImageTag               | 24.3.2                                             |
	| OsType                         | linux                                              |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | broken-storage                                     |
	| ProcessType                    | install                                            |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | broken-storage                                     |
	| RemoteRunnerClean              | false                                              |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | false                                              |
	| RemoteRunnerInstallType        |                                                    |
	| RemoteRunnerReleaseName        | release-runner                                     |
	| RemoteRunnerReleaseUrl         | http://dai-xlr-digitalai-release.digitalai         |
	| RemoteRunnerToken              |                                                    |
	| RemoteRunnerTokenExpiration    | 0                                                  |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| RemoteRunnerUserEmail          | release-runner@no.reply                            |
	| RemoteRunnerUserPassword       | eHpoqUaGB2b2PikB                                   |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabsunsupported                               |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | standard                                           |
	| UseCustomNamespace             | false                                              |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/digitalai/20240429-173358/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_digitalai_install-20240429-173358.yaml 
Starting install processing.
Created keystore digitalai/dai-deploy/digitalai/20240429-173358/kubernetes/repository-keystore.jceks
Skip creating namespace digitalai, already exists
Generated files successfully for PlainK8s installation.
Applying resources to the cluster!
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/digitalai/20240429-173358/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/digitalai/20240429-173358/kubernetes/template/deployment.yaml
Applied resource clusterrole/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-173358/kubernetes/template/manager-clusterrole.yaml
Applied resource clusterrolebinding/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-173358/kubernetes/template/manager-clusterrolebinding.yaml
Applied resource role/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-173358/kubernetes/template/manager-role.yaml
Applied resource rolebinding/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-173358/kubernetes/template/manager-rolebinding.yaml
Applied resource role/xld-operator-proxy from the file digitalai/dai-deploy/digitalai/20240429-173358/kubernetes/template/proxy-role.yaml
Applied resource rolebinding/xld-operator-proxy from the file digitalai/dai-deploy/digitalai/20240429-173358/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalaideploy/dai-xld from the file digitalai/dai-deploy/digitalai/20240429-173358/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```

Installation process successfully finished!

## Using wrong tag in the operator

We can use `xl kube check` to additionally check is everything is running correctly and detect what is happening with installation.
Detect the problem with:

```shell
xl kube check --wait-for-ready 1 --skip-collecting
```

Here is example of the running that line:

```text
? xl kube check --wait-for-ready 1 --skip-collecting
? Following kubectl context will be used during execution: `minikube`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: PlainK8s [Plain multi-node K8s cluster]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): No
? Product server you want to perform check for: dai-deploy [Digital.ai Deploy]
	-------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	-------------------------------- ----------------------------------------------------
	| AdminPassword                  | aHVOYtJiXwxOCV79                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240429-174623                                    |
	| IngressKeystoreSource          | generate                                           |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| OidcConfigType                 | existing                                           |
	| OsType                         | linux                                              |
	| ProcessType                    | check                                              |
	| RemoteRunnerClean              | false                                              |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | false                                              |
	| RemoteRunnerInstallType        |                                                    |
	| RemoteRunnerReleaseName        | release-runner                                     |
	| RemoteRunnerReleaseUrl         | http://dai-xlr-digitalai-release.digitalai         |
	| RemoteRunnerToken              |                                                    |
	| RemoteRunnerTokenExpiration    | 0                                                  |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| RemoteRunnerUserEmail          | release-runner@no.reply                            |
	| RemoteRunnerUserPassword       | HgHgyGsrpQCvjzxr                                   |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | false                                              |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/digitalai/20240429-174623/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_digitalai_check-20240429-174623.yaml 
Collecting the CRD and CR data
Collecting the CR data
Waiting for resources to be ready
Failed with waiting for the deployment xld-operator-controller-manager in the namespace digitalai with error: timeout while waiting for deployment/xld-operator-controller-manager to be Available
 For more details check:
 - Check the deployment describe events with the describe command: kubectl describe deployment xld-operator-controller-manager -n digitalai
 - Check the deployment logs for the more details with commands: kubectl logs deployment/xld-operator-controller-manager -n digitalai -f --all-containers=true
Saved resource data for deployment/xld-operator-controller-manager
 - Check the operator pod events with the describe command: kubectl describe pod xld-operator-controller-manager-58f659f-n8zxb -n digitalai
Skiped saving resource data for pod/xld-operator-controller-manager-58f659f-n8zxb
Saved resource data for events/xld-operator-controller-manager-58f659f-n8zxb.17cabf2727968e3b
Saved resource data for events/xld-operator-controller-manager-58f659f-n8zxb.17cabf2743936c21
Saved resource data for events/xld-operator-controller-manager-58f659f-n8zxb.17cabf291e6d64b2
Saved resource data for events/xld-operator-controller-manager-58f659f-n8zxb.17cabf291ff00c74
Saved resource data for events/xld-operator-controller-manager-58f659f-n8zxb.17cabf2923ba0668
Saved resource data for events/xld-operator-controller-manager-58f659f-n8zxb.17cabf2923c699ac
Saved resource data for events/xld-operator-controller-manager-58f659f-n8zxb.17cabf298a85dd81
Saved resource data for events/xld-operator-controller-manager-58f659f-n8zxb.17cabf298a86b657
Saved resource data for events/xld-operator-controller-manager-58f659f-n8zxb.17cabf29c3290c47
Saved resource data for events/xld-operator-controller-manager-58f659f-n8zxb.17cabf29c328d148
Stopping because error during wait for the deployment xld-operator-controller-manager

```

As you can see the check failed with final message `Stopping because error during wait for the deployment xld-operator-controller-manager`.
So we need to check more previous log is there something additionally helpful.
One of the notes is

    Check the operator pod events with the describe command: kubectl describe pod xld-operator-controller-manager-58f659f-n8zxb -n <ns>

So we run that command, and on the end we can see events:
```text
$ kubectl describe pod xld-operator-controller-manager-58f659f-n8zxb -n digitalai
...
Events:
  Type     Reason     Age                  From               Message
  ----     ------     ----                 ----               -------
  Normal   Scheduled  11m                  default-scheduler  Successfully assigned digitalai/xld-operator-controller-manager-58f659f-n8zxb to k3d-mycluster-server-0
  Normal   Pulling    11m                  kubelet            Pulling image "gcr.io/kubebuilder/kube-rbac-proxy:v0.13.1"
  Normal   Pulled     11m                  kubelet            Successfully pulled image "gcr.io/kubebuilder/kube-rbac-proxy:v0.13.1" in 7.966s (7.966s including waiting)
  Normal   Created    11m                  kubelet            Created container kube-rbac-proxy
  Normal   Started    11m                  kubelet            Started container kube-rbac-proxy
  Warning  Failed     10m (x4 over 11m)    kubelet            Error: ImagePullBackOff
  Normal   Pulling    9m51s (x4 over 11m)  kubelet            Pulling image "docker.io/xebialabsunsupported/deploy-operator:24.3.2"
  Warning  Failed     9m49s (x4 over 11m)  kubelet            Failed to pull image "docker.io/xebialabsunsupported/deploy-operator:24.3.2": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/xebialabsunsupported/deploy-operator:24.3.2": failed to resolve reference "docker.io/xebialabsunsupported/deploy-operator:24.3.2": docker.io/xebialabsunsupported/deploy-operator:24.3.2: not found
  Warning  Failed     9m49s (x4 over 11m)  kubelet            Error: ErrImagePull
  Normal   BackOff    80s (x41 over 11m)   kubelet            Back-off pulling image "docker.io/xebialabsunsupported/deploy-operator:24.3.2"
```

The events have reason of the failure: `Failed to pull image "docker.io/xebialabsunsupported/deploy-operator:24.3.2": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/xebialabsunsupported/deploy-operator:24.3.2": failed to resolve reference "docker.io/xebialabsunsupported/deploy-operator:24.3.2": docker.io/xebialabsunsupported/deploy-operator:24.3.2: not found`.

Ok, we can fix that by editing answers file from previous installation and run same installation again.
We edit the digitalai/generated_answers_dai-deploy_digitalai_install-20240429-173358.yaml and change the line from:

```
OperatorImageTag: 24.3.2
```

to

```
OperatorImageTag: 24.3.2
```

Repeat installation with answers file

```shell
xl kube install --answers digitalai/generated_answers_dai-deploy_digitalai_install-20240429-173358.yaml
```

```text
$ xl kube install --answers digitalai/generated_answers_dai-deploy_digitalai_install-20240429-173358.yaml
? Following kubectl context will be used during execution: `minikube`? Yes
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | C9KOewb2Yr4Ujy7c                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240429-180918                                    |
	| HttpProtocolDeploy             | http                                               |
	| HttpProtocolIngress            | http                                               |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageRegistryType              | default                                            |
	| ImageTag                       | 24.3.2                                             |
	| IngressHost                    | deploy-ns-yourname.local                           |
	| IngressKeystoreSource          | generate                                           |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| KeystorePassphrase             | IwZZ3X9DtMfmwxdF                                   |
	| LicenseSource                  | generate                                           |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeploy            | deploy-operator                                    |
	| OperatorImageTag               | 24.3.2                                             |
	| OsType                         | linux                                              |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | broken-storage                                     |
	| ProcessType                    | install                                            |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | broken-storage                                     |
	| RemoteRunnerClean              | false                                              |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | false                                              |
	| RemoteRunnerInstallType        |                                                    |
	| RemoteRunnerReleaseName        | release-runner                                     |
	| RemoteRunnerReleaseUrl         | http://dai-xlr-digitalai-release.digitalai         |
	| RemoteRunnerToken              |                                                    |
	| RemoteRunnerTokenExpiration    | 0                                                  |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| RemoteRunnerUserEmail          | release-runner@no.reply                            |
	| RemoteRunnerUserPassword       | 17qRaqbr0jCldZ8g                                   |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabsunsupported                               |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | standard                                           |
	| UseCustomNamespace             | false                                              |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/digitalai/20240429-180918/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_digitalai_install-20240429-180918.yaml 
Starting install processing.
Created keystore digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/repository-keystore.jceks
Skip creating namespace digitalai, already exists
Generated files successfully for PlainK8s installation.
Applying resources to the cluster!
? Do you want to replace the resource customresourcedefinition/digitalaideploys.xld.digital.ai with specification from file
digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/custom-resource-definition.yaml: Yes
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/custom-resource-definition.yaml
? Do you want to replace the resource deployment/xld-operator-controller-manager with specification from file
digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/deployment.yaml: Yes
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/deployment.yaml
? Do you want to replace the resource clusterrole/xld-operator-manager with specification from file
digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/manager-clusterrole.yaml: Yes
Applied resource clusterrole/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/manager-clusterrole.yaml
? Do you want to replace the resource clusterrolebinding/xld-operator-manager with specification from file
digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/manager-clusterrolebinding.yaml: Yes
Applied resource clusterrolebinding/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/manager-clusterrolebinding.yaml
? Do you want to replace the resource role/xld-operator-manager with specification from file
digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/manager-role.yaml: Yes
Applied resource role/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/manager-role.yaml
? Do you want to replace the resource rolebinding/xld-operator-manager with specification from file
digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/manager-rolebinding.yaml: Yes
Applied resource rolebinding/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/manager-rolebinding.yaml
? Do you want to replace the resource role/xld-operator-proxy with specification from file
digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/proxy-role.yaml: Yes
Applied resource role/xld-operator-proxy from the file digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/proxy-role.yaml
? Do you want to replace the resource rolebinding/xld-operator-proxy with specification from file
digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/proxy-rolebinding.yaml: Yes
Applied resource rolebinding/xld-operator-proxy from the file digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/template/proxy-rolebinding.yaml
? Do you want to replace the resource digitalaideploy/dai-xld with specification from file
digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/dai-deploy_cr.yaml: Yes
Applied resource digitalaideploy/dai-xld from the file digitalai/dai-deploy/digitalai/20240429-180918/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```

So now we have progress, the operator pod will be ready and running. We can go on the next problem.

## Using wrong storage class on rabbitmq

To detect next problem run again, reuse the answers file from previous check run:

```shell
xl kube check --wait-for-ready 1 --skip-collecting --answers digitalai/generated_answers_dai-deploy_digitalai_check-20240429-174623.yaml
```

Here is example of running that command:
```text
$ xl kube check --wait-for-ready 1 --skip-collecting --answers digitalai/generated_answers_dai-deploy_digitalai_check-20240429-174623.yaml
? Following kubectl context will be used during execution: `minikube`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: PlainK8s [Plain multi-node K8s cluster]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): No
? Product server you want to perform check for: dai-deploy [Digital.ai Deploy]
	-------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	-------------------------------- ----------------------------------------------------
	| AdminPassword                  | zxwaJ3XsxYOyqLnQ                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240429-183827                                    |
	| IngressKeystoreSource          | generate                                           |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| OidcConfigType                 | existing                                           |
	| OsType                         | linux                                              |
	| ProcessType                    | check                                              |
	| RemoteRunnerClean              | false                                              |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | false                                              |
	| RemoteRunnerInstallType        |                                                    |
	| RemoteRunnerReleaseName        | release-runner                                     |
	| RemoteRunnerReleaseUrl         | http://dai-xlr-digitalai-release.digitalai         |
	| RemoteRunnerToken              |                                                    |
	| RemoteRunnerTokenExpiration    | 0                                                  |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| RemoteRunnerUserEmail          | release-runner@no.reply                            |
	| RemoteRunnerUserPassword       | qoG4pcKyVI8jKuqH                                   |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | false                                              |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/digitalai/20240429-183827/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_digitalai_check-20240429-183827.yaml 
Collecting the CRD and CR data
Collecting the CR data
Waiting for resources to be ready
Deployment deployment/xld-operator-controller-manager is available in the namespace digitalai
Deployment deployment/dai-xld-nginx-ingress-controller is available in the namespace digitalai
Deployment deployment/dai-xld-nginx-ingress-controller-default-backend is available in the namespace digitalai
Failed with waiting for the persitent volume claim data-dai-xld-rabbitmq-0 in the namespace digitalai with error: timeout while waiting for pvc/data-dai-xld-rabbitmq-0 to be jsonpath=.status.phase=Bound
 For more details check:
 - Check the PVC describe events with the describe command: kubectl describe pvc data-dai-xld-rabbitmq-0 -n digitalai
 - Check the PVC StorageClass if it is compatible with the defined persitence access mode
 - Check the cluster resources for the StorageClass can provide the defined storage size
Saved resource data for pvc/data-dai-xld-rabbitmq-0
Saved resource data for events/dai-xld-digitalai-deploy.17cac0dfa8744b8b
Saved resource data for events/dai-xld-digitalai-deploy-cc-server-0.17cac0e22bb50d23
Saved resource data for events/dai-xld-digitalai-deploy-cc-server-0.17cac0e226577ef0
Saved resource data for events/dai-xld-digitalai-deploy-cc-server-0.17cac0e223ae5926
Saved resource data for events/dai-xld-digitalai-deploy-master-0.17cac0e8847e0622
Saved resource data for events/dai-xld-digitalai-deploy-master-0.17cac0e888d5b0a7
Saved resource data for events/dai-xld-digitalai-deploy-worker-0.17cac0e8896d1402
Saved resource data for events/dai-xld-digitalai-deploy-master-0.17cac0e88f8d99d4
Saved resource data for events/dai-xld-digitalai-deploy-worker-0.17cac0e88372c0d1
Saved resource data for events/dai-xld-digitalai-deploy-worker-0.17cac0e890032cd0
Stopping because error during wait for the persitent volume claim data-dai-xld-rabbitmq-0
```

In the last line we can see that rabbitmq PVC has a problems `Stopping because error during wait for the persitent volume claim data-dai-rabbitmq-0`.
From log we have suggestion to check describe of the PVC:

    - Check the PVC describe events with the describe command: kubectl describe pvc data-dai-xld-digitalai-rabbitmq-0 -n digitalai

That command is displaying following in the events section:
```text
Name:          data-dai-xld-rabbitmq-0
Namespace:     digitalai
StorageClass:  broken-storage
Status:        Pending
Volume:        
Labels:        app.kubernetes.io/instance=dai-xld
               app.kubernetes.io/name=rabbitmq
Annotations:   volume.beta.kubernetes.io/storage-provisioner: k8s.io/no-provisioner
               volume.kubernetes.io/storage-provisioner: k8s.io/no-provisioner
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      
Access Modes:  
VolumeMode:    Filesystem
Used By:       dai-xld-rabbitmq-0
Events:
  Type    Reason                Age                    From                         Message
  ----    ------                ----                   ----                         -------
  Normal  ExternalProvisioning  3m54s (x123 over 33m)  persistentvolume-controller  Waiting for a volume to be created either by the external provisioner 'k8s.io/no-provisioner' or manually by the system administrator. If volume creation is delayed, please verify that the provisioner is running and correctly registered.
```

The message in the events shows that StorageClass provisioner is not creating the volume: `waiting for a volume to be created, either by external provisioner "k8s.io/no-provisioner" or manually created by system administrator`.
So we can fix the provisioner or change StorageClass, we will change StorageClass.

We can again edit the last installed answers file and change it with correct disk storage class:
From:

```
RabbitmqStorageClass: 'broken-storage'
```

To

```
RabbitmqStorageClass: 'standard'
```

On minikube and Docker Desktop use default storage class (`standard`)!

On Azure use `xl-kube-workshop-file-storage-class`

Repeat installation with answers file, but clean before, because storage class cannot be updated on the PVC.

⚠️ During running clean before command, do not delete CRD because there are other installation that are reusing same CRD!

Cleaning takes some time because we need to wait the pods to timeout initialization.

Run following:

```shell
xl kube clean
xl kube install --answers digitalai/generated_answers_dai-deploy_digitalai_install-20240429-180918.yaml
```
or simply
```shell
xl kube install --clean-before --answers digitalai/generated_answers_dai-deploy_digitalai_install-20240429-180918.yaml
```

Here is example of the installation output:

```text
xl kube install --clean-before --skip-prompts --answers digitalai/generated_answers_dai-deploy_digitalai_install-20240429-180918.yaml
? Following kubectl context will be used during execution: `minikube`? Yes
? Enter the name of custom resource definition you want to reuse or replace: digitalaideploys.xld.digital.ai
? Enter the name of custom resource: dai-xld
? Should we preserve persisted volume claims? If not all volume data will be lost: No
 	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | C9KOewb2Yr4Ujy7c                                   |
	| CleanBefore                    | true                                               |
	| CrName                         | dai-xld                                            |
	| CrdName                        | digitalaideploys.xld.digital.ai                    |
	| CreateNamespace                | true                                               |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240429-190735                                    |
	| HttpProtocolDeploy             | http                                               |
	| HttpProtocolIngress            | http                                               |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageRegistryType              | default                                            |
	| ImageTag                       | 24.3.2                                             |
	| IngressHost                    | deploy-ns-yourname.local                           |
	| IngressKeystoreSource          | generate                                           |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCrdReused                    | false                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| KeystorePassphrase             | IwZZ3X9DtMfmwxdF                                   |
	| LicenseSource                  | generate                                           |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeploy            | deploy-operator                                    |
	| OperatorImageTag               | 24.3.2                                             |
	| OsType                         | linux                                              |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | broken-storage                                     |
	| PreservePvc                    | false                                              |
	| ProcessType                    | install                                            |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | standard                                           |
	| RemoteRunnerClean              | false                                              |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | false                                              |
	| RemoteRunnerInstallType        |                                                    |
	| RemoteRunnerReleaseName        | release-runner                                     |
	| RemoteRunnerReleaseUrl         | http://dai-xlr-digitalai-release.digitalai         |
	| RemoteRunnerToken              |                                                    |
	| RemoteRunnerTokenExpiration    | 0                                                  |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| RemoteRunnerUserEmail          | release-runner@no.reply                            |
	| RemoteRunnerUserPassword       | s02qv5ewRlkRgZl5                                   |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabsunsupported                               |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | standard                                           |
	| UseCustomNamespace             | false                                              |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/digitalai/20240429-190735/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_digitalai_install-20240429-190735.yaml 
Starting install processing.
Created keystore digitalai/dai-deploy/digitalai/20240429-190735/kubernetes/repository-keystore.jceks
Skip creating namespace digitalai, already exists
Generated files successfully for PlainK8s installation.
? We are now starting to clean the cluster from the resources with the old version of the product.
For each resource you will be asked to confirm to delete the resource.
If you skip deleting of some resources the upgrade process will try to apply the resource (if the resource has some non-updatable parts the update of the resource will fail).
Continue?
 Yes
Cleaning the resources on the cluster!
CR dai-xld is available, deleting
? Do you want to delete the resource digitalaideploys.xld.digital.ai/dai-xld: Yes
Deleted digitalaideploys.xld.digital.ai/dai-xld from namespace digitalai
Deleting statefulsets
? Do you want to delete the resource sts/dai-xld-digitalai-deploy-cc-server: Yes
Deleted sts/dai-xld-digitalai-deploy-cc-server from namespace digitalai (already deleted)
? Do you want to delete the resource sts/dai-xld-digitalai-deploy-master: Yes
Deleted sts/dai-xld-digitalai-deploy-master from namespace digitalai (already deleted)
? Do you want to delete the resource sts/dai-xld-digitalai-deploy-worker: Yes
Deleted sts/dai-xld-digitalai-deploy-worker from namespace digitalai (already deleted)
Deleting deployments
? Do you want to delete the resource deployment/xld-operator-controller-manager: Yes
Deleted deployment/xld-operator-controller-manager from namespace digitalai
Deleting jobs
Deleting services
Deleting secrets
? Do you want to delete the resource secret/dai-xld-digitalai-deploy-license: Yes
Deleted secret/dai-xld-digitalai-deploy-license from namespace digitalai
Deleting configmaps
? Do you want to delete the resource configmap/xld-operator-controller-manager: Yes
Deleted configmap/xld-operator-controller-manager from namespace digitalai
? Do you want to delete the resource ingressclass/nginx-dai-xld: Yes
Deleted ingressclass/nginx-dai-xld from namespace digitalai
Deleting roles
? Do you want to delete the resource role/xld-operator-manager: Yes
Deleted role/xld-operator-manager from namespace digitalai
? Do you want to delete the resource role/xld-operator-proxy: Yes
Deleted role/xld-operator-proxy from namespace digitalai
? Do you want to delete the resource clusterrole/xld-operator-manager: Yes
Deleted clusterrole/xld-operator-manager from namespace digitalai
? Do you want to delete the resource clusterrole/dai-xld-nginx-ingress-controller: Yes
Deleted clusterrole/dai-xld-nginx-ingress-controller from namespace digitalai
? Do you want to delete the resource rolebinding/xld-operator-manager: Yes
Deleted rolebinding/xld-operator-manager from namespace digitalai
? Do you want to delete the resource rolebinding/xld-operator-proxy: Yes
Deleted rolebinding/xld-operator-proxy from namespace digitalai
? Do you want to delete the resource clusterrolebinding/xld-operator-manager: Yes
Deleted clusterrolebinding/xld-operator-manager from namespace digitalai
? Do you want to delete the resource clusterrolebinding/dai-xld-nginx-ingress-controller: Yes
Deleted clusterrolebinding/dai-xld-nginx-ingress-controller from namespace digitalai
Deleting PVCs
? Do you want to delete the resource pvc/data-dai-xld-postgresql-0: Yes
Deleted pvc/data-dai-xld-postgresql-0 from namespace digitalai
? Do you want to delete the resource pvc/data-dai-xld-rabbitmq-0: Yes
Deleted pvc/data-dai-xld-rabbitmq-0 from namespace digitalai
? Do you want to delete the resource pvc/data-dir-dai-xld-digitalai-deploy-master-0: Yes
Deleted pvc/data-dir-dai-xld-digitalai-deploy-master-0 from namespace digitalai
? Do you want to delete the resource pvc/data-dir-dai-xld-digitalai-deploy-worker-0: Yes
Deleted pvc/data-dir-dai-xld-digitalai-deploy-worker-0 from namespace digitalai
CRD digitalaideploys.xld.digital.ai is available, deleting
? Do you want to delete the resource crd/digitalaideploys.xld.digital.ai: No
Skipping delete of the resource crd/digitalaideploys.xld.digital.ai
? Do you want to delete the resource crd/digitalaideploys.xld.digital.ai: No
Skipping delete of the resource crd/digitalaideploys.xld.digital.ai
Applying resources to the cluster!
? Do you want to replace the resource customresourcedefinition/digitalaideploys.xld.digital.ai with specification from file
digitalai/dai-deploy/digitalai/20240429-190735/kubernetes/template/custom-resource-definition.yaml: Yes
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/digitalai/20240429-190735/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/digitalai/20240429-190735/kubernetes/template/deployment.yaml
Applied resource clusterrole/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-190735/kubernetes/template/manager-clusterrole.yaml
Applied resource clusterrolebinding/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-190735/kubernetes/template/manager-clusterrolebinding.yaml
Applied resource role/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-190735/kubernetes/template/manager-role.yaml
Applied resource rolebinding/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-190735/kubernetes/template/manager-rolebinding.yaml
Applied resource role/xld-operator-proxy from the file digitalai/dai-deploy/digitalai/20240429-190735/kubernetes/template/proxy-role.yaml
Applied resource rolebinding/xld-operator-proxy from the file digitalai/dai-deploy/digitalai/20240429-190735/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalaideploy/dai-xld from the file digitalai/dai-deploy/digitalai/20240429-190735/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```


## Using wrong storage class on postgresql 

To detect next problem run again, reuse the answers file from previous check run:

```shell
xl kube check --wait-for-ready 1 --skip-collecting --answers digitalai/generated_answers_dai-deploy_digitalai_check-20240429-174623.yaml
```

The `xl kube check` output will look like this:
```
Failed with waiting for the persitent volume claim data-dai-xld-postgresql-0 in the namespace digitalai with error: timeout while waiting for pvc/data-dai-xld-postgresql-0 to be jsonpath=.status.phase=Bound
 For more details check:
 - Check the PVC describe events with the describe command: kubectl describe pvc data-dai-xld-postgresql-0 -n digitalai
 - Check the PVC StorageClass if it is compatible with the defined persitence access mode
 - Check the cluster resources for the StorageClass can provide the defined storage size
Saved resource data for pvc/data-dai-xld-postgresql-0
Saved resource data for events/dai-xld-rabbitmq-0.17cac3f409c44872
Saved resource data for events/dai-xld-rabbitmq-0.1714398075957
Saved resource data for events/dai-xld-digitalai-deploy-worker-0.17cac3f78717f5ae
Saved resource data for events/dai-xld-digitalai-deploy-worker-0.17cac3f79180a761
Saved resource data for events/dai-xld-digitalai-deploy-master-0.17cac3f792d3047f
Saved resource data for events/dai-xld-digitalai-deploy-master-0.17cac3f7879025d5
Saved resource data for events/dai-xld-digitalai-deploy-master-0.17cac3f7848c54e4
Saved resource data for events/dai-xld-digitalai-deploy-worker-0.17cac3f784581679
Saved resource data for events/dai-xld-rabbitmq-0.17cac3fb353a8e71
Saved resource data for events/dai-xld-rabbitmq-0.1714398095517
Stopping because error during wait for the persitent volume claim data-dai-xld-postgresql-0
```

Note: for the Minikube and Docker Desktop users this problem will have same way of detection and same fix as for `RabbitmqStorageClass` answer!
So you can try to resolve it on your own, the below step is describing the problem on the Azure. 


Here is example of running the `xl kube check` command on Azure:

```text
$ xl kube check --wait-for-ready 1 --skip-collecting --answers digitalai/generated_answers_dai-deploy_ns-yourname_check-20221031-231543.yaml
? Following kubectl context will be used during execution: `xl-kube-workshop`? Yes
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-233539                                    |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | ns-yourname                                        |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| ProcessType                    | check                                              |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20221031-233539/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_check-20221031-233539.yaml
Collecting the CR data
Waiting for resources to be ready
Deployment deployment/xld-operator-controller-manager is available in the namespace ns-yourname
Deployment deployment/dai-xld-ns-yourname-nginx-ingress-controller is available in the namespace ns-yourname
Deployment deployment/dai-xld-ns-yourname-nginx-ingress-controller-default-backend is available in the namespace ns-yourname
PVC pvc/data-dai-xld-ns-yourname-rabbitmq-0 is bound in the namespace ns-yourname
Pod pod/dai-xld-ns-yourname-rabbitmq-0 is available in the namespace ns-yourname
PVC pvc/data-dai-xld-ns-yourname-postgresql-0 is bound in the namespace ns-yourname
Failed with waiting for the pod dai-xld-ns-yourname-postgresql-0 in the namespace ns-yourname with error: timeout while waiting for pod/dai-xld-ns-yourname-postgresql-0 to be Ready
 For more details check:
 - Check the pod describe events with the describe command: kubectl describe pod dai-xld-ns-yourname-postgresql-0 -n ns-yourname
 - Check the pod logs for the more details with commands: kubectl logs pod/dai-xld-ns-yourname-postgresql-0 -n ns-yourname -f --all-containers=true
 - Check if cluster nodes can provide the pod requests for memory and CPU
Saved resource data for pod/dai-xld-ns-yourname-postgresql-0
Saved resource data for events/dai-xld-ns-yourname-digitalai-deploy-worker-0.172347e59800aac0
Saved resource data for events/dai-xld-ns-yourname-rabbitmq-0.172347e5b390aae5
Saved resource data for events/dai-xld-ns-yourname-rabbitmq-0.172347e5a766ac56
Saved resource data for events/dai-xld-ns-yourname-rabbitmq-0.172347e5883deb98
Saved resource data for events/dai-xld-ns-yourname-rabbitmq-0.172347e58249ce8a
Saved resource data for events/dai-xld-ns-yourname-digitalai-deploy-cc-server-0.172347e57e1c73e8
Saved resource data for events/dai-xld-ns-yourname-postgresql-0.172347e59b633e10
Saved resource data for events/dai-xld-ns-yourname-nginx-ingress-controller.172347e6de5e2ae4
Saved resource data for events/dai-xld-ns-yourname-rabbitmq-0.1667255352292
Saved resource data for events/dai-xld-ns-yourname-rabbitmq-0.1667255365377
Stopping because error during wait for the pod dai-xld-ns-yourname-postgresql-0
```

In the last line we can see that postgresql pod has problems `Stopping because error during wait for the pod dai-xld-ns-yourname-postgresql-0`.
From log we have suggestion to again check describe of the pod:

    - Check the pod logs for the more details with commands: kubectl logs pod/dai-xld-ns-yourname-postgresql-0 -n ns-yourname -f --all-containers=true

That command is displaying following in the events section:

```text
? kubectl logs pod/dai-xld-ns-yourname-postgresql-0 -n ns-yourname -f --all-containers=true
postgresql 22:50:02.92
postgresql 22:50:02.93 Welcome to the Bitnami postgresql container
postgresql 22:50:02.93 Subscribe to project updates by watching https://github.com/bitnami/bitnami-docker-postgresql
postgresql 22:50:02.93 Submit issues and feature requests at https://github.com/bitnami/bitnami-docker-postgresql/issues
postgresql 22:50:02.94
postgresql 22:50:02.96 INFO  ==> ** Starting PostgreSQL setup **
postgresql 22:50:02.99 INFO  ==> Validating settings in POSTGRESQL_* env vars..
postgresql 22:50:03.00 INFO  ==> Loading custom pre-init scripts...
postgresql 22:50:03.00 INFO  ==> Initializing PostgreSQL database...
chmod: changing permissions of '/bitnami/postgresql/data': Operation not permitted
postgresql 22:50:03.05 WARN  ==> Lack of permissions on data directory!
chmod: changing permissions of '/bitnami/postgresql/data': Operation not permitted
postgresql 22:50:03.06 WARN  ==> Lack of permissions on data directory!
postgresql 22:50:03.06 INFO  ==> pg_hba.conf file not detected. Generating it...
postgresql 22:50:03.06 INFO  ==> Generating local authentication configuration
```

After googling the error we can find that postgresql is not working on the current StorageClass:
[https://github.com/bitnami/bitnami-docker-postgresql/issues/285#issuecomment-814925197](https://github.com/bitnami/bitnami-docker-postgresql/issues/285#issuecomment-814925197)

So PVC's storage class is not correct. We can again edit same answers file and change it with correct disk storage class:
From:

```
PostgresqlStorageClass: 'xl-kube-workshop-file-storage-class'
```

To

```
PostgresqlStorageClass: 'xl-kube-workshop-disk-storage-class'
```

On minikube and Docker Desktop modify to use the default storage class (`standard`)!

Repeat installation with answers file, but clean before, because storage class cannot be updated on the PVC.

⚠️ During running next clean command, do not delete CRD because there are other installation that are reusing same CRD!

Cleaning takes some time because we need to wait the pods to timeout initialization.

Run following:

```shell
xl kube clean
xl kube install --skip-prompts --answers digitalai/generated_answers_dai-deploy_ns-yourname_install-20240429-190735.yaml
```
or simply
```shell
kube install --clean-before --answers digitalai/generated_answers_dai-deploy_ns-yourname_install-20240429-190735.yaml
```

Here is example of the installation output:

```text
xl kube install --skip-prompts --answers digitalai/generated_answers_dai-deploy_ns-yourname_install-20240429-190735.yaml
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | C9KOewb2Yr4Ujy7c                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240429-194818                                    |
	| HttpProtocolDeploy             | http                                               |
	| HttpProtocolIngress            | http                                               |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageRegistryType              | default                                            |
	| ImageTag                       | 24.3.2                                             |
	| IngressHost                    | deploy-ns-yourname.local                           |
	| IngressKeystoreSource          | generate                                           |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| KeystorePassphrase             | IwZZ3X9DtMfmwxdF                                   |
	| LicenseSource                  | generate                                           |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeploy            | deploy-operator                                    |
	| OperatorImageTag               | 24.3.2                                             |
	| OsType                         | linux                                              |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | standard                                           |
	| ProcessType                    | install                                            |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | standard                                           |
	| RemoteRunnerClean              | false                                              |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | false                                              |
	| RemoteRunnerInstallType        |                                                    |
	| RemoteRunnerReleaseName        | release-runner                                     |
	| RemoteRunnerReleaseUrl         | http://dai-xlr-digitalai-release.digitalai         |
	| RemoteRunnerToken              |                                                    |
	| RemoteRunnerTokenExpiration    | 0                                                  |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| RemoteRunnerUserEmail          | release-runner@no.reply                            |
	| RemoteRunnerUserPassword       | XCdxl98PPDdPAiux                                   |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabsunsupported                               |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | standard                                           |
	| UseCustomNamespace             | false                                              |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/digitalai/20240429-194818/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_digitalai_install-20240429-194818.yaml 
Starting install processing.
Created keystore digitalai/dai-deploy/digitalai/20240429-194818/kubernetes/repository-keystore.jceks
Skip creating namespace digitalai, already exists
Generated files successfully for PlainK8s installation.
Applying resources to the cluster!
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/digitalai/20240429-194818/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/digitalai/20240429-194818/kubernetes/template/deployment.yaml
Applied resource clusterrole/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-194818/kubernetes/template/manager-clusterrole.yaml
Applied resource clusterrolebinding/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-194818/kubernetes/template/manager-clusterrolebinding.yaml
Applied resource role/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-194818/kubernetes/template/manager-role.yaml
Applied resource rolebinding/xld-operator-manager from the file digitalai/dai-deploy/digitalai/20240429-194818/kubernetes/template/manager-rolebinding.yaml
Applied resource role/xld-operator-proxy from the file digitalai/dai-deploy/digitalai/20240429-194818/kubernetes/template/proxy-role.yaml
Applied resource rolebinding/xld-operator-proxy from the file digitalai/dai-deploy/digitalai/20240429-194818/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalaideploy/dai-xld from the file digitalai/dai-deploy/digitalai/20240429-194818/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```

We can now run final check to see if everything is running OK:

```text
xl kube check --skip-prompts --wait-for-ready 5 --skip-collecting --answers digitalai/generated_answers_dai-deploy_digitalai_check-20240429-174623.yaml
```

Output is now OK:

```text
$ xl kube check --skip-prompts --wait-for-ready 5 --skip-collecting --answers digitalai/generated_answers_dai-deploy_digitalai_check-20240429-174623.yaml
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AdminPassword                  | z6U6q6OoZ58pI653                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240429-201321                                    |
	| IngressKeystoreSource          | generate                                           |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| OidcConfigType                 | existing                                           |
	| OsType                         | linux                                              |
	| ProcessType                    | check                                              |
	| RemoteRunnerClean              | false                                              |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | false                                              |
	| RemoteRunnerInstallType        |                                                    |
	| RemoteRunnerReleaseName        | release-runner                                     |
	| RemoteRunnerReleaseUrl         | http://dai-xlr-digitalai-release.digitalai         |
	| RemoteRunnerToken              |                                                    |
	| RemoteRunnerTokenExpiration    | 0                                                  |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| RemoteRunnerUserEmail          | release-runner@no.reply                            |
	| RemoteRunnerUserPassword       | 6QKTm7tjQ0zPLspS                                   |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | false                                              |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/digitalai/20240429-201321/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_digitalai_check-20240429-201321.yaml 
Collecting the CRD and CR data
Collecting the CR data
Waiting for resources to be ready
Deployment deployment/xld-operator-controller-manager is available in the namespace digitalai
Deployment deployment/dai-xld-nginx-ingress-controller is available in the namespace digitalai
Deployment deployment/dai-xld-nginx-ingress-controller-default-backend is available in the namespace digitalai
PVC pvc/data-dai-xld-rabbitmq-0 is bound in the namespace digitalai
Pod pod/dai-xld-rabbitmq-0 is available in the namespace digitalai
PVC pvc/data-dai-xld-postgresql-0 is bound in the namespace digitalai
Pod pod/dai-xld-postgresql-0 is available in the namespace digitalai
Pod pod/dai-xld-digitalai-deploy-cc-server-0 is available in the namespace digitalai
PVC pvc/data-dir-dai-xld-digitalai-deploy-master-0 is bound in the namespace digitalai
Pod pod/dai-xld-digitalai-deploy-master-0 is available in the namespace digitalai
PVC pvc/data-dir-dai-xld-digitalai-digitalai-deploy-master-1 is bound in the namespace digitalai
Pod pod/dai-xld-digitalai-deploy-master-1 is available in the namespace digitalai
PVC pvc/data-dir-dai-xld-digitalai-deploy-worker-0 is bound in the namespace digitalai
Pod pod/dai-xld-digitalai-deploy-worker-0 is available in the namespace digitalai
PVC pvc/data-dir-dai-xld-digitalai-deploy-worker-1 is bound in the namespace digitalai
Pod pod/dai-xld-digitalai-deploy-worker-1 is available in the namespace digitalai
Checking helm installation status
Operator's dai-xld helm status in the namespace digitalai for the installation:
NAME: dai-xld
LAST DEPLOYED: Mon Apr 29 14:30:08 2024
NAMESPACE: digitalai
STATUS: deployed
REVISION: 3
TEST SUITE: None
NOTES:
## To get the application URL, run:
http://deploy-ns-yourname.local
## To connect over HTTP port forward on port 4516 use:
kubectl port-forward service/dai-xld-digitalai-deploy 4516:deploy-http
## To get the admin password for Digitalai Deploy, run:
kubectl get secret --namespace digitalai dai-xld-digitalai-deploy -o jsonpath="{.data.deployPassword}" | base64 --decode; echo
## Access to the master's services is avalable via headless service: "dai-xld-digitalai-deploy-master"
## To get the password for main database, run:
kubectl get secret --namespace digitalai dai-xld-digitalai-deploy -o jsonpath="{.data.mainDatabasePassword}" | base64 --decode; echo
## To get the password for report database, run:
kubectl get secret --namespace digitalaidai-xld-digitalai-deploy -o jsonpath="{.data.reportDatabasePassword}" | base64 --decode; echo
## To get the password for rabbitMQ, run:
kubectl get secret --namespace digitalai dai-xld-digitalai-deploy -o jsonpath="{.data.rabbitmqPassword}" | base64 --decode; echo

## To edit custom resource dai-xld
kubectl edit digitalaideploys.xld.digital.ai dai-xld -n digitalai

## To restart deploy central configuration pods use restart of the statefulset
kubectl rollout restart sts dai-xld-digitalai-deploy-cc-server -n digitalai

## To restart deploy master pods use restart of the statefulset
kubectl rollout restart sts dai-xld-digitalai-deploy-master -n digitalai

## To restart deploy worker pods use restart of the statefulset
kubectl rollout restart sts dai-xld-digitalai-deploy-worker -n digitalai

Check finished successfully!
```

---

[Next](../part-3/lab-8-change-configuration-file.md)

