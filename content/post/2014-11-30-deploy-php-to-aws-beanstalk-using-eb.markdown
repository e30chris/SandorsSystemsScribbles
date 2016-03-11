---
categories: aws
comments: false
date: 2014-11-30T00:00:00Z
summary: The Problem - Wordpress has lots of hosting options all of which cost money
  to be on a shared hosting server.  I need a autoscaling & auto-healing Wordpress
  setup that will cost less than dedicated WP hosting while offering all the awesomeness
  of AWS.  AWS is hard to setup all the moving pieces correctly, Beanstalk configures
  all those pieces while costing nothing other than paying for the AWS resources Beanstalk
  sets up.
title: Deploy Wordpress to AWS Beanstalk using eb
url: /2014/11/30/deploy-php-to-aws-beanstalk-using-eb/
---

# The Problem
Wordpress has lots of hosting options all of which cost money to be on a shared hosting server.  I need a autoscaling & auto-healing Wordpress setup that will cost less than dedicated WP hosting while offering all the awesomeness of AWS.  AWS is hard to setup all the moving pieces correctly, Beanstalk configures all those pieces while costing nothing other than paying for the AWS resources Beanstalk sets up.

# The Goal

Deploy an autoscaling, auto-healing Wordpress website on AWS Beanstalk using the 'eb' cli tool.  


# The Links

  - AWS Beanstalk - [link](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html)
  - eb - [link](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-reference-get-started.html)
  - AWS Regions & Endpoints - [link](http://docs.aws.amazon.com/general/latest/gr/rande.html)
  - Wordpress - [link](https://wordpress.org)
  - Wordpress on GitHub [link](https://github.com/WordPress/WordPress)
  
# HowTos

## HowTo - Setup the eb tool

On your MacBook create a file to host your AWS creds.

~~~
sandor@pineApplez$ vim ~/.awscreds
~~~

~~~
AWSAccessKeyId=Your AWS access ID
AWSSecretKey=Your AWS secret key
~~~

Set the permissions

~~~
sandor@pineApplez$ chmod 0600 ~/.awscreds
~~~

Install *all* the AWS tools using Homebrew

~~~
brew install awscli
brew install aws-elasticbeanstalk
brew install aws-as
brew install aws-cfn-tools
brew install aws-cloudsearch
brew install aws-elasticcache
brew install aws-mon
brew install aws-sns-cli
~~~
Some of those installs need stuff added to the bash|zsh profile

~~~
# aws stuff
export AWS_ACCESS_KEY="Your AWS access ID"
export AWS_SECRET_KEY="Your AWS secret key"
export AWS_CLOUDFORMATION_HOME="/usr/local/Cellar/aws-cfn-tools/1.0.12/libexec"
export AWS_CLOUDWATCH_HOME="/usr/local/Cellar/cloud-watch/1.0.20.0/libexec"
export AWS_ELASTICACHE_HOME="/usr/local/Cellar/aws-elasticache/1.9.000/libexec"
export CS_HOME=/usr/local/Cellar/aws-cloudsearch/1.0.1.4-2013.01.11
export EC2_HOME="/usr/local/Cellar/ec2-api-tools/1.7.1.0/libexec"
export AWS_SNS_HOME="/usr/local/Cellar/aws-sns-cli/2013-09-27/libexec"
export JAVA_HOME="$(/usr/libexec/java_home)"
export AWS_CREDENTIAL_FILE=/Users/sandor/.awscreds
export ELASTICBEANSTALK_URL="https://elasticbeanstalk.us-west-2.amazonaws.com"
~~~
*Planning ahead for the eventual [Cascadia](https://en.wikipedia.org/wiki/Cascadia_%28independence_movement%29) so Oregon is my AWS home*

## HowTo - Setup a BeanStalk Environment

Setup a basic PHP BeanStalk to verify everything is working.  The end results is you should see a PHP test page.

~~~
sandor@pineApplez$ eb init
sandor@pineApplez$ To get your AWS Access Key ID and Secret Access Key,
  visit "https://aws-portal.amazon.com/gp/aws/securityCredentials".
Enter your AWS Access Key ID: Your AWS Access Key
Enter your AWS Secret Access Key: Your AWS Secret Access Key
Select an AWS Elastic Beanstalk service region.
Available service regions are:
…
2) US West (Oregon)
...
Select (1 to 9): 2
Enter an AWS Elastic Beanstalk application name (auto-generated value is "testapp"):
Enter an AWS Elastic Beanstalk environment name (auto-generated value is "testapp-env"):
Select an environment tier.
Available environment tiers are:
1) WebServer::Standard::1.0
...
Select (1 to 2): 1
Select a solution stack.
Available solution stacks are:
1) 64bit Amazon Linux 2014.09 v1.0.9 running PHP 5.5
…
Select (1 to 49): 1
Select an environment type.
Available environment types are:
...
2) SingleInstance
Select (1 to 2): 2
Create an RDS DB Instance? [y/n]: y
Create an RDS BD Instance from (current value is "[No snapshot]"):
1) [No snapshot]
...
Select (1 to 2): 1
Enter an RDS DB master password:
Retype password to confirm:
If you terminate your environment, your RDS DB Instance will be deleted and you will lose your data.
Create snapshot? [y/n]: n
Attach an instance profile (current value is "[Create a default instance profile]"):
You IAM user does not have sufficient permission. User: arn:aws:iam::246173154382:user/smbec2r is not authorized to perform: iam:ListInstanceProfiles on resource: arn:aws:iam::246173154382:instance-profile/
Do you want to proceed without attaching an instance profile? [y/n]: y
Updated AWS Credential file at "/Users/sandor/.elasticbeanstalk/aws_credential_file".

