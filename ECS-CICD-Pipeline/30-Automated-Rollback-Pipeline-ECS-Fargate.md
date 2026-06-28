# 30 – Automated Rollback Pipeline (Rollback to Previous ECS Task Definition)
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/55352d97-a6b8-4cf0-9455-6466526a89cb" />

> Automatically roll back the Ritual Roast application to the previous stable Amazon ECS Task Definition revision if a deployment fails.

---

# Objective

Build a production-ready Jenkins pipeline that detects deployment failures and restores the last known good ECS Task Definition revision.

---

# Architecture

```text
Developer
   │
GitHub Push
   │
Jenkins Pipeline
   │
Build Docker Image
   │
Push Image to Amazon ECR
   │
Register New Task Definition
   │
Update ECS Service
   │
Health Checks (ALB + ECS)
   │
 ┌───────────────┐
 │ Success       │
 │ Application Live
 └───────────────┘
        │
        ▼
Failure Detected
        │
Rollback Pipeline
        │
Previous Task Definition Revision
        │
Update ECS Service
        │
Application Restored
```

---

# Prerequisites

- Jenkins Pipeline
- Amazon ECS Cluster: `ritual-roast-cluster`
- ECS Service: `ritual-roast-service`
- Amazon ECR Repository
- CloudWatch Logs
- Application Load Balancer
- Previous ECS Task Definition revisions available

---

# Rollback Strategy

1. Build and push Docker image.
2. Register a new Task Definition revision.
3. Update the ECS Service.
4. Wait for ECS deployment to stabilize.
5. Verify:
   - ECS service status
   - Target Group health
   - ALB health checks
6. If verification fails:
   - Determine the previous Task Definition revision.
   - Update the ECS Service to that revision.
   - Notify the team.

---

# Example Jenkins Pipeline Logic

```groovy
post {
    success {
        echo 'Deployment Successful'
    }

    failure {
        echo 'Deployment Failed - Starting Rollback'

        sh '''
        PREVIOUS_TASK=$(aws ecs describe-services \
          --cluster ritual-roast-cluster \
          --services ritual-roast-service \
          --query "services[0].deployments[1].taskDefinition" \
          --output text)

        aws ecs update-service \
          --cluster ritual-roast-cluster \
          --service ritual-roast-service \
          --task-definition $PREVIOUS_TASK
        '''
    }

    always {
        cleanWs()
    }
}
```

---

# Manual Rollback Command

```bash
aws ecs update-service \
  --cluster ritual-roast-cluster \
  --service ritual-roast-service \
  --task-definition <previous-task-definition-arn>
```

---

# Validation

Verify:

- ECS Service is Stable
- Running Tasks are Healthy
- Target Group Health = Healthy
- ALB returns HTTP 200
- CloudWatch Logs contain no critical errors

---

# Notifications

Recommended:

- Slack
- Microsoft Teams
- Email
- Amazon SNS

Include:

- Build Number
- Git Commit ID
- Failed Task Definition
- Restored Task Definition
- Deployment Time

---

# Common Failure Scenarios

| Failure | Rollback |
|---------|----------|
| Container startup failure | Previous Task Definition |
| Health check failure | Previous Task Definition |
| Application crash | Previous Task Definition |
| Wrong image | Previous Task Definition |
| Configuration error | Previous Task Definition |

---

# Best Practices

- Never delete old Task Definition revisions.
- Use immutable image tags.
- Enable CloudWatch alarms.
- Test rollback regularly.
- Keep at least 5 previous revisions.
- Store deployment metadata in Jenkins.

---

# High-Level Flow

```text
Git Push
   │
Jenkins
   │
Build
   │
ECR
   │
Task Definition Revision
   │
Deploy to ECS
   │
Health Check
   ├── Success → Production
   └── Failure → Rollback → Previous Revision
```

---

# Key Takeaway

Automated rollback minimizes downtime by restoring the previous healthy ECS Task Definition revision without rebuilding containers, improving deployment reliability for production workloads.
