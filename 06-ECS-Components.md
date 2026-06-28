# ECS Components
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/24e07cf6-755f-4ada-9124-dcf2d8627236" />

## Amazon ECR
Stores Docker images.

## ECS Cluster
Logical grouping of ECS resources where tasks and services run.

## Task Definition
Blueprint describing:
- Container image
- CPU
- Memory
- Port mappings
- IAM Role
- Environment variables
- Secrets
- Volumes

## Task
A running instance of a task definition.

## ECS Service
Maintains the desired number of running tasks and replaces failed tasks.
