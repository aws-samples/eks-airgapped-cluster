# Introduction

This repository outlines the key steps involved in establishing a secure and isolated EKS private cluster within your AWS environment.It offers step-by-step instructions, sample CloudFormation templates for VPC private endpoints, and an example installation for EKS Addons in air-gapped environments, allowing you to manage containerized applications within a strictly controlled and secure network.

## Table of Contents
- [Introduction](#introduction)
  - [Table of Contents](#table-of-contents)
  - [About this Repo ](#about-this-repo-)
  - [Deploy Cloudformation to create VPC Endpoints](#deploy-cloudformation-to-create-vpc-endpoints)
  - [Creating an EKS Private Cluster with eksctl](#creating-an-eks-private-cluster-with-eksctl)
  - [Creating an EKS Private Cluster Nodegroup ](#creating-an-eks-private-cluster-nodegroup-)
  - [Bootstrap EKS Private Cluster with Multus Addon](#bootstrap-eks-private-cluster-with-multus-addon)
  - [Security](#security)
  - [License](#license)

## About this Repo <a name="About"></a>

This repository provides comprehensive resources for setting up a secure and isolated EKS (Amazon Elastic Kubernetes Service) Private Cluster within your AWS environment. It includes:
 Detailed instructions: Learn how to create a private EKS cluster step-by-step, ensuring your Kubernetes control plane remains inaccessible from the public internet.
    CloudFormation templates: These pre-configured templates simplify the deployment of VPC (Virtual Private Cloud) Private Endpoints for various AWS services, facilitating secure communication within your private network.
    Air-gapped environment support: Discover an example installation process for EKS Addons, like Multus, in an air-gapped environment, where external connectivity is strictly limited.

By leveraging these resources, you can effectively configure a robust and secure EKS private cluster, allowing you to deploy and manage containerized applications within a strictly controlled and isolated environment.


## Deploy Cloudformation to create VPC Endpoints<a name="VPC Endpoints"></a>
1. Prerequisites:

    An AWS account with sufficient permissions to create CloudFormation stacks and VPC resources. An existing VPC with IPV4 Private Subnets associated with it.
    

2. Deploy the CloudFormation Stack:

    Access the AWS Management Console  and navigate to the CloudFormation service. Upload the stack.

## Creating an EKS Private Cluster with eksctl<a name="eksctl"></a>
   
   Follow the steps in [EKS Cluster Creation](./eksctl/README.md)


## Creating an EKS Private Cluster Nodegroup <a name="Nodegroup"></a>

## Bootstrap EKS Private Cluster with Multus Addon<a name="bootstrap"></a>
   Follow the steps in [EKS Add ons](./private-images/README.md)
    









## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

