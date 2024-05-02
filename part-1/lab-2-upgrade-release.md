
# Lab 2 - Upgrade Release 24.1.0

In this lab we will upgrade the previous installation to Digital.ai Release 24.1.0.

Just start upgrade process by running:

```shell
xl kube upgrade --skip-prompts
```

...and answer all the questions. Some of the questions are repeated from the installation.

* In this case we are sharing the cluster with others, so do NOT delete the Custom Resource that is shared between all installations.
  Answer the question `Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it:` with **Yes**.

* **For Azure:** All custom changes in the CR after installation must be preserved. So for the answer  `Edit list of custom resource keys that will migrate to the new Release CR:` you need to add all keys under which you changed the values. We have one example from previous lab `spec.nginx-ingress-controller.service.annotations`, because of that we need to add key in the list, in the new line `.spec.nginx-ingress-controller.service.annotations`.

* **For Minikube:** All custom changes in the CR after installation must be preserved. So for the answer  `Edit list of custom resource keys that will migrate to the new Release CR:` you need to add all keys under which you changed the values. We have one example from previous lab `.spec.nginx-ingress-controller.service.type`, because of that we need to add key in the list, in the new line `.spec.nginx-ingress-controller.service.type`.

* We are upgrading from Operator to Operator. Note: the `xl kube upgrade` command can also be used to upgrade helm-based installations from 10.2 and higher to operator based installation. 

