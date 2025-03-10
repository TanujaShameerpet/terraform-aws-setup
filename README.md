# Terraform AWS Infrastructure Setup

## Overview

This Terraform configuration sets up an AWS infrastructure with the following components:

- **VPC** with public and private subnets
- **Internet Gateway** for public subnets
- **NAT Gateway** for private subnet internet access
- **Route Tables** for proper routing
- **Security Groups** for ALB and EC2
- **Application Load Balancer (ALB)** for handling traffic
- **EC2 Instance** configured with Apache web server

## Prerequisites

Ensure you have the following installed on your machine:

- Terraform
- AWS CLI
- An AWS account with appropriate permissions
- SSH key pair configured in AWS for EC2 access

## Configuration

### AWS Provider

The configuration uses the AWS provider and sets the region:

```hcl
provider "aws" {
  region = "us-east-1" # Change to your preferred region
}

Networking
VPC: Creates a VPC with CIDR 10.0.0.0/16
Subnets: Two public and two private subnets across different availability zones
Internet Gateway: Allows public subnet traffic
NAT Gateway: Allows private subnet instances to access the internet
Route Tables: Routes traffic correctly between subnets
Security Groups
ALB Security Group: Allows HTTP (80) and HTTPS (443) traffic
EC2 Security Group: Allows traffic from ALB on port 80 and SSH (22) access within the VPC
Application Load Balancer (ALB)
ALB routes traffic to a target group attached to an EC2 instance
Uses a self-signed SSL certificate for HTTPS traffic
EC2 Instance
Launches a t2.micro EC2 instance in a private subnet
Installs and starts Apache web server
Serves a simple "Hello World" webpage
Deployment
Initialize Terraform
Run the following command to initialize Terraform:

bash
Copy
Edit
terraform init
Plan Changes
To preview changes before applying:

bash
Copy
Edit
terraform plan
Apply Configuration
Deploy the resources to AWS:

bash
Copy
Edit
terraform apply -auto-approve
Retrieve ALB DNS Name
After deployment, get the ALB DNS name to access the web application:

bash
Copy
Edit
terraform output alb_dns_name
Destroy Resources
To remove all resources created by Terraform:

bash
Copy
Edit
terraform destroy -auto-approve
Outputs
ALB DNS Name: Use this to access your web application
EC2 Private IP: Displays the private IP of the web server instance
Notes
Ensure that the SSL certificate ARN is valid in your AWS account
Modify security groups as needed to restrict access
