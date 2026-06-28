# Step 19A – Create Target Group for Application Load Balancer (ALB)

> Create an **Application Load Balancer Target Group** for the Ritual Roast application running on Amazon ECS Fargate.

---

# Objective

Create a Target Group that receives requests from the Application Load Balancer and forwards them to healthy ECS Fargate tasks.

## Architecture

```text
Internet
    │
    ▼
Application Load Balancer
    │
    ▼
Target Group (ritual-roast-tg)
    │
    ▼
ECS Service
    │
    ▼
ECS Tasks (Fargate)
    │
    ▼
Ritual Roast PHP Application
```

## Why Target Group?

- Registers ECS tasks
- Performs health checks
- Routes traffic only to healthy tasks
- Automatically updates targets during scaling

## Prerequisites

- ECS Cluster created
- ECS Task Definition created
- Docker image pushed to Amazon ECR
- ALB Security Group created
- VPC: `ritual-roast-vpc`
- App Subnets:
  - `rr-app-subnet1-us-east-1a`
  - `rr-app-subnet2-us-east-1b`

## AWS Console

`EC2 → Load Balancing → Target Groups → Create Target Group`

## Basic Configuration

| Setting | Value |
|---|---|
| Target Type | IP |
| Protocol | HTTP |
| Port | 80 |
| IP Address Type | IPv4 |
| VPC | ritual-roast-vpc |
| Name | ritual-roast-tg |

## Health Check

| Setting | Value |
|---|---|
| Protocol | HTTP |
| Path | / |
| Healthy Threshold | 3 |
| Unhealthy Threshold | 3 |
| Timeout | 5 seconds |
| Interval | 30 seconds |
| Success Codes | 200 |

## Register Targets

Do **not** manually register targets.

Amazon ECS automatically registers and deregisters task IP addresses when the ECS Service is created.

## Verification

- Target Group: `ritual-roast-tg`
- Target Type: `IP`
- Protocol: `HTTP`
- Port: `80`
- Health Check Path: `/`
- Initial status: **0 Targets** (expected until ECS Service is created)

## Common Mistakes

- Choosing **Instance** instead of **IP**
- Wrong VPC
- Wrong health check path
- Wrong application port
- Manual target registration

## Best Practices

- Use IP target type for Fargate
- One Target Group per application
- Keep health checks lightweight
- Restrict access using the ALB Security Group

## High-Level Flow

```text
Internet
   │
   ▼
Application Load Balancer
   │
   ▼
Target Group
   │
   ▼
ECS Service
   │
   ▼
ECS Tasks
   │
   ▼
Ritual Roast Application
```

## Next Step

**Step 19B – Create Application Load Balancer (ALB)**
