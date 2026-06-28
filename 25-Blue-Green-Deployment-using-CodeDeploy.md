# 25 – Blue/Green Deployment using AWS CodeDeploy (Zero-Downtime Deployment)
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/117ed4b9-524b-49d4-a740-6064fa1dc2d2" />

> Deploy new versions of the Ritual Roast application with **zero downtime** by using Amazon ECS Blue/Green deployments integrated with AWS CodeDeploy.

---

# Objective

Blue/Green deployment creates a new version of your ECS tasks (Green) alongside the existing version (Blue). After validation, traffic is gradually or instantly shifted using the Application Load Balancer.

---

# Architecture

```text
Users
   │
Application Load Balancer
   │
Production Listener (80)
   │
 ┌───────────────┬────────────────┐
 │               │
Blue Target   Green Target
Group         Group
(Current)     (New)
 │               │
ECS Tasks     ECS Tasks
v1.0.0        v1.0.1
```

---

# AWS Services Used

- Amazon ECS
- AWS CodeDeploy
- Amazon ECR
- Application Load Balancer
- Two Target Groups
- CloudWatch
- IAM

---

# Prerequisites

- ECS Cluster: `ritual-roast-cluster`
- ECS Service configured for CodeDeploy
- ALB with two Target Groups
- Docker image available in Amazon ECR
- AppSpec file
- Task Definition
- CodeDeploy Application & Deployment Group

---

# Deployment Flow

1. Developer commits code.
2. Build Docker image.
3. Push image to Amazon ECR.
4. Register a new Task Definition revision.
5. Update the AppSpec file with the new Task Definition ARN.
6. Create a CodeDeploy deployment.
7. CodeDeploy launches Green tasks.
8. ALB performs health checks.
9. Traffic shifts from Blue to Green.
10. Blue tasks terminate after the configured wait time.

---

# Create CodeDeploy Application

AWS Console

```text
CodeDeploy
 └── Applications
      └── Create Application
```

Configuration:

| Setting | Value |
|---|---|
| Compute Platform | Amazon ECS |
| Application Name | ritual-roast-codedeploy |

---

# Create Deployment Group

Configure:

- ECS Cluster
- ECS Service
- Production Listener
- Test Listener (optional)
- Blue Target Group
- Green Target Group
- Deployment Configuration
- Automatic Rollback

---

# Deployment Configurations

Recommended:

- CodeDeployDefault.ECSAllAtOnce
- CodeDeployDefault.ECSLinear10PercentEvery1Minutes
- CodeDeployDefault.ECSCanary10Percent5Minutes

---

# AppSpec Example

```yaml
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: <TASK_DEFINITION_ARN>
        LoadBalancerInfo:
          ContainerName: ritual-roast-app
          ContainerPort: 80
```

---

# Verification

- Green Target Group becomes Healthy.
- Traffic shifts successfully.
- ECS deployment completes.
- Application is accessible.
- CloudWatch Logs show no errors.

---

# Automatic Rollback

Enable rollback on:

- Failed deployment
- Failed health checks
- CloudWatch alarms

CodeDeploy automatically restores traffic to the Blue environment.

---

# Best Practices

- Use immutable Docker image tags.
- Always keep the previous task definition revision.
- Enable CloudWatch alarms.
- Test in Green before traffic shifting.
- Enable automatic rollback.

---

# High-Level Flow

```text
Developer
   │
Git
   │
Docker Build
   │
Amazon ECR
   │
Task Definition Revision
   │
AWS CodeDeploy
   │
Green Environment
   │
Health Checks
   │
Traffic Shift
   │
Production
```

---

# Key Takeaway

Blue/Green deployment with AWS CodeDeploy provides zero-downtime releases, automated traffic shifting, health validation, and fast rollback for Amazon ECS Fargate workloads.
