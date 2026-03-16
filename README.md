## AWS 3-Tier Platform (Terraform + CI/CD)
## Overview

This project provisions a production-style highly available AWS 3-tier web platform using Terraform and deploys infrastructure through a fully automated GitHub Actions CI/CD pipeline. The platform was deployed, validated, tested for self-healing behaviour, and safely destroyed to demonstrate cost-aware infrastructure lifecycle management.


The platform demonstrates real-world DevOps practices including:

  -Infrastructure as Code (Terraform)
  
  -Remote state management (S3 + DynamoDB locking)
  
  -Load balancing and Auto Scaling
  
  -Self-healing architecture
  
  -Monitoring and scaling policies
  
  -CI validation pipeline
  
  -Protected deployment pipeline with manual approval

## Architecture

The platform deploys:

  -Custom VPC across multiple Availability Zones
  
  -Public subnets for Load Balancer and NAT Gateway
  
  -Private subnets for application instances
  
  -Internet Gateway for public access
  
  -NAT Gateway for outbound internet from private tier
  
  -Application Load Balancer (ALB)
  
  -Launch Template
  
  -Auto Scaling Group
  
  -CloudWatch CPU alarm
  
  -Target tracking scaling policy
  
  -Nginx web application bootstrap via EC2 user-data

Traffic flow:

Internet → ALB (public subnets) → EC2 instances (private subnets)

## Key Features

## High Availability

  -Multi-AZ deployment
  
  -ALB health checks
  
  -Auto Scaling Group ensures instance replacement
  
  -Verified self-healing behaviour

## Secure Network Design

  -Application servers isolated in private subnets
  
  -Controlled inbound access via Security Groups
  
  -Outbound internet access through NAT Gateway only

## Observability & Scaling

  -CloudWatch alarm triggers scale-out
  
  -Target tracking policy maintains performance under load

## Remote State Management

  -Terraform state stored in S3
  
  -DynamoDB state locking prevents concurrent corruption
  
  -Enables team-safe infrastructure deployments

## CI/CD Pipeline (GitHub Actions)

The repository includes a two-stage deployment pipeline:

## Stage 1 — Terraform CI

Runs automatically on push:

  -terraform init
  
  -terraform fmt
  
  -terraform validate
  
  -terraform plan
  
  -uploads execution plan as artifact

This ensures infrastructure code quality and visibility before deployment.

## Stage 2 — Protected Deploy

  -Requires manual approval
  
  -Uses saved Terraform plan artifact
  
  -Executes:
    terraform apply

This models real production release controls.

## Project Structure

aws-3tier-platform/
│

├── main.tf

├── variables.tf

├── outputs.tf

├── provider.tf

├── terraform.tfvars

│

├── backend-setup/

│   ├── s3.tf

│   └── dynamodb.tf

│

├── .github/workflows/

│   └── terraform.yml

│

└── README.md

## Deployment

## Prerequisites

  -AWS Account
  
  -Terraform installed
  
  -IAM user with programmatic access
  
  -GitHub repository secrets configured:
    AWS_ACCESS_KEY_ID
    AWS_SECRET_ACCESS_KEY
    AWS_REGION

## Local Deployment
    terraform init
    terraform plan
    terraform apply

## CI Deployment

Push to main branch:
    git push origin main

Approve deployment in GitHub Actions UI.

## Validation

After deployment:

  -Access ALB DNS output
  
  -Confirm web page loads
  
  -Terminate an EC2 instance to observe Auto Scaling self-healing

## Cleanup

To avoid unnecessary AWS charges, destroy the infrastructure after testing.

Destroy main infrastructure

  terraform destroy

Destroy backend resources

   cd backend-setup
   
   terraform destroy

## Important notes

Destroy the main infrastructure before deleting the remote state backend.

If the S3 backend bucket has versioning enabled, empty all object versions and delete markers before destroying the bucket.

Always confirm that EC2 instances, Load Balancers, Auto Scaling Groups, NAT Gateways, and target groups have been removed.


---


## Learning Outcomes

This project demonstrates practical skills in:

  -AWS networking design
  
  -Terraform infrastructure lifecycle
  
  -DevOps CI/CD pipeline engineering
  
  -Cloud resilience and scaling patterns
  
  -Production deployment governance




## Author
Iskandar Nuhu

Cloud & DevOps Engineer