~~~

Now start the test BeanStalk

~~~
sandor@pineApplez$ eb start
Starting application "testapp".
Waiting for environment "testapp-env" to launch.
2014-12-01 00:01:10	INFO	createEnvironment is starting.
2014-12-01 00:01:12	INFO	Using elasticbeanstalk-us-west-2-246133154382 as Amazon S3 storage bucket for environment data.
2014-12-01 00:01:47	INFO	Created EIP: 54.149.45.218
2014-12-01 00:01:50	INFO	Created security group named: awseb-e-97v6vazrp7-stack-AWSEBSecurityGroup-1AECQMWSW2P6H
2014-12-01 00:02:00	INFO	Creating RDS database named: ab1e1p73zy1ede2. This may take a few minutes.
2014-12-01 00:11:27	INFO	Created RDS database named: aa1e1p73zv1ede2
2014-12-01 00:12:23	INFO	Waiting for EC2 instances to launch. This may take a few minutes.
2014-12-01 00:14:05	INFO	Successfully launched environment: testapp-env
~~~

Navigate to the EIP - http://54.149.45.218 and you should see the Beanstalk PHP congrats page.

![AWS Congrats Page Screenshot](https://dl.dropboxusercontent.com/u/6735750/WebHostPics/AWSCongratsScreen.png)

Now that we know everything is setup correctly to deploy a test PHP environment we will deploy a full on Wordpress install.

## HowTo - Deploy Wordpress to Beanstalk

First goal is to get a bare bones Wordpress setup working.  There are two ways to deploy to Beanstalk, hand it a docker container of your app or do a git push.  For this example we are going with a Docker container because in 2 years that will be the way it is and I dont want to go back and read how retarded I was in 2014 deploying with git.

This HowTo assumes we are starting with a clean whiteboard Wordpress deployment.  The HowTo of migrating an existing Wordpress will be a separate post.

Clone Wordpress from the Github mirror of the WP SVN, into the same directory we used for the eb steps above. 

~~~
sandor@pineApplez$ git clone git@github.com:WordPress/WordPress.git .
~~~




















