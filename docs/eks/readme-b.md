# Steps to access EKS cluster
## Login into AWS console from the Event Engine dashboard
![Event Engine](../images/eks/credentials.png)

Remeber to only use "us-west-2" as your region
![Event Engine](../images/eks/eks-region.png)

Select EC2 from Management Console and select Instances (running). You should see a EC2 instance called SplunkWorkshop-box

![Event Engine](../images/eks/ec2-splunk.png)

Select SplunkWorkshop-box instance and click connect to open a SSH session
![Event Engine](../images/eks/ec2-splunk.png)

## Verify EKS cluster
```
eksctl get cluster
```

### Update Kubectl
```
aws eks --region us-west-2 update-kubeconfig --name eksworkshop-eksctl 
```
### Test Kubectl
```
kubectl get nodes
```
### Install Helm 3
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3

chmod 700 get_helm.sh
./get_helm.sh

```

### Get Workshop content
``` 
wget https://github.com/splunk/imt-workshop/archive/refs/heads/master.zip
unzip master.zip
```