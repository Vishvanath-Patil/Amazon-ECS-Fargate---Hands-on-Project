# Step 9 - Create IAM Task Execution Role

## Objective

Allow ECS tasks to pull container images from Amazon ECR and retrieve secrets from AWS Secrets Manager.

## Required Permissions

- Amazon ECR
- Amazon CloudWatch Logs
- AWS Secrets Manager

## Steps

1. Open IAM.
2. Create a new Role.
3. Select **ECS Task** as the trusted entity.
4. Attach the required permissions.
5. Save the role.

## Result

The role will be selected while creating the ECS Task Definition.
