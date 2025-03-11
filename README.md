 # Terraform AWS Infrastructure Setup

## Overview

This Terraform configuration sets up the website with the following infrastructure:

- **VPC** with public and private subnets
- **Internet Gateway** for public subnets
- **NAT Gateway** for private subnet
- **Route Tables** for routing between subnets
- **Security Groups** for ALB and EC2 instances
- **Application Load Balancer (ALB)** for HTTPS handling traffic
- **EC2 Instance** configured with Apache web server

## Objective 

Application Load Balancer in public subnet must talk to EC2 in Private Subnet to whenever a user requests using https://sample-alb-968498464.us-east-1.elb.amazonaws.com/

## Prerequisites

Ensure you have the following installed on your machine:

- Terraform
- AWS CLI
- An AWS account with appropriate permissions
- AWS Credentials 

## Configuration

### AWS Credentials

Download IAM user or root user credentials and configure your aws credentials in your local

### VPC Setup
The VPC is created with a CIDR block of 10.0.0.0/16 and named as main-vpc.

### Subnets Setup
Two Public Subnets and Two Private subnets are associate with the main-vpc
     - Public Subnet 1: 10.0.1.0/24 in us-east-1a
     - Public Subnet 2: 10.0.2.0/24 in us-east-1b
     - Private Subnet: 10.0.3.0/24 in us-east-1a
     - Private Subnet 2: 10.0.4.0/24 in us-east-1
### Route Tables
- Public Route Table: Routes traffic to the Internet Gateway.
- Private Route Table: Routes traffic to ALB through port 80 and with in the VPC through port 22

### Security Groups
- ALB Security Group: Allows inbound HTTP (80) and HTTPS (443) traffic from all IPs (0.0.0.0/0).
- EC2 Security Group: Allows inbound HTTP (80) traffic from the ALB and SSH (22) traffic from within the VPC (10.0.0.0/16).
  
### Output Image
<img width="1438" alt="image" src="https://github.com/user-attachments/assets/e2cc8a1e-4ca3-4347-9153-491afa937638" />

### Application Load Balancer
An Application Load Balancer is created, using the ALB security group and public subnets. It listens for HTTPS traffic on port 443, and forwards the traffic to EC2 instance in private subnet.

### SSL Certificate
Created self-signed certificate and uploaded it in AWS and 

### EC2 Instance
An EC2 instance is launched in the private subnet associated with EC2 security created as mentioned above, using the t2.micro instance type . Apache web server is installed, and a simple "Hello World" message is served. The EC2 instance is attached to the ALB's target group.

## Outputs
The configuration outputs the following values:

ALB DNS Name: The DNS name of the Application Load Balancer to access the web application.
EC2 Private IP: The private IP address of the EC2 instance.


## Deployment 

``` terraform init ```
Run the above command to initialize the terraform 

``` terraform plan ```
Run the plan command to preview the changes before you deploy the resources

``` terraform apply ```
Run the apply command to deploy the infrastructure

For other terraform commands such ```terraform destroy``` etc refer to https://developer.hashicorp.com/terraform/cli/commands








