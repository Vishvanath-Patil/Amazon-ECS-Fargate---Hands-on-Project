# Troubleshooting
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/dff9c708-be65-4430-a5de-8a286425599c" />

## ECS Task Stops Immediately

Possible checks:
- Container image exists in Amazon ECR.
- Task Definition uses the correct image.
- IAM Task Execution Role is attached.

## Application Not Accessible

Check:
- ECS Service status
- Target Group health
- Application Load Balancer DNS
- Security Groups

## Database Connection Issues

Verify:
- RDS status is Available.
- Database Security Group allows MySQL (3306) from the application security group.
- Secrets Manager contains the correct credentials.

## Image Pull Failure

Verify:
- Image pushed successfully to Amazon ECR.
- Task Definition references the correct repository URI.
