
# Lab 7 - Fix errors - Troubleshoot

We will now try to detect and correct problems that can happen during installation.

## Installation with few errors

Do installation with following errors:
- not existing operator image: xebialabs/deploy-operator:22.3.X
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
? Following kubectl context will be used during execution: `xl-kube-workshop`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
? Product server you want to perform install for: dai-deploy [Digital.ai Deploy]
? Enter the repository name (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): xebialabs
? Enter the deploy server image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): xl-deploy
? Enter the image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 22.3.1
? Enter the deploy task engine image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): deploy-task-engine
? Enter the central configuration image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): central-configuration
? Enter the deploy master server replica count: 2
? Enter PVC size for Deploy master (Gi): 1
? Select between supported Access Modes: ReadWriteOnce [ReadWriteOnce]
? Enter the deploy worker replica count: 2
? Enter PVC size for Deploy worker (Gi): 1
? Enter PVC size for Central Configuration (Gi): 0.500000
? Select between supported ingress types: nginx [NGINX]
? Do you want to enable an TLS/SSL configuration (if yes, requires existing TLS secret in the namespace): No
? Provide DNS name for accessing UI of the server: deploy-ns-yourname.westus2.cloudapp.azure.com
? Provide administrator password: ARTLY8Qgl6FPLXN7
? Type of the OIDC configuration: no-oidc [No OIDC Configuration]
? Enter the operator image to use (eg: <repositoryName>/<imageName>:<tagName>): xebialabs/deploy-operator:22.3.X
? Select source of the license: file [Path to the license file (the file can be in clean text or base64 encoded)]
? Provide license file for the server: ./xld-license.lic
? Select source of the repository keystore: generate [Generate the repository keystore during installation (you need to have keytool utility installed in your path)]
? Provide keystore passphrase: HEXRzENbwPvn85YB
? Provide storage class for the server: xl-kube-workshop-file-storage-class
? Do you want to install a new PostgreSQL on the cluster: Yes
? Provide Storage Class to be defined for PostgreSQL: xl-kube-workshop-file-storage-class
? Provide PVC size for PostgreSQL (Gi): 1
? Do you want to install a new RabbitMQ on the cluster: Yes
? Replica count to be defined for RabbitMQ: 1
? Storage Class to be defined for RabbitMQ: broken-storage
? Provide PVC size for RabbitMQ (Gi): 1
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | ARTLY8Qgl6FPLXN7                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-230614                                    |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageTag                       | 22.3.1                                             |
	| IngressHost                    | deploy-ns-yourname.westus2.cloudapp.azure.com      |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| KeystorePassphrase             | HEXRzENbwPvn85YB                                   |
	| License                        | LS0tIExpY2Vuc2UgLS0tCkxpY2Vuc2UgdmVyc2lvbjogMwpQ.. |
	| LicenseFile                    | ./xld-license.lic                                  |
	| LicenseSource                  | file                                               |
	| Namespace                      | ns-yourname                                        |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeployGeneric     | xebialabs/deploy-operator:22.3.X                   |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | xl-kube-workshop-file-storage-class                |
	| ProcessType                    | install                                            |
	| PvcSizeCc                      | 0.500000                                           |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | broken-storage                                     |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabs                                          |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | xl-kube-workshop-file-storage-class                |
	| UseCustomNamespace             | true                                               |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_install-20221031-230614.yaml