For details of CR keys that will migrate to new CR, check [Custom resource keys migration during upgrade](https://docs.digital.ai/bundle/devops-release-version-v.24.1/page/release/operator/xl-op-upgrade-custom-resource-keys.html)

For the other questions and answers details, check [Upgrade Wizard for Digital.ai Release](https://docs.digital.ai/bundle/devops-release-version-v.24.1/page/release/operator/xl-op-upgrade-wizard-release.html)


Here is example of the upgrade answers, based on Azure:

```text
$ xl kube upgrade --skip-prompts

⚠️? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: PlainK8s [Plain multi-node K8s cluster]
⚠️? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
⚠️? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
? Product server you want to perform upgrade for: dai-release [Digital.ai Release with optional Remote Runner]
? Select the type of upgrade you want: operatorToOperator [Operator to Operator]
? Select type of image registry: default [Default (Uses various public image registries for the installation images)]
? Enter the repository name for the application and operator images (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): xebialabsunsupported
? Enter the Release image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): xl-release
⚠️? Enter the application image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 24.1.0-1226.113
? Type of the OIDC configuration: no-oidc [No OIDC Configuration]
? Enter the operator image to use (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): release-operator
⚠️? Enter the operator image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 24.1.0-1226.113
? Enter the name of custom resource definition you want to reuse or replace: digitalaireleases.xlr.digital.ai
⚠️? Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it: Yes
? Enter the name of custom resource: dai-xlr-ns-yourname
? Edit list of custom resource keys that will migrate to the new Release CR: <Received>
⚠️ Add: 
.spec.nginx-ingress-controller.service.annotations
? Should we preserve persisted volume claims? If not all volume data will be lost: Yes
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | true                                               |
	| CrName                         | dai-xlr-ns-yourname                                |
	| CrdName                        | digitalaireleases.xlr.digital.ai                   |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240108-114406                                    |
	| ImageNameRelease               | xl-release                                         |
	| ImageRegistryType              | default                                            |
	| ImageTag                       | 24.1.0-1226.113                                    |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCrdReused                    | true                                               |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| Namespace                      | ns-yourname                                        |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeUpgrade          | no-oidc                                            |
	| OperatorImageRelease           | release-operator                                   |
	| OperatorImageTag               | 24.1.0-1226.113                                    |
	| OsType                         | darwin                                             |
	| PreserveCrValuesRelease        | .metadata.name: .\n.spec.persistence.storageClas.. |
	| PreservePvc                    | true                                               |
	| ProcessType                    | upgrade                                            |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | false                                              |
	| RemoteRunnerInstallConfirm     | false                                              |
	| RemoteRunnerReleaseName        | remote-runner                                      |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| RepositoryName                 | xebialabsunsupported                               |
	| ServerType                     | dai-release                                        |
	| ShortServerName                | xlr                                                |
	| UpgradeType                    | operatorToOperator                                 |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-release/ns-yourname/20240108-111630/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release_ns-yourname_upgrade-20240108-111630.yaml 
Starting upgrade processing
Fetching values from cluster... -Upgrading from old version 23.3.2
Generated files successfully for operatorToOperator upgrade for PlainK8s.
CRD creation will be skipped, expecting to have CRD already on cluster
Cleaning the resources on the cluster!
CR dai-xlr-ns-yourname is available, deleting
Deleted digitalaireleases.xlr.digital.ai/dai-xlr-ns-yourname from namespace ns-yourname
Deleting statefulsets
Deleted sts/dai-xlr-ns-yourname-digitalai-release from namespace ns-yourname
Deleted sts/dai-xlr-ns-yourname-postgresql from namespace ns-yourname (already deleted)
Deleted sts/dai-xlr-ns-yourname-rabbitmq from namespace ns-yourname
Deleting deployments
Deleted deployment/xlr-operator-controller-manager from namespace ns-yourname
Deleting jobs
Deleting services
Deleted svc/xlr-operator-controller-manager-metrics-service from namespace ns-yourname
Deleting secrets
Deleted secret/sh.helm.release.v1.dai-xlr-ns-yourname.v1 from namespace ns-yourname
Deleting configmaps
Deleted configmap/xlr-operator-controller-manager from namespace ns-yourname
Deleted ingressclass/nginx-dai-xlr-ns-yourname from namespace ns-yourname
Deleting roles
Deleted role/xlr-operator-leader-election from namespace ns-yourname
Deleted role/xlr-operator-manager from namespace ns-yourname
Deleted role/xlr-operator-proxy from namespace ns-yourname
Deleted clusterrole/dai-xlr-ns-yourname-nginx-ingress-controller from namespace ns-yourname
Deleted clusterrole/ns-yourname-xlr-operator-manager from namespace ns-yourname
Deleted rolebinding/xlr-operator-leader-election from namespace ns-yourname
Deleted rolebinding/xlr-operator-manager from namespace ns-yourname
Deleted rolebinding/xlr-operator-proxy from namespace ns-yourname
Deleted clusterrolebinding/dai-xlr-ns-yourname-nginx-ingress-controller from namespace ns-yourname
Deleted clusterrolebinding/ns-yourname-xlr-operator-manager from namespace ns-yourname
Patch PVCs
Applying resources to the cluster!
Applied resource service/xlr-operator-controller-manager-metrics-service from the file digitalai/dai-release/ns-yourname/20240108-111630/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource deployment/xlr-operator-controller-manager from the file digitalai/dai-release/ns-yourname/20240108-111630/kubernetes/template/deployment.yaml
Applied resource role/xlr-operator-leader-election from the file digitalai/dai-release/ns-yourname/20240108-111630/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xlr-operator-leader-election from the file digitalai/dai-release/ns-yourname/20240108-111630/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrole/ns-yourname-xlr-operator-manager from the file digitalai/dai-release/ns-yourname/20240108-111630/kubernetes/template/manager-clusterrole.yaml
Applied resource clusterrolebinding/ns-yourname-xlr-operator-manager from the file digitalai/dai-release/ns-yourname/20240108-111630/kubernetes/template/manager-clusterrolebinding.yaml
Applied resource role/xlr-operator-manager from the file digitalai/dai-release/ns-yourname/20240108-111630/kubernetes/template/manager-role.yaml
Applied resource rolebinding/xlr-operator-manager from the file digitalai/dai-release/ns-yourname/20240108-111630/kubernetes/template/manager-rolebinding.yaml
Applied resource role/xlr-operator-proxy from the file digitalai/dai-release/ns-yourname/20240108-111630/kubernetes/template/proxy-role.yaml
Applied resource rolebinding/xlr-operator-proxy from the file digitalai/dai-release/ns-yourname/20240108-111630/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalairelease/dai-xlr-ns-yourname from the file digitalai/dai-release/ns-yourname/20240108-111630/kubernetes/dai-release_cr.yaml
Upgrade finished successfully!
```

The upgrade process is first cleaning everything from the cluster and after that applying the new version.


Use the following command to wait for the upgrade to be complete:

```shell
xl kube check --skip-collecting --skip-prompts --wait-for-ready 5
```

When done, reload Release in the browser and check the version number again. It should now be **Version 24.1.0**

Note: Minikube service ports are changed, check the service ports again to see which you need to use in the Release URL.

---

[Next](../part-1/lab-3-oidc-setup.md)
