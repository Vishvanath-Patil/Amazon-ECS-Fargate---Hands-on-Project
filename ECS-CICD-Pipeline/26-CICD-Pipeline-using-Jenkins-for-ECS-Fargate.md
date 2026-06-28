# 26 – CI/CD Pipeline using Jenkins for Amazon ECS Fargate
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/3a6f41e1-45bf-4964-86d6-42265241e70d" />

## Objective

Automatically build, test and deploy the Ritual Roast application whenever developers push code to GitHub.

---

# Architecture

```text
Developer
   │
Git Push
   │
GitHub
   │ Webhook
Jenkins Pipeline
   │
Checkout Source
   │
Build Docker Image
   │
Run Tests (Optional)
   │
Login to Amazon ECR
   │
Push Docker Image
   │
Register New ECS Task Definition Revision
   │
Update ECS Service
   │
Rolling Deployment
   │
Application Load Balancer
   │
Users
```

---

# Prerequisites

- Jenkins server installed
- Docker installed on Jenkins
- AWS CLI v2
- Git installed
- GitHub repository
- Amazon ECR repository (ritual-roast-app)
- ECS Cluster (ritual-roast-cluster)
- ECS Service (ritual-roast-service)

---

# Step 1 - Install Jenkins Plugins

- Git
- Pipeline
- Docker Pipeline
- Credentials Binding
- Amazon ECR
- Blue Ocean (Optional)

---

# Step 2 - Configure Jenkins Credentials

## GitHub
- Username/Personal Access Token

## AWS
- Access Key ID
- Secret Access Key

## Docker Hub (Optional)

---

# Step 3 - IAM Permissions

Grant Jenkins permissions for:

- AmazonEC2ContainerRegistryPowerUser
- AmazonECS_FullAccess
- IAM PassRole (for ECS Task Execution Role)
- CloudWatchLogsReadOnlyAccess

Prefer attaching an IAM Role if Jenkins runs on EC2.

---

# Step 4 - Configure GitHub Webhook

GitHub Repository

Settings → Webhooks

Payload URL

```
http://<jenkins-server>:8080/github-webhook/
```

Content Type

```
application/json
```

Event

```
Just the push event
```

---

# Step 5 - Create Jenkins Pipeline Job

Choose **Pipeline**

Pipeline Definition

```
Pipeline script from SCM
```

SCM

```
Git
```

Repository URL

```
https://github.com/<user>/<repository>.git
```

Branch

```
main
```

Script Path

```
Jenkinsfile
```

---

# Step 6 - Jenkinsfile Stages

1. Checkout
2. Build Docker Image
3. Unit Tests (optional)
4. Login to Amazon ECR
5. Push Docker Image
6. Register ECS Task Definition Revision
7. Update ECS Service
8. Verify Deployment
9. Notify Success/Failure

---

# Step 7 - Build Docker Image

```bash
docker build -t ritual-roast-app:${BUILD_NUMBER} .
```

---

# Step 8 - Push Image to Amazon ECR

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

docker tag ritual-roast-app:${BUILD_NUMBER} <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/ritual-roast-app:${BUILD_NUMBER}

docker push <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/ritual-roast-app:${BUILD_NUMBER}
```

---

# Step 9 - Register New Task Definition Revision

Update only the image URI with the new image tag and register a new revision.

---

# Step 10 - Update ECS Service

```bash
aws ecs update-service --cluster ritual-roast-cluster --service ritual-roast-service --force-new-deployment
```

---

# Step 11 - Verify Deployment

- ECS Deployment completed
- Target Group Healthy
- ALB accessible
- CloudWatch Logs clean

---

# Rollback

If deployment fails:

- Update ECS Service to previous Task Definition revision
- OR use CodeDeploy Blue/Green rollback

---

# Best Practices

- Use immutable image tags.
- Never deploy :latest in production.
- Store secrets in Jenkins Credentials.
- Scan Docker images before deployment.
- Enable build notifications.
- Keep previous Task Definition revisions.

---

# Recommended Jenkinsfile Stages

Checkout → Build → Test → ECR Login → Push Image → Register Task Definition → Update ECS Service → Verify → Notify
