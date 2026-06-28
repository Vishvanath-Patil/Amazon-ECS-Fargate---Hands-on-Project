# Project Steps

## Step 1
Create a VPC with:
- 2 Availability Zones
- Public Subnets
- Private App Subnets
- Private Database Subnets
- Internet Gateway
- NAT Gateway

## Step 2
Create Security Groups.

## Step 3
Create Amazon RDS MySQL.

## Step 4
Store database credentials in AWS Secrets Manager.

## Step 5
Build the Docker image.

## Step 6
Create an Amazon ECR repository.

## Step 7
Push the Docker image to Amazon ECR.

## Step 8
Create an ECS Cluster using Fargate.

## Step 9
Create an IAM Task Execution Role.

## Step 10
Create an ECS Task Definition.

## Step 11
Create an Application Load Balancer.

## Step 12
Create an ECS Service.

## Step 13
Open the ALB DNS name and verify the application.

## Step 14
Submit sample data and verify it is stored in the MySQL database.

## Cleanup

Delete:
- ECS Service
- ECS Cluster
- Load Balancer
- Target Group
- ECR Repository
- Secrets
- RDS Database
- VPC
