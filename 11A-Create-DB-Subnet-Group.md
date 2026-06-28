# Step 11A -- Create Amazon RDS DB Subnet Group

> **Important:** Before creating the Amazon RDS MySQL instance, create a
> **DB Subnet Group**. This allows Amazon RDS to know which private
> subnets can host the database.

------------------------------------------------------------------------

# Purpose of a DB Subnet Group
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/059a0e78-0f80-4380-98e9-9c17f87722c1" />


A **DB Subnet Group** is a collection of **private subnets** in
different Availability Zones that Amazon RDS uses when deploying a
database instance.

## Why is it required?

-   Specifies where the RDS instance can be launched.
-   Enables Multi-AZ deployments (when enabled).
-   Keeps the database in **private subnets** instead of public subnets.
-   Provides high availability by including subnets from multiple
    Availability Zones.
-   Required when creating an RDS instance inside a VPC.

------------------------------------------------------------------------

# Architecture

``` text
VPC
├── Public Subnet A
├── Public Subnet B
├── App Subnet A
├── App Subnet B
├── DB Subnet A  ← Included
└── DB Subnet B  ← Included

DB Subnet Group
     │
     ▼
Amazon RDS MySQL
```

------------------------------------------------------------------------

# Prerequisites

-   VPC created
-   Two Database Private Subnets created
-   Internet Gateway and NAT Gateway configured
-   Database Security Group created

------------------------------------------------------------------------

# Steps

## 1. Open Amazon RDS

AWS Console → Amazon RDS

------------------------------------------------------------------------

## 2. Navigate to Subnet Groups

Left Menu → **Subnet groups**

Click **Create DB subnet group**

------------------------------------------------------------------------

## 3. Basic Configuration

  Setting       Example
  ------------- -----------------------------------------
  Name          coffee-shop-db-subnet-group
  Description   DB subnet group for Coffee Shop project
  VPC           Select your project VPC

------------------------------------------------------------------------

## 4. Add Database Subnets

Select only the **private database subnets**.

Example:

  Availability Zone   Subnet
  ------------------- ---------------------
  AZ-A                DB Private Subnet 1
  AZ-B                DB Private Subnet 2

**Do not select public or application subnets.**

------------------------------------------------------------------------

## 5. Create the Subnet Group

Click **Create**.

------------------------------------------------------------------------

# Verification

Verify that:

-   Status is **Complete**
-   Correct VPC is selected
-   Two private DB subnets are listed
-   Two Availability Zones are included

------------------------------------------------------------------------

# Common Mistakes

-   Selecting public subnets
-   Selecting application subnets
-   Including only one Availability Zone
-   Using subnets from different VPCs

------------------------------------------------------------------------

# Best Practices

-   Use at least two private subnets.
-   Use different Availability Zones.
-   Reserve DB subnets exclusively for databases.
-   Apply the Database Security Group to the RDS instance.

------------------------------------------------------------------------

# Next Step

Proceed to **Step 11B -- Create Amazon RDS MySQL Database** and select
this DB Subnet Group during database creation.
