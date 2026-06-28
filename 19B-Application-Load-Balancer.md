# Step 11 - Create Application Load Balancer
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/d4ca5cb6-c46d-4574-88b4-38e237ee3257" />

## Objective

Expose the ECS application to users.

## Steps

1. Create an Application Load Balancer.
2. Select Internet-facing.
3. Select Public Subnets.
4. Attach the ALB Security Group.
5. Create a Target Group.
6. Target Type = IP.
7. Configure Health Check.
8. Create Listener on HTTP Port 80.

## Result

The Load Balancer will distribute traffic to ECS Tasks.