Starting install processing.
Created keystore digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/repository-keystore.jceks
Skip creating namespace ns-yourname, already exists
Creating namespace ns-yourname... - Using custom resource name dai-xld-ns-yourname
Generated files successfully for AzureAKS installation.
Applying resources to the cluster!
Applied resource clusterrole/deploy-ns-yourname-operator-proxy-role from the file digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/template/cluster-role-digital-proxy-role.yaml
Applied resource clusterrole/deploy-ns-yourname-operator-manager-role from the file digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/template/cluster-role-manager-role.yaml
Applied resource clusterrole/deploy-ns-yourname-operator-metrics-reader from the file digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/template/cluster-role-metrics-reader.yaml
Applied resource service/xld-operator-controller-manager-metrics-service from the file digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/template/custom-resource-definition.yaml
? Do you want to replace the resource customresourcedefinition/digitalaideploys.xld.digital.ai with specification from file
digitalai/dai-deploy/ns-vedran333/20221102-202805/kubernetes/template/custom-resource-definition.yaml: No
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/template/deployment.yaml
Applied resource role/xld-operator-leader-election-role from the file digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xld-operator-leader-election-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrolebinding/deploy-ns-yourname-operator-manager-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/template/manager-rolebinding.yaml
Applied resource clusterrolebinding/deploy-ns-yourname-operator-proxy-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalaideploy/dai-xld-ns-yourname from the file digitalai/dai-deploy/ns-yourname/20221031-230614/kubernetes/dai-deploy_cr.yaml
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
? Following kubectl context will be used during execution: `xl-kube-workshop`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
? Product server you want to perform check for: dai-deploy [Digital.ai Deploy]
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-231543                                    |
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
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20221031-231543/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_check-20221031-231543.yaml
Collecting the CR data
Waiting for resources to be ready
Failed with waiting for the deployment xld-operator-controller-manager in the namespace ns-yourname with error: timeout while waiting for deployment/xld-operator-controller-manager to be Available
 For more details check:
 - Check the deployment describe events with the describe command: kubectl describe deployment xld-operator-controller-manager -n ns-yourname
 - Check the deployment logs for the more details with commands: kubectl logs deployment/xld-operator-controller-manager -n ns-yourname -f --all-containers=true
Saved resource data for deployment/xld-operator-controller-manager
 - Check the operator pod events with the describe command: kubectl describe pod xld-operator-controller-manager-5757db45fb-4lv8q -n ns-yourname
Skiped saving resource data for pod/xld-operator-controller-manager-5757db45fb-4lv8q
Saved resource data for events/xld-operator-controller-manager-5757db45fb.172346ce0176d8af
Saved resource data for events/xld-operator-controller-manager.172346cdfefae2ca
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce2257fbf5
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce29898bcf
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce2ef95966
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce2f0893dc
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce4ee0c4ff
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce4ee11aef
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce5ccb113f
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce5ccb3b0b
Stopping because error during wait for the deployment xld-operator-controller-manager
```

As you can see the check failed with final message `Stopping because error during wait for the deployment xld-operator-controller-manager`.
So we need to check more previous log is there something additionally helpful.
One of the notes is

    Check the operator pod events with the describe command: kubectl describe pod xld-operator-controller-manager-5757db45fb-4lv8q -n ns-yourname

So we run that command, and on the end we can see events:
```text
$ kubectl describe pod xld-operator-controller-manager-5757db45fb-4lv8q -n ns-yourname
...
Events:
  Type     Reason     Age                  From               Message
  ----     ------     ----                 ----               -------
  Normal   Scheduled  11m                  default-scheduler  Successfully assigned ns-yourname/xld-operator-controller-manager-5757db45fb-4lv8q to aks-nodepool1-22570493-vmss000001
  Normal   Pulled     11m                  kubelet            Container image "gcr.io/kubebuilder/kube-rbac-proxy:v0.8.0" already present on machine
  Normal   Created    11m                  kubelet            Created container kube-rbac-proxy
  Normal   Started    11m                  kubelet            Started container kube-rbac-proxy
  Warning  Failed     10m (x3 over 11m)    kubelet            Failed to pull image "xebialabs/deploy-operator:22.3.X": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/xebialabs/deploy-operator:22.3.X": failed to resolve reference "docker.io/xebialabs/deploy-operator:22.3.X": docker.io/xebialabs/deploy-operator:22.3.X: not found
  Warning  Failed     10m (x3 over 11m)    kubelet            Error: ErrImagePull
  Warning  Failed     9m50s (x6 over 11m)  kubelet            Error: ImagePullBackOff
  Normal   Pulling    9m38s (x4 over 11m)  kubelet            Pulling image "xebialabs/deploy-operator:22.3.X"
  Normal   BackOff    57s (x43 over 11m)   kubelet            Back-off pulling image "xebialabs/deploy-operator:22.3.X"
