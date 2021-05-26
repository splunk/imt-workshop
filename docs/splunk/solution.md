### Exercises and solutions
Your mission, should you choose to accept it is to solve the mystery why 
ImageSample and Hungry-job are stuck in Pending status.

Hint: Use monitoring and troubleshooting workflow from SignalFx to Splunk Cloud. Contextual logs will guide you towards the solutions. 

Fixes are available in /fixes directory.

Hungry Cron Job is requesting too many resources. 5000 CPU millicores (5 whole CPUs) and 32GB of memory! That is why Kubernetes is not scheduling the pod even though it is being requested every minute by the cronjob. Let's fix these resource requests to a more reasonable number 

=== "Shell Command"
    ```
    kubectl patch cronjob hungry-job --patch="$(cat apps/fixes/hungry-fix.yaml)"
    ```
ImageSample pod is pending because it was trying to use an image which does not exist in the container registry provided in the pod spec.<br/><br/> 

=== "Shell Command"
    ```
    kubectl patch deployment imagesample --patch="$(cat apps/fixes/imagesample-fix.yaml)"
    ```
After a few seconds check Kubernetes Navigator to make sure your patches have been applied successfully.
![Errors Corrected](../images/splunk/errors-corrected.png){: .zoom} 

