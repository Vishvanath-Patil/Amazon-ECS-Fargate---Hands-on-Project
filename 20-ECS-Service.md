# Step 12 - Create ECS Service
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/9789d0ff-d81b-4e9e-a63d-72cf1a7d4b21" />

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
