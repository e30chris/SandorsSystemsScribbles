---
title = "Deploy AWS Redshift into a dedicated VPC on the EC2-VPC platform"
date = "2015-05-01"
tags = [ "aws", "redshift", "vpc" ]
categories = [ "aws" ]
image = ""
---

## The Problem:
The Problem - By default new AWS accounts are based on the EC2-VPC platform.  If you want to launch a Redshift cluster into a specific VPC that is not the default you must first create a Redshift Cluster Subnet.

## The solution:
Create a Redshift Cluster Subnet that will be joined to a existing VPC (not the default VPC) which will then allow the new Redshift cluster to be launched into that dedicated VPC.

### Backstory:
As the SRE team we manage each teams AWS environment.  To keep it simple all teams are in their own VPC with the default VPC being left unused.  Launching a Redshift cluster for the Data Analytics team and trying to put that Redshift cluster into the Data Analytics VPC presented the problem of not being able to select anything other than the default VPC.  Reading the AWS documentation does not address launching Redshift into a non-default VPC.  This Sandors notebook note does.

---

### Pre-Requisites:
Create a VPC with subnets for the Redshift cluster to be deployed.

## CLI

Create the Cluster Subnet Group

_subnet-ids is the VPC Subnet you want the Redshift cluster launched into. This is an existing VPC (not the default)._

~~~
aws redshift create-cluster-subnet-group

  --cluster-subnet-group-name argodataanalyticsredshiftsubnet

  --description "Argo Data Analytics Redshift Subnet"

  --subnet-ids subnet-382fee1w

~~~

[AWS CLI Documentation create=cluster-subnet-group](http://docs.aws.amazon.com/cli/latest/reference/redshift/create-cluster-subnet-group.html)

## AWS Console

 - Open RDS Dashboard
 - Security
 - Subnet Groups
   - Click _Create Cluster Subnet Group_
   - Give the Subnet a name & description
   - Select the VPC you want to launch Redshift into
   - Select the Availability Zone you want the subnet in
   - Select the subnet for that AZ

[AWS Console Documentation](http://docs.aws.amazon.com/redshift/latest/mgmt/managing-cluster-subnet-group-console.html)
