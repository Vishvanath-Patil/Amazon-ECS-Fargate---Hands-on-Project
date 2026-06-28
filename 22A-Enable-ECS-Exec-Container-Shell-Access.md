# Step 22A – Enable ECS Exec (Container Shell Access)
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/90a45b90-8081-498f-81b5-a21e2bc6fb5c" />

## Overview

Amazon ECS Exec allows you to securely access running Amazon ECS Fargate containers without SSH by using AWS Systems Manager (SSM).

---

## Prerequisites

- Amazon ECS Cluster created
- ECS Service deployed
- Running ECS Tasks
- AWS CLI v2 installed
- Supported Fargate Platform Version (1.4.0+)
- IAM permissions configured
- Network connectivity to AWS Systems Manager

---

## Enable ECS Exec During Service Creation

1. Open **Amazon ECS Console**
2. Select your Cluster
3. Create or Update the ECS Service
4. Expand **Advanced Settings**
5. Enable:

```
Enable Execute Command
```

6. Deploy the service.

---

## Enable ECS Exec for an Existing Service

Amazon ECS → Clusters → ritual-roast-cluster → Services → ritual-roast-service → Update

Enable:

```
Enable Execute Command
```

Redeploy the service.

---

## Required Task Role Permissions

The **Task Role** (not the Task Execution Role) requires:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect":"Allow",
    "Action":[
      "ssmmessages:CreateControlChannel",
      "ssmmessages:CreateDataChannel",
      "ssmmessages:OpenControlChannel",
      "ssmmessages:OpenDataChannel"
    ],
    "Resource":"*"
  }]
}
```

---

## IAM Permissions for Users

Developer / Administrator should have:

- ecs:ExecuteCommand
- ecs:DescribeTasks
- ecs:DescribeServices
- ssm:StartSession

---

## Network Requirements

Tasks must be able to reach AWS Systems Manager using either:

- NAT Gateway (recommended)
- OR Interface VPC Endpoints:
  - com.amazonaws.<region>.ssm
  - com.amazonaws.<region>.ssmmessages
  - com.amazonaws.<region>.ec2messages

---

## AWS CLI Example

```bash
aws ecs execute-command \
--cluster ritual-roast-cluster \
--task <task-id> \
--container ritual-roast-app \
--interactive \
--command "/bin/bash"
```

If bash is unavailable:

```bash
--command "/bin/sh"
```

---

## Verification

AWS Console:

Amazon ECS → Clusters → Tasks → Running Task

Verify the **Execute Command** button is available.

---

## Common Issues

| Issue | Solution |
|-------|----------|
| Execute Command not visible | Enable ECS Exec and redeploy service |
| AccessDeniedException | Check IAM permissions |
| TargetNotConnectedException | Verify NAT Gateway or VPC Endpoints |
| Session Manager error | Use AWS CLI v2 |

---

## Security Best Practices

- Use least-privilege IAM policies.
- Enable CloudTrail auditing.
- Store secrets in AWS Secrets Manager.
- Disable ECS Exec if not required.
- Prefer ECS Exec over SSH.

---

## High-Level Flow

```text
Developer / Administrator
        │
AWS Console / AWS CLI
        │
Amazon ECS Service
        │
Execute Command
        │
AWS Systems Manager
        │
Running ECS Task
        │
Ritual Roast Container
```

---

## Next Step

Continue with monitoring, logging, and operational troubleshooting.
