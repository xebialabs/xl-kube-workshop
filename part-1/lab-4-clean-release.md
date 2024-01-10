
# Lab 4 - Clean

Now we will learn how to clean the cluster from our installation of  Digital.ai Release.

Run the following command to clean the cluster from the installed resources:

```shell
xl kube clean --skip-prompts
```

⚠️Note that in case if you are sharing cluster with others, do not delete the CR that is shared between all installations.
So you need to answer to `Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it:` with **Yes**.

The following example deletes installed resources along with PVC. Only the CRD remains on the cluster (example on the Azure):

```text
? xl kube clean
? Following kubectl context will be used during execution: `minikube`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: PlainK8s [Plain multi-node K8s cluster]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
? Product server you want to perform clean for: dai-release [Digital.ai Release with optional Remote Runner]
? Enter the name of custom resource definition you want to reuse or replace: digitalaireleases.xlr.digital.ai
⚠️? Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it: No
? Enter the name of custom resource: dai-xlr-ns-yourname
⚠️? Should we preserve persisted volume claims? If not all volume data will be lost: No
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CrName                         | dai-xlr-ns-yourname                                |
	| CrdName                        | digitalaireleases.xlr.digital.ai                   |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240108-115438                                    |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCrdReused                    | false                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| Namespace                      | ns-yourname                                        |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| PreservePvc                    | false                                              |
	| ProcessType                    | clean                                              |
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
For current process files will be generated in the: digitalai/dai-release/ns-yourname/20240108-115438/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release_ns-yourname_clean-20240108-115438.yaml 
Cleaning the resources on the cluster!
CR dai-xlr-ns-yourname is available, deleting
? Do you want to delete the resource digitalaireleases.xlr.digital.ai/dai-xlr-ns-yourname: Yes
Deleted digitalaireleases.xlr.digital.ai/dai-xlr-ns-yourname from namespace ns-yourname
Deleting statefulsets
Deleting deployments
? Do you want to delete the resource deployment/xlr-operator-controller-manager: Yes
Deleted deployment/xlr-operator-controller-manager from namespace ns-yourname
Deleting jobs
Deleting services
? Do you want to delete the resource svc/xlr-operator-controller-manager-metrics-service: Yes
Deleted svc/xlr-operator-controller-manager-metrics-service from namespace ns-yourname
Deleting secrets
? Do you want to delete the resource secret/dai-xlr-ns-yourname-digitalai-release-license: Yes
Deleted secret/dai-xlr-ns-yourname-digitalai-release-license from namespace ns-yourname
Deleting configmaps
? Do you want to delete the resource configmap/xlr-operator-controller-manager: Yes
Deleted configmap/xlr-operator-controller-manager from namespace ns-yourname
Deleting roles
? Do you want to delete the resource role/xlr-operator-leader-election: Yes
Deleted role/xlr-operator-leader-election from namespace ns-yourname
? Do you want to delete the resource role/xlr-operator-manager: Yes
Deleted role/xlr-operator-manager from namespace ns-yourname
? Do you want to delete the resource role/xlr-operator-proxy: Yes
Deleted role/xlr-operator-proxy from namespace ns-yourname
? Do you want to delete the resource clusterrole/ns-yourname-xlr-operator-manager: Yes
Deleted clusterrole/ns-yourname-xlr-operator-manager from namespace ns-yourname
? Do you want to delete the resource rolebinding/xlr-operator-leader-election: Yes
Deleted rolebinding/xlr-operator-leader-election from namespace ns-yourname
? Do you want to delete the resource rolebinding/xlr-operator-manager: Yes
Deleted rolebinding/xlr-operator-manager from namespace ns-yourname
? Do you want to delete the resource rolebinding/xlr-operator-proxy: Yes
Deleted rolebinding/xlr-operator-proxy from namespace ns-yourname
? Do you want to delete the resource clusterrolebinding/ns-yourname-xlr-operator-manager: Yes
Deleted clusterrolebinding/ns-yourname-xlr-operator-manager from namespace ns-yourname
Deleting PVCs
? Do you want to delete the resource pvc/dai-xlr-ns-yourname-digitalai-release: Yes
Deleted pvc/dai-xlr-ns-yourname-digitalai-release from namespace ns-yourname
? Do you want to delete the resource pvc/data-dai-xlr-ns-yourname-postgresql-0: Yes
Deleted pvc/data-dai-xlr-ns-yourname-postgresql-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dai-xlr-ns-yourname-rabbitmq-0: Yes
Deleted pvc/data-dai-xlr-ns-yourname-rabbitmq-0 from namespace ns-yourname
CRD digitalaireleases.xlr.digital.ai is available, deleting
? Do you want to delete the resource crd/digitalaireleases.xlr.digital.ai: Yes
Deleted crd/digitalaireleases.xlr.digital.ai from namespace ns-yourname
? Do you want to delete the resource crd/digitalaireleases.xlr.digital.ai: Yes
Deleted crd/digitalaireleases.xlr.digital.ai from namespace ns-yourname (already deleted)
Clean finished successfully!
```

The clean process is cleaning everything from the cluster. 

For the other questions and answers details check [XL Kube Command Reference](https://docs.digital.ai/bundle/devops-release-version-v.24.1/page/release/operator/xl-kube.html#xl-kube-clean)

---

[Next](../part-2/lab-5-install-deploy.md)
