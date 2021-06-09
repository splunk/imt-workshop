# Deploying NGINX in EKS - Lab Summary

* Deploy a NGINX ReplicaSet into your EKS cluster and confirm the discovery of your NGINX deployment.
* Run a load test to create metrics and confirm them streaming into Splunk Observability Cloud!

---

## 1. Start your NGINX

Verify the number of pods running in the Splunk UI by selecting the **WORKLOADS** tab. This should give you an overview of the workloads on your cluster.

![Workload Agent](../images/otel/k8s-workloads.png)

Note the single agent container running per node among the default Kubernetes pods. This single container will monitor all the pods and services being deployed on this node!

Now switch back to the default cluster node view by selecting the **MAP** tab and select your cluster again.

In the Multipass or AWS/EC2 shell session and change into the `nginx` directory:

=== "Shell Command"

    ```text
    cd ~/workshop/k8s/nginx
    ```
  
---

## 2. Create NGINX deployment

Create the NGINX `configmap`[^1] using the `nginx.conf` file:

=== "Shell Command"

    ```text
    kubectl create configmap nginxconfig --from-file=nginx.conf
    ```

=== "Output"

    ```
    configmap/nginxconfig created
    ```

Then create the deployment:

=== "Shell Command"

    ```
    kubectl create -f nginx-deployment.yaml
    ```

=== "Output"

    ```
    deployment.apps/nginx created
    service/nginx created
    ```

Next we will confirm that NGINX is running as Kubernetes service

=== "Shell Command"

    ```
    kubectl get svc
    ```


Validate the deployment has been successful. NGINX will be assigned an external IP. It will take a few minutes for the Elastic Load Balancer to be available. Grab the External-IP of NGINX service and visit the page using browser on your computer.

If you have the Splunk UI open you should see new Pods being started and containers being deployed.

It should only take around 20 seconds for the pods to transition into a Running state. In the Splunk UI you will have a cluster that looks like below:

![back to Cluster](../images/otel/cluster.png)

If you select the **WORKLOADS** tab again you will now see that there is a new ReplicaSet and a deployment added for NGINX:

![NGINX loaded](../images/otel/k8s-workloads-nginx.png)

---

Let's validate this in your shell as well:

=== "Shell Command"

    ```text
    kubectl get pods
    ```

=== "Output"

    ```text
    NAME                                                          READY   STATUS    RESTARTS   AGE
    splunk-otel-collector-k8s-cluster-receiver-77784c659c-ttmpk   1/1     Running   0          9m19s
    splunk-otel-collector-agent-249rd                             1/1     Running   0          9m19s
    svclb-nginx-vtnzg                                             1/1     Running   0          5m57s
    nginx-7b95fb6b6b-7sb9x                                        1/1     Running   0          5m57s
    nginx-7b95fb6b6b-lnzsq                                        1/1     Running   0          5m57s
    nginx-7b95fb6b6b-hlx27                                        1/1     Running   0          5m57s
    nginx-7b95fb6b6b-zwns9                                        1/1     Running   0          5m57s
    ```


Validate you are seeing metrics in the UI by going to hamburger icon, top let and select **Dashboards → NGINX → NGINX Servers**. Using the **Overrides** filter on `k8s.cluster.name:`, find the name of your cluster as returned by `echo $(hostname)-eks-cluster` in the terminal.

![NGINX Dashboard](../images/otel/nginx-dashboard.png)

[^1]: A ConfigMap is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume. A ConfigMap allows you to decouple environment-specific configuration from your container images, so that your applications are easily portable.
