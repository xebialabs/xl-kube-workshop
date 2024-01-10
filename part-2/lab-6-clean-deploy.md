
# Lab 6 - Clean

Now we will learn how to clean the cluster from our installation of the Digital.ai Deploy.

Run following to clean the cluster from the installed resources:

```shell
xl kube clean
```

⚠️Note that in case if you are sharing cluster with others, do not delete the CR that is shared between all installations.
So you need to answer always to `Should CRD be reused, if No we will delete the CRD digitalaideploys.xld.digital.ai, and all related CRs will be deleted with it:` with **Yes**.

With following example we are deleting installed resources along with PVC and CRD (example on minikube):

```text
$ xl kube clean
? Following kubectl context will be used during execution: `minikube`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: PlainK8s [Plain multi-node K8s cluster]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
? Product server you want to perform clean for: dai-deploy [Digital.ai Deploy]
? Enter the name of custom resource definition you want to reuse or replace: digitalaideploys.xld.digital.ai
? Should CRD be reused, if No we will delete the CRD digitalaideploys.xld.digital.ai, and all related CRs will be deleted with it: No
? Enter the name of custom resource: dai-xld-ns-yourname
? Should we preserve persisted volume claims? If not all volume data will be lost: No
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CrName                         | dai-xld-ns-yourname                                |
	| CrdName                        | digitalaideploys.xld.digital.ai                    |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20240108-232336                                    |
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
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20240108-232336/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_clean-20240108-232336.yaml 
Cleaning the resources on the cluster!
CR dai-xld-ns-yourname is available, deleting
? Do you want to delete the resource digitalaideploys.xld.digital.ai/dai-xld-ns-yourname: Yes
Deleted digitalaideploys.xld.digital.ai/dai-xld-ns-yourname from namespace ns-yourname
Deleting statefulsets
? Do you want to delete the resource sts/dai-xld-ns-yourname-digitalai-deploy-master: Yes
Deleted sts/dai-xld-ns-yourname-digitalai-deploy-master from namespace ns-yourname
? Do you want to delete the resource sts/dai-xld-ns-yourname-digitalai-deploy-worker: Yes
Deleted sts/dai-xld-ns-yourname-digitalai-deploy-worker from namespace ns-yourname
? Do you want to delete the resource sts/dai-xld-ns-yourname-postgresql: Yes
Deleted sts/dai-xld-ns-yourname-postgresql from namespace ns-yourname (already deleted)
? Do you want to delete the resource sts/dai-xld-ns-yourname-rabbitmq: Yes
Deleted sts/dai-xld-ns-yourname-rabbitmq from namespace ns-yourname (already deleted)
Deleting deployments
? Do you want to delete the resource deployment/xld-operator-controller-manager: Yes
Deleted deployment/xld-operator-controller-manager from namespace ns-yourname
Deleting jobs
Deleting services
? Do you want to delete the resource svc/xld-operator-controller-manager-metrics-service: Yes
Deleted svc/xld-operator-controller-manager-metrics-service from namespace ns-yourname
Deleting secrets
? Do you want to delete the resource secret/dai-xld-ns-yourname-digitalai-deploy-license: Yes
Deleted secret/dai-xld-ns-yourname-digitalai-deploy-license from namespace ns-yourname
Deleting configmaps
? Do you want to delete the resource configmap/xld-operator-controller-manager: Yes
Deleted configmap/xld-operator-controller-manager from namespace ns-yourname
? Do you want to delete the resource ingressclass/nginx-dai-xld-ns-yourname: Yes
Deleted ingressclass/nginx-dai-xld-ns-yourname from namespace ns-yourname
Deleting roles
? Do you want to delete the resource role/xld-operator-leader-election: Yes
Deleted role/xld-operator-leader-election from namespace ns-yourname
? Do you want to delete the resource role/xld-operator-manager: Yes
Deleted role/xld-operator-manager from namespace ns-yourname
? Do you want to delete the resource role/xld-operator-proxy: Yes
Deleted role/xld-operator-proxy from namespace ns-yourname
? Do you want to delete the resource clusterrole/dai-xld-ns-yourname-nginx-ingress-controller: Yes
Deleted clusterrole/dai-xld-ns-yourname-nginx-ingress-controller from namespace ns-yourname
? Do you want to delete the resource clusterrole/ns-yourname-xld-operator-manager: Yes
Deleted clusterrole/ns-yourname-xld-operator-manager from namespace ns-yourname
? Do you want to delete the resource rolebinding/xld-operator-leader-election: Yes
Deleted rolebinding/xld-operator-leader-election from namespace ns-yourname
? Do you want to delete the resource rolebinding/xld-operator-manager: Yes
Deleted rolebinding/xld-operator-manager from namespace ns-yourname
? Do you want to delete the resource rolebinding/xld-operator-proxy: Yes
Deleted rolebinding/xld-operator-proxy from namespace ns-yourname
? Do you want to delete the resource clusterrolebinding/dai-xld-ns-yourname-nginx-ingress-controller: Yes
Deleted clusterrolebinding/dai-xld-ns-yourname-nginx-ingress-controller from namespace ns-yourname
? Do you want to delete the resource clusterrolebinding/ns-yourname-xld-operator-manager: Yes
Deleted clusterrolebinding/ns-yourname-xld-operator-manager from namespace ns-yourname
Deleting PVCs
? Do you want to delete the resource pvc/data-dai-xld-ns-yourname-postgresql-0: Yes
Deleted pvc/data-dai-xld-ns-yourname-postgresql-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dai-xld-ns-yourname-rabbitmq-0: Yes
Deleted pvc/data-dai-xld-ns-yourname-rabbitmq-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-0: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-0: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-0 from namespace ns-yourname
CRD digitalaideploys.xld.digital.ai is available, deleting
? Do you want to delete the resource crd/digitalaideploys.xld.digital.ai: Yes
Deleted crd/digitalaideploys.xld.digital.ai from namespace ns-yourname
? Do you want to delete the resource crd/digitalaideploys.xld.digital.ai: Yes
Deleted crd/digitalaideploys.xld.digital.ai from namespace ns-yourname (already deleted)
Clean finished successfully!
```

The clean process is cleaning everything from the cluster. For each resource it is asking for confirmation before delete.

For the other questions and answers details check [XL Kube Command Reference](https://docs.digital.ai/bundle/devops-deploy-version-v.24.1/page/deploy/operator/xl-kube.html#xl-kube-clean)

---

[Next](../part-3/lab-7-troubleshoot.md)
