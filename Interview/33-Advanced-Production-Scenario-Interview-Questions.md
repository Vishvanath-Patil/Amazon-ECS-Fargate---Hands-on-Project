# 33 – Advanced Production Scenario-Based Interview Questions (Amazon ECS Fargate)

> Real-world production scenarios and model answers for Senior Cloud & DevOps Engineer interviews.

---

# 1. Production Outage

### Q: Users report the application is completely down. Where do you start?

**Answer**

Follow this order:

1. Check CloudWatch Alarms
2. Verify ALB status
3. Check Target Group health
4. Verify ECS Service events
5. Check Running Tasks
6. Review CloudWatch Logs
7. Verify RDS availability
8. Check Secrets Manager
9. Verify VPC networking
10. Roll back if required

---

# 2. ALB Returning 502 Errors

### Q: Why does ALB return HTTP 502?

**Possible Causes**

- Container crashed
- Wrong container port
- Application not listening on port 80
- Apache/PHP failed
- Target closed connection

**Troubleshooting**

- ECS Tasks
- Target Group Health
- Container Logs
- Dockerfile EXPOSE
- Task Definition Port Mapping

---

# 3. ALB Returning 503 Errors

### Q: What causes HTTP 503?

**Answer**

Usually no healthy targets.

Check:

- Target Group
- Health Checks
- ECS Tasks
- Desired Count
- ECS Service Events

---

# 4. ECS Tasks Keep Stopping

### Q: Why?

Possible reasons

- Out Of Memory (OOM)
- Application crash
- Invalid environment variables
- Secret missing
- Database connection failed
- CPU exhausted
- Image pull failed

Commands

```bash
aws ecs describe-tasks
aws logs tail <log-group> --follow
```

---

# 5. Health Check Failure

### Q: Target becomes Unhealthy.

Check:

- Health Check Path
- Port Mapping
- Security Groups
- Application startup time
- Database dependency
- Apache service

---

# 6. RDS Failover

### Q: Primary database failed.

Answer

If Multi-AZ is enabled:

- AWS promotes standby
- DNS changes automatically
- ECS reconnects using the same endpoint

Applications should retry failed connections.

---

# 7. Secrets Rotation

### Q: Password changed.

Answer

Store credentials in AWS Secrets Manager.

Rotate the secret.

Deploy new ECS Tasks or refresh applications so they retrieve the updated secret.

Never hardcode passwords.

---

# 8. CI/CD Pipeline Failure

### Q: Jenkins failed after pushing the image.

Check:

- Jenkins Console Output
- Docker build logs
- AWS CLI authentication
- Amazon ECR login
- IAM permissions
- ECS deployment events

Rollback if deployment already started.

---

# 9. Docker Build Failure

### Common Causes

- Dockerfile syntax
- Missing COPY files
- Wrong base image
- Build context
- Disk full
- Permission denied

Useful commands

```bash
docker build .
docker images
docker system df
docker logs
```

---

# 10. IAM Permission Issues

### Q: ECS cannot pull image.

Check:

Task Execution Role

Needs

- AmazonECSTaskExecutionRolePolicy

Also verify:

- ECR permissions
- CloudWatch Logs permissions

Application access uses the **Task Role**, not the Task Execution Role.

---

# 11. NAT Gateway Failure

### Symptoms

- ECS cannot pull images
- Secrets unavailable
- CloudWatch Logs stop
- ECS Exec fails

Verify:

- NAT Gateway
- Route Table
- Internet Gateway
- Elastic IP

---

# 12. Route Table Issues

### Private App Subnets

Default Route

```
0.0.0.0/0
        ↓
NAT Gateway
```

### Public Subnets

```
0.0.0.0/0
       ↓
Internet Gateway
```

Wrong routes cause internet connectivity failures.

---

# 13. ECS Auto Scaling

### Q: CPU reaches 90%.

Answer

Use ECS Service Auto Scaling.

Example

- Scale Out > 70%
- Scale In < 30%

Metrics

- CPU
- Memory
- ALB Request Count

---

# 14. Image Updated But Old Version Running

Check

- New Task Definition Revision
- ECS Service updated
- Correct image tag
- Avoid using latest
- Force new deployment

---

# 15. Zero-Downtime Deployment

Answer

- Register new Task Definition
- ECS starts new tasks
- ALB health checks pass
- Old tasks drain
- No interruption

---

# 16. Interview Tip

Always explain troubleshooting in this order:

Infrastructure
→ Networking
→ ECS
→ Containers
→ Application
→ Database
→ Logs
→ Rollback

This structured approach demonstrates production support experience.
