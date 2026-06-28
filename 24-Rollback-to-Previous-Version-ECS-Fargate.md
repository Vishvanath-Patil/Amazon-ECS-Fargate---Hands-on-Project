# 24 – Rollback to Previous Version (Amazon ECS Fargate)
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/c94b1f35-4dd4-4e6c-8aa3-66078b7bc1e4" />

> This guide explains how to safely roll back the Ritual Roast application to a previous stable release using Amazon ECS Task Definition revisions.

---

# Scenario

A new application version has been deployed, but users report issues such as:

- Application errors
- Database connectivity problems
- Increased response time
- Failed health checks
- Unexpected bugs

Instead of rebuilding the application, simply deploy the previous **Task Definition Revision**.

---

# Rollback Architecture

```text
Users
   │
Application Load Balancer
   │
ECS Service
   │
Task Definition Revision 5 (Failed)
        │
Rollback
        ▼
Task Definition Revision 4 (Stable)
```

---

# Prerequisites

- ECS Cluster: `ritual-roast-cluster`
- ECS Service: `ritual-roast-service`
- Previous Task Definition Revision exists
- Previous Docker image is still available in Amazon ECR

---

# Step 1 – Open Amazon ECS

Amazon ECS → Clusters → `ritual-roast-cluster`

---

# Step 2 – Open the ECS Service

Services → `ritual-roast-service`

Click **Update**

---

# Step 3 – Select Previous Task Definition

In **Task Definition Revision**, select the previous stable revision.

Example:

| Current | Rollback |
|---------|----------|
| ritual-roast-app-task:5 | ritual-roast-app-task:4 |

---

# Step 4 – Update Service

Leave the remaining settings unchanged.

Click **Update Service**.

Amazon ECS starts a rolling deployment automatically.

---

# Step 5 – Monitor Deployment

Verify:

- New tasks (revision 4) start successfully
- Old tasks (revision 5) drain gracefully
- Target Group health checks become Healthy
- ALB routes traffic only to healthy tasks

---

# Step 6 – Validate Rollback

- Access the ALB DNS URL
- Verify application functionality
- Review CloudWatch Logs
- Confirm database connectivity
- Ensure Target Group shows healthy targets

---

# Rollback Using AWS CLI

List task definitions

```bash
aws ecs list-task-definitions
```

Update service

```bash
aws ecs update-service   --cluster ritual-roast-cluster   --service ritual-roast-service   --task-definition ritual-roast-app-task:4
```

---

# Verification Checklist

- Previous revision is ACTIVE
- ECS deployment completed successfully
- Application is accessible
- CloudWatch Logs show no errors
- Target Group health is Healthy

---

# Common Mistakes

- Deregistering old task definitions
- Deleting previous ECR images
- Using the wrong task definition revision
- Changing multiple settings during rollback
- Ignoring Target Group health status

---

# Best Practices

- Keep multiple Task Definition revisions.
- Use semantic Docker image tags (v1.0.0, v1.0.1).
- Never overwrite production images.
- Validate every deployment before deleting old revisions.
- Monitor CloudWatch and ECS events during rollback.

---

# High-Level Rollback Flow

```text
Failed Deployment
        │
Select Previous Task Definition
        │
Update ECS Service
        │
Rolling Deployment
        │
Health Checks
        │
Traffic Restored
```

---

# Key Takeaway

Amazon ECS makes rollback simple by deploying a previous Task Definition revision. No container rebuild is required if the earlier Docker image is still available in Amazon ECR.