```

The events have reason of the failure: `Failed to pull image "xebialabs/deploy-operator:22.3.X": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/xebialabs/deploy-operator:22.3.X": failed to resolve reference "docker.io/xebialabs/deploy-operator:22.3.X": docker.io/xebialabs/deploy-operator:22.3.X: not found`.

Ok, we can fix that by editing answers file from previous installation and run same installation again.
We edit the digitalai/generated_answers_dai-deploy_ns-yourname_install-20221031-230614.yaml and change the line from:

```
OperatorImageDeployGeneric: xebialabs/deploy-operator:22.3.X
```

to

```
OperatorImageDeployGeneric: xebialabs/deploy-operator:22.3.1
```

Repeat installation with answers file

```shell
xl kube install --answers digitalai/generated_answers_dai-deploy_ns-yourname_install-20221031-230614.yaml
```

```text
$ xl kube install --answers digitalai/generated_answers_dai-deploy_ns-yourname_install-20221031-230614.yaml
? Following kubectl context will be used during execution: `xl-kube-workshop`? Yes
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | ARTLY8Qgl6FPLXN7                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-232810                                    |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageTag                       | 22.3.1                                             |
	| IngressHost                    | deploy-ns-yourname.westus2.cloudapp.azure.com      |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| KeystorePassphrase             | HEXRzENbwPvn85YB                                   |
	| License                        | LS0tIExpY2Vuc2UgLS0tCkxpY2Vuc2UgdmVyc2lvbjogMwpQ.. |
	| LicenseFile                    | ./xld-license.lic                                  |
	| LicenseSource                  | file                                               |
	| Namespace                      | ns-yourname                                        |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeployGeneric     | xebialabs/deploy-operator:22.3.1                   |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | xl-kube-workshop-file-storage-class                |
	| ProcessType                    | install                                            |
	| PvcSizeCc                      | 0.500000                                           |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | broken-storage                                     |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabs                                          |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | xl-kube-workshop-file-storage-class                |
	| UseCustomNamespace             | true                                               |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_install-20221031-232810.yaml
Starting install processing.
Created keystore digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/repository-keystore.jceks
Skip creating namespace ns-yourname, already exists
Update central configuration values... - Using custom resource name dai-xld-ns-yourname
Generated files successfully for AzureAKS installation.
Applying resources to the cluster!
? Do you want to replace the resource clusterrole/deploy-ns-yourname-operator-proxy-role with specification from file
digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/cluster-role-digital-proxy-role.yaml: Yes
Applied resource clusterrole/deploy-ns-yourname-operator-proxy-role from the file digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/cluster-role-digital-proxy-role.yaml
? Do you want to replace the resource clusterrole/deploy-ns-yourname-operator-manager-role with specification from file
digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/cluster-role-manager-role.yaml: Yes
Applied resource clusterrole/deploy-ns-yourname-operator-manager-role from the file digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/cluster-role-manager-role.yaml
? Do you want to replace the resource clusterrole/deploy-ns-yourname-operator-metrics-reader with specification from file
digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/cluster-role-metrics-reader.yaml: Yes
Applied resource clusterrole/deploy-ns-yourname-operator-metrics-reader from the file digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/cluster-role-metrics-reader.yaml
? Do you want to replace the resource service/xld-operator-controller-manager-metrics-service with specification from file
digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/controller-manager-metrics-service.yaml: Yes
Applied resource service/xld-operator-controller-manager-metrics-service from the file digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/controller-manager-metrics-service.yaml
? Do you want to replace the resource customresourcedefinition/digitalaideploys.xld.digital.ai with specification from file
digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/custom-resource-definition.yaml: Yes
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/custom-resource-definition.yaml
? Do you want to replace the resource deployment/xld-operator-controller-manager with specification from file
digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/deployment.yaml: Yes
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/deployment.yaml
? Do you want to replace the resource role/xld-operator-leader-election-role with specification from file
digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/leader-election-role.yaml: Yes
Applied resource role/xld-operator-leader-election-role from the file digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/leader-election-role.yaml
? Do you want to replace the resource rolebinding/xld-operator-leader-election-rolebinding with specification from file
digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/leader-election-rolebinding.yaml: Yes
Applied resource rolebinding/xld-operator-leader-election-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/leader-election-rolebinding.yaml
? Do you want to replace the resource clusterrolebinding/deploy-ns-yourname-operator-manager-rolebinding with specification from file
digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/manager-rolebinding.yaml: Yes
Applied resource clusterrolebinding/deploy-ns-yourname-operator-manager-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/manager-rolebinding.yaml
? Do you want to replace the resource clusterrolebinding/deploy-ns-yourname-operator-proxy-rolebinding with specification from file
digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/proxy-rolebinding.yaml: Yes
Applied resource clusterrolebinding/deploy-ns-yourname-operator-proxy-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/template/proxy-rolebinding.yaml
? Do you want to replace the resource digitalaideploy/dai-xld-ns-yourname with specification from file
digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/dai-deploy_cr.yaml: Yes
Applied resource digitalaideploy/dai-xld-ns-yourname from the file digitalai/dai-deploy/ns-yourname/20221031-232810/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```

