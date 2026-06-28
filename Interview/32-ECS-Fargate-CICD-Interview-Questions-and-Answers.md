# ECS Fargate + CI/CD Interview Questions & Answers (Scenario Based)
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/f471f7d9-e088-4e7e-b14d-7b19a48b30ec" />

## Overview

This document contains scenario-based interview questions and model answers based on the Ritual Roast Amazon ECS Fargate project.

---

# Scenario 1 – Explain Your Project

**Q:** Explain your end-to-end project.

**Answer:**
I containerized a PHP application using Docker, stored images in Amazon ECR, deployed them on Amazon ECS Fargate behind an Application Load Balancer, used Amazon RDS MySQL for the database, AWS Secrets Manager for credentials, CloudWatch for logs, and Jenkins + GitHub Webhooks for CI/CD. Deployments use rolling updates, with rollback support through previous ECS Task Definition revisions.

---

# Scenario 2 – Why ECS Fargate?

**Q:** Why did you choose ECS Fargate instead of EC2?

**Answer:**
- No server management
- Automatic infrastructure management
- Pay only for CPU and memory used
- Better security isolation
- Faster deployments
- Easy scaling

---

# Scenario 3 – Application Down After Deployment

**Q:** A deployment completed but the application is unavailable. What will you check?

**Answer:**
1. ECS Service events
2. Running Tasks
3. Target Group health
4. ALB listener
5. Security Groups
6. CloudWatch Logs
7. RDS connectivity
8. Secrets Manager values

---

# Scenario 4 – Target Group Unhealthy

**Q:** Why would targets become unhealthy?

**Answer:**
- Wrong container port
- Incorrect health check path
- Application startup failure
- Security Group issue
- Application crashed
- Database connection failure

---

# Scenario 5 – ECS Tasks Keep Restarting

**Answer:**
Check:
- CloudWatch Logs
- Exit code
- CPU/Memory limits
- Environment variables
- Secrets
- Container health

---

# Scenario 6 – Secrets Manager

**Q:** Why use Secrets Manager?

**Answer:**
Never hardcode database credentials. ECS injects secrets securely into containers and credentials can be rotated without rebuilding images.

---

# Scenario 7 – Jenkins Pipeline

**Q:** Explain your CI/CD pipeline.

**Answer:**
Developer pushes code → GitHub Webhook triggers Jenkins → Build Docker image → Push to ECR → Register Task Definition revision → Update ECS Service → Rolling deployment → Verify → Notify.

---

# Scenario 8 – Deployment Failure

**Q:** How do you roll back?

**Answer:**
Update the ECS Service to the previous Task Definition revision or let the automated rollback pipeline redeploy the previous stable revision.

---

# Scenario 9 – Rolling Deployment

**Q:** What happens during rolling deployment?

**Answer:**
New tasks start first. After ALB health checks pass, traffic shifts to new tasks and old tasks are drained.

---

# Scenario 10 – Blue/Green Deployment

**Answer:**
AWS CodeDeploy creates a Green environment, validates it, shifts traffic from Blue to Green, and can automatically roll back if validation fails.

---

# Scenario 11 – ECS Exec

**Q:** How do you access running containers?

**Answer:**
Enable ECS Exec on the ECS Service, configure Task Role permissions, and use AWS Console or `aws ecs execute-command`.

---

# Scenario 12 – Auto Scaling

**Q:** How would you scale the application?

**Answer:**
Use ECS Service Auto Scaling with CloudWatch metrics such as CPU or Memory utilization.

---

# Scenario 13 – High CPU Usage

**Answer:**
Check CloudWatch metrics, container logs, application code, increase task CPU, or scale out.

---

# Scenario 14 – Image Not Updating

**Answer:**
Verify image tag, ECR push, Task Definition revision, and ECS Service update.

---

# Scenario 15 – IAM Roles

**Q:** Difference between Task Role and Task Execution Role?

**Answer:**
Task Execution Role lets ECS pull images and write logs. Task Role is used by the application to access AWS services like Secrets Manager.

---

# Scenario 16 – Security

**Answer:**
Private subnets for ECS/RDS, ALB in public subnets, IAM least privilege, Secrets Manager, Security Groups, CloudWatch and CloudTrail enabled.

---

# Frequently Asked Technical Questions

- Difference between ECS Cluster, Service and Task?
- Why Target Type = IP for Fargate?
- Why ALB instead of NLB?
- Why use ECR?
- Why create a new Task Definition revision every release?
- What is force-new-deployment?
- Difference between Rolling and Blue/Green deployments?
- How does Jenkins know code changed?
- Why use GitHub Webhooks?
- How does ECS register tasks in the Target Group?
- How does ALB perform health checks?
- How do you troubleshoot a failed deployment?
- How do you monitor ECS?
- What happens if RDS is unavailable?
- How do you achieve zero downtime?

---

# Interview Closing Question

**Q:** What did you personally implement?

**Answer:**
I designed the VPC, networking, security groups, Amazon ECR repository, ECS Cluster, Task Definition, ALB, Target Group, RDS integration, Secrets Manager configuration, Jenkins CI/CD pipeline, GitHub Webhooks, automated deployments, rollback process, monitoring, and operational documentation for the project.

---
This guide is intended as a practical interview companion for Amazon ECS Fargate, Docker, Jenkins, GitHub, Amazon ECR, CI/CD, and production deployment scenarios.
