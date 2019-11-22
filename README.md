# cb-core-oc-sa-demo
K8s configuration for Core v2 install for SA demo environment.

# Creates Masters and Jobs

In the demo OC run the following script:

```groovy
import jenkins.model.Jenkins
import hudson.ExtensionList

import java.nio.file.Path
import java.nio.file.Paths

import hudson.security.ACL
import jenkins.util.groovy.GroovyHookScript

import java.util.logging.Logger

String scriptName = "init_01_quickstart_hook.groovy"

Logger logger = Logger.getLogger(scriptName)

logger.info("Running quickstart hook")
//kickoff quickstart scripts once licensed and plugins are installed
ACL.impersonate(ACL.SYSTEM, new Runnable() {
    @Override
    public void run() {
      new GroovyHookScript("quickstart").run();
    }
});
```
Once the Ops Managed Master is up it will run a job for all branches of all Kypseli repos with an `OpsJenkinsfile`.

# Cleanup

In the demo OC run the following script:

```groovy
for(j in jenkins.model.Jenkins.theInstance.getAllItems()) {
    j.delete()
}
```
Next cleanup all the K8s resources:

```
kubectl -n cloud-run delete statefulset,pod,pvc,ingress,service --all
kubectl -n core-demo delete statefulset,pod,pvc,ingress,service -l type=master
```
