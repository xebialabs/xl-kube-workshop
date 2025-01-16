
# Lab 8 - How to change configuration file

Here we will update the configuration file on the central configuration server.

## Custom configuration of `deploy-repository.yaml.template` on CC pods

### Steps

1. Download current template configuration file that exists on cc Deploy pod that is running.
   For example, if you want to update `deploy-repository.yaml.template`, it is in the 
   `/opt/xebialabs/central-configuration-server/central-conf/deploy-repository.yaml.template` in master pod.

   Lets first define the variables to make the process easier

   ```shell
   export FILE_TO_UPDATE=deploy-repository.yaml.template
   export TEMPLATE_DIR=central-conf
   export TARGET_DIR=centralConfiguration
   ```

   Download the `deploy-repository.yaml.template` from a CC pod.

    ```shell
    kubectl cp -c deploy \
   ns-yourname/dai-xld-ns-yourname-digitalai-deploy-cc-server-0:$TEMPLATE_DIR/$FILE_TO_UPDATE \
   $FILE_TO_UPDATE
    ```

2. Make our changes to the file

   ```shell
   vi $FILE_TO_UPDATE
   ```
   
3. Create a patch file

   ```shell
   c=$(cat $FILE_TO_UPDATE) \
   contentPath=".spec.centralConfiguration.extraConfiguration.${TEMPLATE_DIR}_${FILE_TO_UPDATE//./-}.content" \
   f="${TEMPLATE_DIR}/$FILE_TO_UPDATE" \
   filePath=".spec.centralConfiguration.extraConfiguration.${TEMPLATE_DIR}_${FILE_TO_UPDATE//./-}.path" \
   yq -n 'eval(strenv(contentPath)) = strenv(c) | eval(strenv(filePath)) = strenv(f)' \
   > "$FILE_TO_UPDATE.patch.yaml"
   ```

4. Apply the patch file

   ```shell
   kubectl patch -n ns-yourname digitalaideploys.xld.digital.ai dai-xld-ns-yourname \
     --type=merge --patch-file $FILE_TO_UPDATE.patch.yaml
   ```
   
   The pods will now restart central configuration pod, after which we should be able to see our changes in the container.

5. Restart master and worker Deploy pods, by deleting statefulsets

    ```shell
    kubectl delete sts dai-xld-ns-yourname-digitalai-deploy-master
    kubectl delete sts dai-xld-ns-yourname-digitalai-deploy-worker
    ```
    
    The pods will now restart with the new configuration.

---

That's all Folks!
