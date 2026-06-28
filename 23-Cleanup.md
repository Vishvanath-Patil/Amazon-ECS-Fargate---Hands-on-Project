# Cleanup Resources
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/7d1f8c46-ffeb-4efd-985e-766ff2c01843" />

After completing the tutorial, remove resources to avoid unnecessary AWS charges.

Delete resources in this order:

1. ECS Service
2. Running Tasks
3. ECS Cluster
4. Application Load Balancer
5. Target Group
6. Amazon ECR Repository
7. AWS Secrets Manager Secret
8. Amazon RDS Database
9. NAT Gateway
10. Internet Gateway
11. VPC

## Verification

Confirm that no ECS, RDS, ECR, or networking resources remain for this project.