So now we have progress, the operator pod will be ready and running. We can go on the next problem.

## Using wrong storage class on rabbitmq

To detect next problem run again, reuse the answers file from previous check run:

```shell
xl kube check --wait-for-ready 1 --skip-collecting --answers digitalai/generated_answers_dai-deploy_ns-yourname_check-20221031-231543.yaml
```

Here is example of running that command:
```text
$ xl kube check --wait-for-ready 1 --skip-collecting --answers digitalai/generated_answers_dai-deploy_ns-yourname_check-20221031-231543.yaml
? Following kubectl context will be used during execution: `docker-desktop`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS 
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
? Product server you want to perform check for: dai-deploy [Digital.ai Deploy]
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221103-140036                                    |
	| IngressType                    | none                                               |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | ns-yourname                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| ProcessType                    | check                                              |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20221103-140036/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_check-20221103-140036.yaml
Collecting the CR data
Waiting for resources to be ready
Deployment deployment/xld-operator-controller-manager is available in the namespace ns-yourname
Failed with waiting for the persitent volume claim data-dai-xld-ns-yourname-rabbitmq-0 in the namespace ns-yourname with error: timeout while waiting for pvc/data-dai-xld-ns-yourname-rabbitmq-0 to be jsonpath=.status.phase=Bound
 For more details check:
 - Check the PVC describe events with the describe command: kubectl describe pvc data-dai-xld-ns-yourname-rabbitmq-0 -n ns-yourname
 - Check the PVC StorageClass if it is compatible with the defined persitence access mode
 - Check the cluster resources for the StorageClass can provide the defined storage size
Saved resource data for pvc/data-dai-xld-ns-yourname-rabbitmq-0
Saved resource data for events/dai-xld-ns-yourname-digitalai-deploy-worker-0.1724148709579cdb
Saved resource data for events/dai-xld-ns-yourname-digitalai-deploy-cc-server-0.1724148709804c83
Saved resource data for events/dai-xld-ns-yourname-postgresql-0.1724148709034734
Saved resource data for events/dai-xld-ns-yourname-digitalai-deploy-worker-0.17241487541f3458
Saved resource data for events/dai-xld-ns-yourname-digitalai-deploy-worker-0.1724148755767ead
Saved resource data for events/dai-xld-ns-yourname-digitalai-deploy-worker-0.17241487597f1c3a
Saved resource data for events/dai-xld-ns-yourname-digitalai-deploy-cc-server-0.17241487768d8488
Saved resource data for events/dai-xld-ns-yourname-digitalai-deploy-cc-server-0.17241488ec60e8a3
Saved resource data for events/dai-xld-ns-yourname-digitalai-deploy-cc-server-0.17241488f21a8715
Saved resource data for events/dai-xld-ns-yourname-digitalai-deploy-cc-server-0.17241488e97d430c
Stopping because error during wait for the persitent volume claim data-dai-xld-ns-yourname-rabbitmq-0
```

In the last line we can see that rabbitmq PVC has a problems `Stopping because error during wait for the persitent volume claim data-dai-xld-ns-yourname-rabbitmq-0`.
From log we have suggestion to check describe of the PVC:

    - Check the PVC describe events with the describe command: kubectl describe pvc data-dai-xld-ns-yourname-rabbitmq-0 -n ns-yourname

