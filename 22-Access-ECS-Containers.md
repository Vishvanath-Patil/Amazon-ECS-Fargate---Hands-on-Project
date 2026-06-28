# Access ECS Containers

## Developers
- Use ECS Exec
- View CloudWatch Logs

## Cloud & DevOps Administrator
- AWS Console access to ECS, ECR, EC2, RDS, IAM, Secrets Manager, CloudWatch
- AWS CLI access
- ECS Exec

### Required IAM

Developer:
- ecs:ExecuteCommand
- ecs:DescribeTasks
- logs:GetLogEvents

Administrator:
- AmazonECS_FullAccess
- AmazonEC2ContainerRegistryPowerUser
- CloudWatchLogsFullAccess
- SecretsManagerReadWrite
- AmazonRDSReadOnlyAccess
- IAMReadOnlyAccess

### Access Flow

Developer/Admin -> AWS Console or CLI -> ECS Cluster -> ECS Service -> Running Task -> ECS Exec -> Ritual Roast Container -> Amazon RDS
