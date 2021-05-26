### Setting up Kubernetes Logging by Deploying Splunk Connect for Kubernetes

=== "Shell Command"
    ```
    cd splunk
            
    export HEC_TOKEN=# paste your hec token value
            
    export HEC_HOST=# paste Splunk Cloud host e.g. prd-p-ox1qg.splunkcloud.com
    ```
Deploy SCK as a DaemonSet using Helm in Kubernetes namespace splunk

=== "Shell Command"
    ```
    kubectl create ns splunk
    ```
Deploy SCK Helm Chart

=== "Shell Command"
    ```
    helm install \
    splunk --set global.splunk.hec.token=$HEC_TOKEN \
    --set global.splunk.hec.host=$HEC_HOST \
    --namespace splunk -f values.yaml https://github.com/splunk/splunk-connect-for-kubernetes/releases/download/1.4.1/splunk-connect-for-kubernetes-1.4.1.tgz
    ```
=== "OUTPUT"
    ```text
    NAME: splunk
    LAST DEPLOYED: Sat Jul 25 21:12:34 2020
    NAMESPACE: splunk
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
    NOTES:
    ███████╗██████╗ ██╗     ██╗   ██╗███╗   ██╗██╗  ██╗██╗
    ██╔════╝██╔══██╗██║     ██║   ██║████╗  ██║██║ ██╔╝╚██╗
    ███████╗██████╔╝██║     ██║   ██║██╔██╗ ██║█████╔╝  ╚██╗
    ╚════██║██╔═══╝ ██║     ██║   ██║██║╚██╗██║██╔═██╗  ██╔╝
    ███████║██║     ███████╗╚██████╔╝██║ ╚████║██║  ██╗██╔╝
    ╚══════╝╚═╝     ╚══════╝ ╚═════╝ ╚═╝  ╚═══╝╚═╝  ╚═╝╚═╝

    Listen to your data.

    Splunk Connect for Kubernetes is spinning up in your cluster.
    After a few minutes, you should see data being indexed in your Splunk.

    If you get stuck, we're here to help.
    Look for answers here: http://docs.splunk.com
    ```    