That command is displaying following in the events section:
```text
$ kubectl describe pvc data-dai-xld-ns-yourname-rabbitmq-0 -n ns-yourname
Name:          data-dai-xld-ns-yourname-rabbitmq-0
Namespace:     ns-yourname
StorageClass:  broken-storage
Status:        Pending
Volume:
Labels:        app.kubernetes.io/instance=dai-xld-ns-yourname
               app.kubernetes.io/name=rabbitmq
Annotations:   volume.beta.kubernetes.io/storage-provisioner: k8s.io/no-provisioner
               volume.kubernetes.io/storage-provisioner: k8s.io/no-provisioner
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:
Access Modes:
VolumeMode:    Filesystem
Used By:       dai-xld-ns-yourname-rabbitmq-0
Events:
  Type    Reason                Age                   From                         Message
  ----    ------                ----                  ----                         -------
  Normal  ExternalProvisioning  47s (x10 over 9m14s)  persistentvolume-controller  waiting for a volume to be created, either by external provisioner "k8s.io/no-provisioner" or manually created by system administrator

```

The message in the events shows that StorageClass provisioner is not creating the volume: `waiting for a volume to be created, either by external provisioner "k8s.io/no-provisioner" or manually created by system administrator`.
So we can fix the provisioner or change StorageClass, we will change StorageClass.

We can again edit same answers file and change it with correct disk storage class:
From:

```
RabbitmqStorageClass: 'broken-storage'
```

To

```
RabbitmqStorageClass: 'xl-kube-workshop-file-storage-class'
```

On minikube and Docker Desktop use here your default storage class!

Repeat installation with answers file, but clean before, because storage class cannot be updated on the PVC and use `--skip-prompts` to avoid answering all checks.

⚠️ During running next clean command, do not delete CRD because there are other installation that are reusing same CRD!

Cleaning takes some time because we need to wait the pods to timeout initialization.

Run following:

```shell
xl kube clean --skip-prompts
xl kube install --skip-prompts --answers digitalai/generated_answers_dai-deploy_ns-yourname_install-20221031-230614.yaml
```

Here is example of the installation output:

```text
xl kube install --skip-prompts --answers digitalai/generated_answers_dai-deploy_ns-yourname_install-20221031-230614.yaml
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | ARTLY8Qgl6FPLXN7                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221101-000553                                    |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageTag                       | 22.3.1                                             |
	| IngressHost                    | deploy-ns-yourname.westus2.cloudapp.azure.com |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| KeystorePassphrase             | HEXRzENbwPvn85YB                                   |
	| License                        | LS0tIExpY2Vuc2UgLS0tCkxpY2Vuc2UgdmVyc2lvbjogMwpQ.. |
	| LicenseFile                    | ./xld-license.lic                                  |
	| LicenseSource                  | file                                               |
	| Namespace                      | ns-yourname                                       |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeployGeneric     | xebialabs/deploy-operator:22.3.1                   |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | xl-kube-workshop-file-storage-class          |
	| ProcessType                    | install                                            |
	| PvcSizeCc                      | 0.500000                                           |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | xl-kube-workshop-file-storage-class          |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabs                                          |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | xl-kube-workshop-file-storage-class          |
	| UseCustomNamespace             | true                                               |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_install-20221101-000553.yaml
Starting install processing.
Created keystore digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/repository-keystore.jceks
Skip creating namespace ns-yourname, already exists
Update central configuration values... - Using custom resource name dai-xld-ns-yourname
Generated files successfully for AzureAKS installation.
Applying resources to the cluster!
Applied resource clusterrole/deploy-ns-yourname-operator-proxy-role from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/cluster-role-digital-proxy-role.yaml
Applied resource clusterrole/deploy-ns-yourname-operator-manager-role from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/cluster-role-manager-role.yaml
Applied resource clusterrole/deploy-ns-yourname-operator-metrics-reader from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/cluster-role-metrics-reader.yaml
Applied resource service/xld-operator-controller-manager-metrics-service from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/deployment.yaml
Applied resource role/xld-operator-leader-election-role from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xld-operator-leader-election-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrolebinding/deploy-ns-yourname-operator-manager-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/manager-rolebinding.yaml
Applied resource clusterrolebinding/deploy-ns-yourname-operator-proxy-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalaideploy/dai-xld-ns-yourname from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```


