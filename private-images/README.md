# Create ECR private repository and push the images
## Step 1:
In EKS Private Cluster we want to deploy  a sample add-ons like Multus using kubernates manifests. We will use this github https://github.com/aws/amazon-vpc-cni-k8s/blob/master/config/multus/README.md
Pull the public ECR image and push it to your Private ECR, refer to the private ECR image in your manifests file and apply them on your cluster.

Create  a ECR  private repository in your AWS account. 

```bash linenums="1"
aws ecr create-repository \
    --repository-name cicd \
    --region region
```
## Step 2:
Login to the public ECR repo and pull the image multus-cni:v4.0.2-eksbuild.1_thick

```bash linenums="1"
aws ecr get-login-password —region aws_region| docker login —username AWS —password-stdin 602401143452.dkr.ecr.eu-central-1.amazonaws.com
docker pull 602401143452.dkr.ecr.aws_region.amazonaws.com/eks/multus-cni:v4.0.2-eksbuild.1_thick
docker tag 602401143452.dkr.ecr.aws_region.amazonaws.com/eks/multus-cni:v4.0.2-eksbuild.1_thick aws_account_id.dkr.ecr.eu-central-1.amazonaws.com/cicd/multus-cni:latest
```
## Step 3:

Now, login to your Private ECR repository and push the local docker image, which you have pulled in the above step.

```bash linenums="1"
aws ecr get-login-password --region aws_region| docker login --username AWS --password-stdin aws_account.dkr.ecr.eu-central-1.amazonaws.com
docker push aws_account.dkr.ecr.aws_region.amazonaws.com/cicd/multus-cni:latest
```

Once you have sccessfully pushed the image to your local ECR private repository, replace the image URI in the kubernates mafifests file. 
A sample manifests file is  [Multus](././multus-daemonset-thick.yaml)


Finally, apply it to the cluster

```bash linenums="1"
kubectl apply -f multus-daemonset-thick.yml
apiextensions.k8s.io/network-attachment-definitions.k8s.cni.cncf.io created
clusterrole.rbac.authorization.k8s.io/multus createdclusterrolebinding.rbac.authorization.k8s.io/multus created
serviceaccount/multus created
configmap/multus-daemon-config created
daemonset.apps/kube-multus-ds created
```
Check the pods in kube-system

```bash linenums="1"
kubectl get pods -o wide -n kube-system
NAME                       READY   STATUS    RESTARTS   AGE   IP           NODE                                          NOMINATED NODE   READINESS GATESaws-node-vnb2f             2/2     Running   0          31d   10.4.1.122   ip-10-4-1-122.eu-central-1.compute.internal   <none>           <none>coredns-6566899dc6-l9wg7   1/1     Running   0          38d   10.4.1.96    ip-10-4-1-122.eu-central-1.compute.internal   <none>           <none>coredns-6566899dc6-psr2p   1/1     Running   0          38d   10.4.1.45    ip-10-4-1-122.eu-central-1.compute.internal   <none>           <none>kube-multus-ds-6pfgn       1/1     Running   0          16s   10.4.1.122   ip-10-4-1-122.eu-central-1.compute.internal   <none>           <none>
```  

You can also verify the multus version in the pod
```bash linenums="1"

kubectl -n kube-system exec -it kube-multus-ds-6pfgn  -c kube-multus -- /usr/src/multus-cni/bin/multus --version                                                                                                                                     
multus: version:v4.0.2-eksbuild.1(clean,released), commit:f03765681fe81ee1e0633ee1734bf48ab3bccf2b, date:2023-11-15T18:36:48+00:00

``` 
