# EC2 (Elastic Cloud Computing)

* Basically computer with operating system and pre-installed softwares

![./ec2.png](./ec2.png)


# EBS (Elastic Block Storage)

* Storage of the EC2 file system

![./ebs.png](./ebs.png)



# S3 (Simple Storage Service)

* Triggers events when objects are added/modified/deleted
* Keeps older versions of objects
* S3 buckets can be accessed throug URL
* Enables the hosting of static web sites
  
![./s3.png](./s3.png)



# CloudFront
* Basically it is a CDN
* Works perfectly with S3, EC2, Route53, LoadBalancer
  
![./cloudfront.png](./cloudfront.png)



# RDS (Relational Database Service)

![./rds.png](./rds.png)



# Route53
* DNS service

![./route53.png](./route53.png)


# EB (Elastic Beanstalk)
* AWS Elastic Beanstalk is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker on familiar servers such as Apache, Nginx, Passenger, and IIS.
* You can simply upload your code and Elastic Beanstalk automatically handles the deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring.
* App versions are stored in S3
* Free, just pay for EC2 instances, load balancers and S3 buckets

![./elasticbeanstalk.png](./elasticbeanstalk.png)
![./eb-structure.png](./eb-structure.png)



# Lambda
* Function code execution as a service
* FaaS, Serverless
* Executes code
* No server management required
* Only pay when your code is running
* Structure
  * Code
  * Platform type (Node, Java, Go etc.)
  * Triggers (API gateway, cloudwatch, cloudfront etc.)
  * Configuration

![./Lambda.png](./Lambda.png)


# DynamoDb
* Managed NoSQL db server
* No hardware choices
* Pay only for storage
* First 25GB free

![./dynamodb.png](./dynamodb.png)


# VPC (Virtual Private Cloud)
* Secure and control access
* VPCs secure groups of instances
* Configure VPC routing tables
* Use NAT Gateway for outbound traffic
* Internal IP address allocation
* Basic VPC config is free

![./vpc.png](./vpc.png)
![./vpc-structure.png](./vpc-structure.png)


# CloudWatch
* Monitoring service
* Monitoring resources
* Acting on alerts
* Monitor and aggregate the logs

![./cloudwatch.png](./cloudwatch.png)