## Using wrong storage class on postgresql 

To detect next problem run again, reuse the answers file from previous check run:

```shell
xl kube check --wait-for-ready 1 --skip-collecting --answers digitalai/generated_answers_dai-deploy_ns-yourname_check-20221031-231543.yaml
```


Note: for the Minikube and Docker Desktop users this problem will have same way of detection and same fix as for `RabbitmqStorageClass` answer!
So you can try to resolve it on your own, rest of document is describing the problem on the Azure. 


Here is example of running that command:

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

On minikube and Docker Desktop use here your default storage class!


Repeat installation with answers file, but clean before, because storage class cannot be updated on the PVC and use `--skip-prompts` to avoid answering all checks.

⚠️ During running next clean command, do not delete CRD because there are other installation that are reusing same CRD!

Cleaning takes some time because we need to wait the pods to timeout initialization.

Run following:

```shell
xl kube clean --skip-prompts
xl kube install --skip-prompts --answers digitalai/generated_answers_dai-deploy_ns-yourname_install-20221031-230614.yaml
```

Here is example of the installation output:

```text
xl kube install --skip-prompts --answers digitalai/generated_answers_dai-deploy_ns-yourname_install-20221031-230614.yaml
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | ARTLY8Qgl6FPLXN7                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221101-000553                                    |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageTag                       | 22.3.1                                             |
	| IngressHost                    | deploy-ns-yourname.westus2.cloudapp.azure.com |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| KeystorePassphrase             | HEXRzENbwPvn85YB                                   |
	| License                        | LS0tIExpY2Vuc2UgLS0tCkxpY2Vuc2UgdmVyc2lvbjogMwpQ.. |
	| LicenseFile                    | ./xld-license.lic                                  |
	| LicenseSource                  | file                                               |
	| Namespace                      | ns-yourname                                       |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeployGeneric     | xebialabs/deploy-operator:22.3.1                   |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | xl-kube-workshop-disk-storage-class          |
	| ProcessType                    | install                                            |
	| PvcSizeCc                      | 0.500000                                           |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | xl-kube-workshop-file-storage-class          |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabs                                          |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | xl-kube-workshop-file-storage-class          |
	| UseCustomNamespace             | true                                               |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_install-20221101-000553.yaml
Starting install processing.
Created keystore digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/repository-keystore.jceks
Skip creating namespace ns-yourname, already exists
Update central configuration values... - Using custom resource name dai-xld-ns-yourname
Generated files successfully for AzureAKS installation.
Applying resources to the cluster!
Applied resource clusterrole/deploy-ns-yourname-operator-proxy-role from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/cluster-role-digital-proxy-role.yaml
Applied resource clusterrole/deploy-ns-yourname-operator-manager-role from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/cluster-role-manager-role.yaml
Applied resource clusterrole/deploy-ns-yourname-operator-metrics-reader from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/cluster-role-metrics-reader.yaml
Applied resource service/xld-operator-controller-manager-metrics-service from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/deployment.yaml
Applied resource role/xld-operator-leader-election-role from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xld-operator-leader-election-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrolebinding/deploy-ns-yourname-operator-manager-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/manager-rolebinding.yaml
Applied resource clusterrolebinding/deploy-ns-yourname-operator-proxy-rolebinding from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalaideploy/dai-xld-ns-yourname from the file digitalai/dai-deploy/ns-yourname/20221101-000553/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```

We can now run final check to see if everything is running OK:

```text
xl kube check --skip-prompts --wait-for-ready 5 --skip-collecting --answers digitalai/generated_answers_dai-deploy_ns-yourname_check-20221031-231543.yaml
```

Output is now OK:

```text
$ xl kube check --skip-prompts --wait-for-ready 5 --skip-collecting --answers digitalai/generated_answers_dai-deploy_ns-yourname_check-20221031-231543.yaml
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

---

[Next](../part-3/lab-8-change-configuration-file.md)

