## Self managed Node Group creation

* Go to S3 and create bucket (folder/directory) with *Create bucket*.
* Bucket name to be unique like *multus-cluster* (recommend to use your name or unique keyword), and then *Create bucket*.
* Click the bucket you just created and drag & drop lambda_function.zip file . Then, click *Upload*.
* Please memorize bucket name you create (this is required in CloudFormation)
* Go to CloudFormation console by selecting CloudFormation from Services drop down or by search menu. 
    * Select *Create stack*, *with new resources(standard)*.
    * Click *Template is ready" (default), "Upload a template file", "Choose file". Select "eks-nodegroup-multus.yaml" file that you have downloaded from this GitHub. 
    * Stack name -> eks-nodegroup
    * ClusterName -> eks-airgapped-cluster (your own cluster name)
    * ClusterControlPlaneSecurityGroup -> "eks-cluster-sg-eks-private-demo-cluster-xxxx"
    * NodeGroupName -> ng1
    * AutoScalingGroup Min/Desired/MaxSize -> 1/2/3
    * NodeInstanceType -> select EC2 flavor, based on the requirement (or choose default)
    * NodeImageIdSSMParam --> EKS optimized linux2 AMI release (default 1.21, change the release value, if needed)
    * NodeImageId --> (if using custome AMI then use AMIID, this option will override NodeImageIdSSMParam)
    * NodeVolumeSize --> configure Root Volume size (default 50 gb)
    * KeyName -> ee-default-keypair (or any ssh key you have)
    * BootstrapArguments -> configure your k8 node labels, (leave default if not sure)
    * useIPsFromStartOfSubnet -> use true (to use option 1 mentioned above) or false (to use option 2 i.e. cidr reservation)
    * VpcId -> vpc-eks-multus-cluster (that you created)
    * Subnets -> privateAz1-eks-multus-cluster (this is for main primary K8s networking network)
    * MultusSubnets -> multus1Az1 and Multus2Az1 (subnets are attached in same order as provided, so multus1Az1 as eth1 and Multus2Az1 as eth2 )
    * MultusSecurityGroups -> multus-Sg-eks-multus-cluster
    * LambdaS3Bucket -> the one you created (eks-multus-cluster)
    * LambdaS3Key -> lambda_function.zip
    * InterfaceTags --> (optional , leave it blank or put a key-value pair as Tags on the i/f)
    * *Next*, check "I acknowledge...", and then *Next*.

* Once CloudFormation stack creation is completed, check *Output* part in the menu and copy the value of NodeInstanceRole (e.g. arn:aws:iam::XXXXXXXXXXXXX:role/ng1-NodeInstanceRole-1C77OUUUP6686 --> this is an example, you have to use your own)


## Apply AWS Auth Configmap
* Go to the bastion host/cloud9 in the CICD account where we can run kubectl command. 
* Download aws-auth-cm file.
  ````
  curl -o aws-auth-cm.yaml https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/aws-auth-cm.yaml
  ````

  ````
  kubectl apply -f aws-auth-cm.yaml
  ````