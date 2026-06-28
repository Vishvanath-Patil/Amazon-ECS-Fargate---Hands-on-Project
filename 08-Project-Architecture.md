# Project Architecture
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/90bfc527-86c9-4625-80fa-3512601953d9" />

Resources created in the tutorial:

- VPC
- 2 Availability Zones
- Public Subnets
- Private Application Subnets
- Private Database Subnets
- Internet Gateway
- NAT Gateway
- Application Load Balancer
- Amazon ECS Cluster
- Amazon RDS MySQL
- AWS Secrets Manager
- Amazon ECR
- ECS Task Definition
- ECS Service

Application Flow

User
 ↓
Application Load Balancer
 ↓
ECS Service
 ↓
Fargate Tasks
 ↓
Application
 ↓
Amazon RDS MySQL
