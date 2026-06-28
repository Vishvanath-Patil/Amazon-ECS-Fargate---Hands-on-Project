# 31 – End-to-End CI/CD Pipeline Execution
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/a71bc20d-dc15-48e1-948b-2f4f260064aa" />

> Complete end-to-end execution flow for deploying the Ritual Roast application to Amazon ECS Fargate using GitHub, Jenkins, Docker, Amazon ECR and Amazon ECS.

---

# Objective

Understand the complete CI/CD lifecycle from a developer committing code to a successful production deployment with automated verification and rollback support.

---

# End-to-End Architecture

```text
Developer
    │
Git Commit & Push
    │
GitHub Repository
    │
GitHub Webhook
    │
Jenkins Pipeline (EC2)
    │
Checkout Source Code
    │
Build Docker Image
    │
Run Tests (Optional)
    │
Login to Amazon ECR
    │
Push Docker Image
    │
Register ECS Task Definition Revision
    │
Update ECS Service
    │
Rolling Deployment
    │
Application Load Balancer
    │
Amazon RDS + AWS Secrets Manager
    │
Users
```

---

# Prerequisites

- GitHub repository configured
- Jenkins installed on EC2
- GitHub Webhook configured
- Docker installed on Jenkins
- AWS CLI v2 installed
- Amazon ECR repository
- ECS Cluster: `ritual-roast-cluster`
- ECS Service: `ritual-roast-service`
- Application Load Balancer
- Target Group
- Amazon RDS MySQL
- AWS Secrets Manager

---

# Pipeline Execution

## Step 1 – Developer Pushes Code

```bash
git add .
git commit -m "Feature update"
git push origin main
```

## Step 2 – GitHub Sends Webhook

GitHub automatically invokes:

```
http://<jenkins-server>:8080/github-webhook/
```

## Step 3 – Jenkins Starts Pipeline

Pipeline stages:

1. Checkout
2. Build
3. Test
4. Login to ECR
5. Push Image
6. Register Task Definition
7. Deploy to ECS
8. Verify Deployment
9. Notify

## Step 4 – Build Docker Image

```bash
docker build -t ritual-roast-app:${BUILD_NUMBER} .
```

## Step 5 – Push Image to Amazon ECR

Authenticate, tag and push the image to the ECR repository.

## Step 6 – Register New ECS Task Definition

Create a new revision referencing the newly pushed image tag.

## Step 7 – Update ECS Service

```bash
aws ecs update-service \
--cluster ritual-roast-cluster \
--service ritual-roast-service \
--force-new-deployment
```

## Step 8 – Rolling Deployment

- New tasks start
- ALB health checks pass
- Old tasks drain
- Traffic shifts to the new version

## Step 9 – Verification

Verify:

- ECS Service = Stable
- Running Tasks = Healthy
- Target Group = Healthy
- ALB returns HTTP 200
- CloudWatch Logs contain no critical errors

## Step 10 – Notifications

Recommended integrations:

- Slack
- Microsoft Teams
- Email
- Amazon SNS

---

# Failure Handling

If deployment validation fails:

- Roll back to the previous ECS Task Definition revision.
- Notify the operations team.
- Preserve build logs and deployment metadata.

---

# Best Practices

- Use immutable Docker image tags.
- Keep previous task definition revisions.
- Store secrets in AWS Secrets Manager.
- Use IAM Roles instead of static credentials.
- Enable CloudWatch monitoring and alarms.
- Enable automated rollback.

---

# Interview Summary

**Developer → GitHub → Webhook → Jenkins → Docker Build → Amazon ECR → ECS Task Definition → ECS Service → ALB → Users**

This flow represents a production-ready CI/CD pipeline for Amazon ECS Fargate.

---

# Next Enhancements

- SonarQube Code Analysis
- Trivy Image Scanning
- Blue/Green Deployment with CodeDeploy
- Manual Approval for Production
- Multi-Environment (Dev/UAT/Prod)
