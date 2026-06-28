# ECS Components

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
