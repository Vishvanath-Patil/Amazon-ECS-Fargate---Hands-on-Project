# Step 12 - Create ECS Service

## Objective

Deploy the application.

## Configuration

- ECS Cluster
- Task Definition
- Launch Type = AWS Fargate
- Desired Tasks
- Private Application Subnets
- Application Security Group
- Attach Application Load Balancer
- Target Group

## Deployment

Create the ECS Service.

## Verification

- Task Status = Running
- Target Group = Healthy
- Open the ALB DNS Name in a browser.
- Verify the Coffee Shop application loads successfully.
