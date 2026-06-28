# ECS Launch Types
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/305a3f58-5a0b-4cf9-ba9c-e67dfb660c96" />

## EC2 Launch Type

Characteristics:
- You manage EC2 instances.
- Responsible for patching and updates.
- Supports Reserved and Spot Instances.

Workflow:
Container Image -> Task Definition -> ECS Cluster -> EC2 Instances -> Containers

## AWS Fargate Launch Type

Characteristics:
- Serverless container platform.
- AWS manages infrastructure.
- Pay for CPU and Memory used.
- No EC2 management.

Workflow:
Container Image -> Task Definition -> ECS Service -> Fargate -> Containers

The tutorial uses AWS Fargate.
