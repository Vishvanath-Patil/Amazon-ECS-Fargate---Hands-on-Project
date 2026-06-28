# Logging and Monitoring (Amazon ECS Fargate)
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/78fbcced-c377-4aa1-84c2-6b8cc6f95944" />

## Overview

This document explains how to monitor and troubleshoot the Ritual Roast
application running on Amazon ECS Fargate using AWS native services.

## Services Used

-   Amazon CloudWatch Logs
-   Amazon CloudWatch Metrics
-   Amazon ECS
-   Application Load Balancer
-   Amazon RDS

------------------------------------------------------------------------

# Prerequisites

-   ECS Cluster is running
-   ECS Service is deployed
-   Task Definition uses the **awslogs** log driver
-   IAM Task Execution Role includes CloudWatch Logs permissions

------------------------------------------------------------------------

# Step 1 - Enable CloudWatch Logs

During **Task Definition** creation:

Container → Logging

Select:

-   Log Driver: **awslogs**
-   Log Group: `/ecs/ritual-roast`
-   Region: Your AWS Region
-   Stream Prefix: `ecs`

Register the updated Task Definition and update the ECS Service.

------------------------------------------------------------------------

# Step 2 - View Container Logs

AWS Console

CloudWatch → Logs → Log Groups → `/ecs/ritual-roast`

Open the latest log stream to verify:

-   Application started successfully
-   Database connection established
-   No runtime errors

------------------------------------------------------------------------

# Step 3 - Monitor ECS Service

AWS Console

ECS → Cluster → Service → Metrics

Monitor:

-   Running Task Count
-   Desired Task Count
-   CPU Utilization
-   Memory Utilization
-   Service Events

Expected: - Tasks = RUNNING - CPU and Memory within normal limits

------------------------------------------------------------------------

# Step 4 - Monitor Application Load Balancer

EC2 → Load Balancers → Select ALB → Monitoring

Check:

-   Healthy Host Count
-   Request Count
-   Target Response Time
-   HTTP 4XX Errors
-   HTTP 5XX Errors

------------------------------------------------------------------------

# Step 5 - Monitor Target Group

EC2 → Target Groups

Verify:

-   Target Health = Healthy
-   All registered Fargate tasks are healthy

------------------------------------------------------------------------

# Step 6 - Monitor Amazon RDS

RDS → Databases → Monitoring

Monitor:

-   CPU Utilization
-   Database Connections
-   Free Storage Space
-   Freeable Memory
-   Read/Write IOPS

------------------------------------------------------------------------

# Step 7 - CloudWatch Alarms (Recommended)

Create alarms for:

-   ECS CPU \> 70%
-   ECS Memory \> 80%
-   ALB Healthy Hosts \< 1
-   RDS CPU \> 80%
-   RDS Free Storage Low

------------------------------------------------------------------------

# Troubleshooting

## ECS Task Stops

Check: - ECS Service Events - CloudWatch Logs - Task Definition -
Secrets Manager values

## Target Unhealthy

Check: - Health Check Path - Container Port - Security Groups

## Database Connection Failed

Check: - RDS endpoint - Security Group rules - Database credentials in
Secrets Manager

## No Logs Available

Verify: - awslogs driver configured - IAM Task Execution Role
permissions - Correct log group and region

------------------------------------------------------------------------

# Best Practices

-   Enable CloudWatch Logs for every container.
-   Review ECS Service events after deployments.
-   Create CloudWatch alarms for critical metrics.
-   Monitor ALB target health after each deployment.
-   Monitor RDS resource utilization regularly.

------------------------------------------------------------------------

# Expected Result

-   Application logs are available in CloudWatch.
-   ECS tasks remain healthy.
-   ALB target group remains healthy.
-   Database operates without connectivity issues.
-   CloudWatch alarms notify on abnormal conditions.
