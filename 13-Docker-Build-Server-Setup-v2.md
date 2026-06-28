# Step 13 -- Configure Docker Build Server

> Prepare a Linux EC2 instance to build Docker images and authenticate
> with Amazon ECR before deploying to Amazon ECS.

## Objective

Configure a build server with:

-   Docker
-   AWS CLI
-   IAM permissions
-   Amazon ECR authentication
-   Project source code

------------------------------------------------------------------------

# Prerequisites

-   Amazon Linux 2 or Amazon Linux 2023 EC2 instance
-   Internet access
-   SSH access
-   IAM Role attached to the EC2 instance **or** AWS Access Key

------------------------------------------------------------------------

# Verify Operating System

``` bash
cat /etc/os-release
uname -r
```

------------------------------------------------------------------------

# Update Server

### Amazon Linux 2023

``` bash
sudo dnf update -y
```

### Amazon Linux 2

``` bash
sudo yum update -y
```

------------------------------------------------------------------------

# Install Docker

### Amazon Linux 2023

``` bash
sudo dnf install docker -y
```

### Amazon Linux 2

``` bash
sudo amazon-linux-extras install docker -y
```

------------------------------------------------------------------------

# Start Docker

``` bash
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
```

Expected:

    Active: active (running)

------------------------------------------------------------------------

# Configure Docker for Current User

``` bash
sudo usermod -aG docker ec2-user
newgrp docker
```

Verify:

``` bash
docker ps
docker --version
docker info
```

------------------------------------------------------------------------

# Install AWS CLI

Check whether AWS CLI is already installed:

``` bash
aws --version
```

If not installed:

### Amazon Linux 2023

``` bash
sudo dnf install awscli -y
```

### Amazon Linux 2

``` bash
sudo yum install awscli -y
```

Verify:

``` bash
aws --version
```

------------------------------------------------------------------------

# Configure AWS Credentials

## Option 1 (Recommended)

Attach an IAM Role to the EC2 instance.

No manual configuration is required.

Verify:

``` bash
aws sts get-caller-identity
```

## Option 2

Configure access keys:

``` bash
aws configure
```

Provide:

-   AWS Access Key ID
-   AWS Secret Access Key
-   Default Region
-   Output Format (json)

Verify:

``` bash
aws sts get-caller-identity
```

------------------------------------------------------------------------

# Required IAM Permissions

The build server requires permissions to:

-   Amazon ECR
-   Amazon ECS (optional for deployment)
-   CloudWatch Logs (optional)

Minimum ECR actions:

-   ecr:GetAuthorizationToken
-   ecr:BatchCheckLayerAvailability
-   ecr:InitiateLayerUpload
-   ecr:UploadLayerPart
-   ecr:CompleteLayerUpload
-   ecr:PutImage
-   ecr:BatchGetImage

AWS managed policy commonly used:

-   AmazonEC2ContainerRegistryPowerUser

------------------------------------------------------------------------

# Authenticate Docker to Amazon ECR

``` bash
aws ecr get-login-password \
--region <region> | docker login \
--username AWS \
--password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
```

Expected:

    Login Succeeded

------------------------------------------------------------------------

# Prepare Project Directory

``` bash
mkdir coffee-shop-app
cd coffee-shop-app
```

Copy the application files:

-   Dockerfile
-   HTML
-   CSS
-   JavaScript
-   Images
-   Source code

Verify:

``` bash
ls -la
```

------------------------------------------------------------------------

# Validate Dockerfile

``` bash
cat Dockerfile
```

------------------------------------------------------------------------

# Connectivity Test

``` bash
ping -c 4 google.com
```

------------------------------------------------------------------------

# Validation Checklist

-   Server updated
-   Docker installed
-   Docker service running
-   User added to docker group
-   AWS CLI installed
-   AWS authentication verified
-   IAM permissions available
-   Docker authenticated with Amazon ECR
-   Project copied
-   Dockerfile verified

------------------------------------------------------------------------

# Next Step

Continue with **Step 14 -- Build Docker Image**.
