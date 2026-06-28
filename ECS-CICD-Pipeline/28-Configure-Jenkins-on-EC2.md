# 28 – Configure Jenkins on EC2

> Complete production-ready guide for installing and configuring Jenkins on an Amazon EC2 instance to build and deploy the **Ritual Roast** application to Amazon ECS Fargate.

---

# Architecture

```text
Developer
   │
GitHub
   │
Webhook
   ▼
Jenkins on EC2
   │
Docker Build
   │
Amazon ECR
   │
Amazon ECS
   │
Application Load Balancer
   │
Users
```

---

# Prerequisites

- AWS Account
- VPC and Security Group
- Amazon Linux 2023 or RHEL 9 EC2
- IAM Role attached to EC2
- Internet access

---

# Step 1 – Launch EC2

Recommended:

| Setting | Value |
|---|---|
| AMI | Amazon Linux 2023 |
| Instance | t3.medium |
| Storage | 30 GB gp3 |
| Security Group | 22, 8080 |

---

# Step 2 – Attach IAM Role

Role name:

`iam-role-grant-ec2-ssm-and-ecr-access`

Recommended policies:

- AmazonSSMManagedInstanceCore
- EC2InstanceProfileForImageBuilderECRContainerBuilds
- AmazonEC2ContainerRegistryPowerUser
- AmazonECS_FullAccess

---

# Step 3 – Install Java

```bash
sudo dnf install java-21-amazon-corretto -y
java -version
```

---

# Step 4 – Install Jenkins

```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo dnf install jenkins -y
sudo systemctl enable --now jenkins
```

---

# Step 5 – Install Docker

```bash
sudo dnf install docker -y
sudo systemctl enable --now docker
sudo usermod -aG docker jenkins
sudo usermod -aG docker ec2-user
sudo systemctl restart jenkins
```

---

# Step 6 – Install Git

```bash
sudo dnf install git -y
git --version
```

---

# Step 7 – Install AWS CLI v2

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

---

# Step 8 – Verify IAM Role

```bash
aws sts get-caller-identity
aws ecr describe-repositories
aws ecs list-clusters
```

---

# Step 9 – Open Jenkins

```
http://<EC2-Public-IP>:8080
```

Unlock:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

# Step 10 – Install Plugins

- Pipeline
- Git
- Docker Pipeline
- Credentials Binding
- Amazon ECR
- Blue Ocean (Optional)
- Workspace Cleanup

---

# Step 11 – Configure Tools

Manage Jenkins → Global Tool Configuration

- Git
- Docker
- JDK
- Pipeline

---

# Step 12 – Configure Credentials

- GitHub PAT
- AWS Credentials (if not using IAM Role)
- Slack/Webhook (optional)

---

# Step 13 – Create Pipeline Job

Pipeline → Pipeline Script from SCM

Repository:

```
https://github.com/Vishvanath-Patil/Amazon-ECS-Fargate---Hands-on-Project.git
```

Script Path:

```
Jenkinsfile
```

---

# Verification

```bash
docker --version
git --version
aws --version
java -version
systemctl status jenkins
```

Browse to Jenkins and run a test pipeline.

---

# Best Practices

- Use an EC2 IAM Role instead of static AWS keys.
- Keep Jenkins, Docker, and AWS CLI updated.
- Restrict port 8080 with Security Groups.
- Store secrets in Jenkins Credentials.
- Back up Jenkins home directory.
- Enable CloudWatch monitoring.

---

# Next Step

Proceed to **29 – Configure GitHub Webhook**.
