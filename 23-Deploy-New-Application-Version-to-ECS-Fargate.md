# Deployment Guide - Deploy a New Application Version to Amazon ECS Fargate

## Scenario

A developer has made application code changes for the Ritual Roast application. The Cloud & DevOps team must deploy the new version with minimum downtime.

---

# Deployment Flow

Developer
  |
  | Push code to Git
  v
Docker Build Server
  |
Build Docker Image
  |
Tag Image (v1.0.1)
  |
Push Image to Amazon ECR
  |
Register New ECS Task Definition Revision
  |
Update ECS Service
  |
Rolling Deployment
  |
Application Load Balancer Health Checks
  |
Traffic shifts to new tasks
  |
Deployment Complete

---

# Prerequisites

- Updated application source code
- Docker Build Server
- Amazon ECR repository: ritual-roast-app
- ECS Cluster: ritual-roast-cluster
- ECS Service: ritual-roast-service
- Task Definition: ritual-roast-app-task

---

# Step 1 - Pull Latest Code

```bash
git pull origin main
```

---

# Step 2 - Build Docker Image

```bash
docker build -t ritual-roast-app:v1.0.1 .
```

---

# Step 3 - Login to Amazon ECR

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
```

---

# Step 4 - Tag Image

```bash
docker tag ritual-roast-app:v1.0.1 <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/ritual-roast-app:v1.0.1
```

---

# Step 5 - Push Image

```bash
docker push <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/ritual-roast-app:v1.0.1
```

---

# Step 6 - Create New Task Definition Revision

Amazon ECS → Task Definitions

Select **ritual-roast-app-task**

Choose **Create new revision**

Update only the Image URI to the new image tag:

```
<ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/ritual-roast-app:v1.0.1
```

Save the new revision.

---

# Step 7 - Update ECS Service

Amazon ECS

→ Clusters

→ ritual-roast-cluster

→ Services

→ ritual-roast-service

→ Update

Select the latest Task Definition revision and deploy.

---

# Step 8 - Monitor Deployment

Verify:

- New tasks start successfully
- Target Group health checks are Healthy
- Old tasks drain automatically
- ALB routes traffic to new tasks

---

# Step 9 - Validation

- Open ALB DNS
- Verify application
- Check CloudWatch Logs
- Verify database connectivity

---

# Rollback

If deployment fails:

1. Update ECS Service.
2. Select the previous Task Definition revision.
3. Deploy.
4. ECS performs a rolling rollback.

---

# Best Practices

- Never use only `latest`; use version tags.
- Register a new Task Definition revision for every release.
- Do not modify running tasks.
- Validate in a lower environment before production.
- Monitor CloudWatch Logs and Target Group health.

---

# High-Level Release Flow

Developer → Git → Docker Build Server → Amazon ECR → ECS Task Definition Revision → ECS Service Update → Rolling Deployment → ALB → Users
