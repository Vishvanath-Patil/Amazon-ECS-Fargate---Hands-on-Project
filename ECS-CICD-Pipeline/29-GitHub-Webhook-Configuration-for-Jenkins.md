# 29 – Configure GitHub Webhook for Jenkins
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/b854cf79-28e9-4a45-8701-3695e30de71e" />

> Configure a GitHub Webhook so every push to the repository automatically triggers the Jenkins CI/CD pipeline.

---

# Architecture

```text
Developer
   │
git push
   ▼
GitHub Repository
   │
Webhook (HTTP POST)
   ▼
Jenkins
   │
Pipeline
   ▼
Docker Build
   ▼
Amazon ECR
   ▼
Amazon ECS Fargate
```

---

# Prerequisites

- Jenkins running on EC2
- Public DNS/IP reachable from GitHub
- Port **8080** open (or reverse proxy)
- GitHub repository
- Jenkins Pipeline job created
- GitHub plugin installed

---

# Step 1 – Open GitHub Repository

```
GitHub
 → Repository
 → Settings
 → Webhooks
 → Add webhook
```

---

# Step 2 – Configure Payload URL

Example:

```text
http://<jenkins-public-ip>:8080/github-webhook/
```

If using HTTPS:

```text
https://jenkins.example.com/github-webhook/
```

---

# Step 3 – Content Type

Select:

```
application/json
```

---

# Step 4 – Secret (Recommended)

Create a strong webhook secret and configure the same value in Jenkins.

Example:

```text
ritual-roast-webhook-secret
```

---

# Step 5 – Events

Choose:

```
Just the push event
```

(Optional)

- Pull Request
- Release

---

# Step 6 – Save Webhook

Click **Add webhook**.

GitHub immediately sends a **Ping** request.

A successful response should show:

```
✓ 200 OK
```

---

# Step 7 – Configure Jenkins

Pipeline Job

```
Build Triggers
```

Enable:

```
GitHub hook trigger for GITScm polling
```

Save the job.

---

# Step 8 – Test

```bash
git add .
git commit -m "Update Ritual Roast"
git push origin main
```

Expected flow:

1. GitHub receives push
2. Webhook sends POST request
3. Jenkins starts pipeline
4. Docker image builds
5. Image pushed to Amazon ECR
6. ECS Service deploys latest version

---

# Verification

GitHub

```
Settings
→ Webhooks
→ Recent Deliveries
```

Status should be:

```
200 OK
```

Jenkins

```
Dashboard
→ Pipeline Job
→ Build History
```

---

# Troubleshooting

| Issue | Solution |
|-------|----------|
| 404 | Verify `/github-webhook/` URL |
| 403 | Check webhook secret and security rules |
| Timeout | Verify EC2 Security Group and public access |
| Build not triggered | Enable GitHub hook trigger |
| Connection refused | Ensure Jenkins service is running |

---

# Security Best Practices

- Use HTTPS.
- Configure a webhook secret.
- Restrict Jenkins access by IP where possible.
- Do not expose unnecessary ports.
- Rotate GitHub Personal Access Tokens regularly.

---

# High-Level Flow

```text
Developer
   │
Git Push
   │
GitHub
   │
Webhook
   │
Jenkins
   │
Pipeline
   │
Amazon ECR
   │
Amazon ECS
   │
Application Live
```

---

# Next Step

**30 – End-to-End CI/CD Pipeline Execution**
