# Auto Scaling Setup (Amazon ECS Fargate)
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/01139e65-1bea-43ad-a98d-b61c3781f1e2" />

## Overview

Amazon ECS Service Auto Scaling automatically adjusts the number of
running Fargate tasks based on CloudWatch metrics such as CPU and Memory
utilization.

## Prerequisites

-   ECS Cluster
-   ECS Service (Fargate)
-   Application Load Balancer
-   CloudWatch metrics enabled

## Recommended Configuration

  Setting              Value
  -------------------- -------------------------------------
  Minimum Tasks        2
  Desired Tasks        2
  Maximum Tasks        6
  Metric               ECS Service Average CPU Utilization
  Target Value         60%
  Scale-out Cooldown   60 seconds
  Scale-in Cooldown    120 seconds

------------------------------------------------------------------------

# Step 1 - Open ECS Service

AWS Console

ECS → Clusters → *Your Cluster* → Services → *Your Service*

Click **Update Service** or open the **Auto Scaling** tab.

------------------------------------------------------------------------

# Step 2 - Configure Auto Scaling

Enable **Service Auto Scaling**.

Configure:

-   Minimum Tasks: 2
-   Desired Tasks: 2
-   Maximum Tasks: 6

------------------------------------------------------------------------

# Step 3 - Scaling Policy

Choose:

-   Policy Type: Target Tracking
-   Metric: ECS Service Average CPU Utilization
-   Target Value: 60%

Alternative:

-   ECS Service Average Memory Utilization
-   Target Value: 70%

------------------------------------------------------------------------

# Step 4 - Cooldown

Scale Out Cooldown: 60 seconds

Scale In Cooldown: 120 seconds

------------------------------------------------------------------------

# Step 5 - Save

Click **Update**.

------------------------------------------------------------------------

# Verification

Navigate to:

ECS → Cluster → Service → Auto Scaling

Verify:

-   Auto Scaling Enabled
-   Min = 2
-   Max = 6
-   Target Tracking Policy = Active

CloudWatch → Alarms

Confirm alarms were created automatically.

------------------------------------------------------------------------

# Test Auto Scaling

Generate load using multiple browser sessions or a load-testing tool.

Expected:

-   CPU \> 60% → New Fargate task starts.
-   CPU decreases → Extra task stops after cooldown.

------------------------------------------------------------------------

# Best Practices

-   Keep at least 2 tasks in production.
-   Place tasks across multiple Availability Zones.
-   Use Target Tracking instead of Step Scaling for most workloads.
-   Monitor CloudWatch alarms regularly.

------------------------------------------------------------------------

# Troubleshooting

## Tasks are not scaling

Check:

-   Auto Scaling enabled
-   Service desired count not fixed manually
-   CloudWatch metrics available

## Scale-out never happens

Verify:

-   CPU exceeds target
-   Maximum task count is greater than desired count

## Tasks never scale in

Check:

-   Scale-in cooldown
-   Running CPU utilization
-   Ongoing deployments

------------------------------------------------------------------------

## Expected Result

✔ ECS automatically launches additional Fargate tasks during high load.

✔ ECS automatically removes extra tasks after demand decreases.
