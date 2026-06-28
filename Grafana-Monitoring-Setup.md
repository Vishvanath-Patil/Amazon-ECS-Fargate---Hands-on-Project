# Grafana Monitoring Setup (Amazon ECS Fargate)
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/834398a8-5d82-4510-8e9c-a53bd902e680" />

## Overview

This guide explains how to monitor an Amazon ECS Fargate application
using Amazon CloudWatch metrics and Grafana.

## Architecture

Amazon ECS Fargate │ ▼ Amazon CloudWatch Metrics & Logs │ ▼ Amazon
Managed Grafana (or Grafana Server) │ ▼ Dashboards & Alerts

------------------------------------------------------------------------

# Prerequisites

-   ECS Fargate Cluster deployed
-   ECS Service running
-   CloudWatch metrics enabled
-   CloudWatch Logs configured
-   Amazon Managed Grafana workspace (or self-managed Grafana)
-   IAM permissions to read CloudWatch metrics

------------------------------------------------------------------------

# Step 1 -- Create a Grafana Workspace

AWS Console

Amazon Managed Grafana → Create Workspace

Configure: - Authentication method - IAM permissions - AWS Region

Wait until the workspace status becomes **Active**.

------------------------------------------------------------------------

# Step 2 -- Access Grafana

Open the workspace URL.

Sign in using the configured authentication method.

------------------------------------------------------------------------

# Step 3 -- Add CloudWatch Data Source

Grafana → Connections → Data Sources → Add Data Source

Select **CloudWatch**.

Configuration: - Authentication: AWS SDK Default / Workspace IAM Role -
Region: Your AWS Region

Click **Save & Test**.

Expected: **Data source is working.**

------------------------------------------------------------------------

# Step 4 -- Import or Create Dashboard

Create a dashboard and add panels for:

## ECS

-   CPU Utilization
-   Memory Utilization
-   Running Tasks
-   Desired Tasks

## Application Load Balancer

-   Request Count
-   Target Response Time
-   Healthy Host Count
-   HTTP 4XX Errors
-   HTTP 5XX Errors

## Amazon RDS

-   CPU Utilization
-   Database Connections
-   Free Storage
-   Freeable Memory

------------------------------------------------------------------------

# Step 5 -- Configure Variables

Create variables:

-   Region
-   Cluster
-   Service
-   Load Balancer
-   Database

This allows dashboard filtering.

------------------------------------------------------------------------

# Step 6 -- Create Alerts

Recommended alerts:

-   ECS CPU \> 70%
-   ECS Memory \> 80%
-   ALB Healthy Hosts \< 1
-   ALB 5XX Errors \> 0
-   RDS CPU \> 80%
-   Low Free Storage

Configure notification channels such as Email, Slack, or Amazon SNS.

------------------------------------------------------------------------

# Verification

Confirm:

-   ECS metrics update every few minutes.
-   CloudWatch logs are available.
-   Dashboards display live metrics.
-   Alerts trigger when thresholds are exceeded.

------------------------------------------------------------------------

# Best Practices

-   Use dashboard variables.
-   Group ECS, ALB, and RDS metrics.
-   Keep alert thresholds realistic.
-   Review dashboards after each deployment.
-   Monitor Auto Scaling events.

------------------------------------------------------------------------

# Troubleshooting

## No metrics visible

-   Verify CloudWatch data source.
-   Check IAM permissions.
-   Confirm the correct AWS Region.

## Dashboard empty

-   Select the correct cluster and service.
-   Ensure ECS tasks are running.

## Alerts not firing

-   Verify evaluation interval.
-   Check notification channel configuration.

------------------------------------------------------------------------

# Expected Result

A centralized Grafana dashboard showing ECS Fargate, ALB, CloudWatch,
and Amazon RDS health with alerting for critical conditions.
