# VPC Private Endpoint

This product creates VPC Private EndPoints for EKS Private Cluster.
There are two types of VPC endpoints:

- interface endpoints

- gateway endpoints

 This Service Catalog products creates Interface Endpoints for the following services:

  - Amazon Simple Storage Service (Amazon S3)
  - AWS Systems Manager
  - Amazon ECR
  - Amazon Elastic File System
  - AWS Elastic Block Storage
  - Amazon EKS
  - AWS Security Token Service
  - AWS S3
  - AWS Lambda
  - AWS Events
  - AWS ECR Docker
  - AWS Sync States
  - AWS States
  - AWS Cloudformation
  - AWS Lambda

 Gateway Endpoints for the following services:
  
   - Amazon DynamoDB
   - Amazon Simple Storage Service (Amazon S3)
    


## Prerequisite Solutions

- VPC with Private Subnets exists in the AWS account


## Deploy the Solution 

1. Deploy the Cloudformation Template in the target AWS Account with the required input parameters i.e VPC id and Subnet CIDR




