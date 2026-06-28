# Step 13 -- Configure Docker Build Server
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/95972830-ce68-4681-a15b-76779a587473" />

Prepare an EC2 build server to build Docker images and push them to
Amazon ECR.

## 13.1 Launch EC2

Recommended: - Amazon Linux 2023 (Amazon Linux 2 supported) -
t3.medium - 20 GB gp3 - SSH (22) allowed from your IP

## 13.2 IAM Role

Role Name:

`iam-role-grant-ec2-ssm-and-ecr-access`

Attach:

-   AmazonSSMManagedInstanceCore
-   EC2InstanceProfileForImageBuilderECRContainerBuilds

Attach the role to the EC2 instance.

Verify:

``` bash
aws sts get-caller-identity
```

## 13.3 Connect

``` bash
ssh -i key.pem ec2-user@<public-ip>
```

or AWS Systems Manager Session Manager.

## 13.4 Update

Amazon Linux 2023

``` bash
sudo dnf update -y
```

Amazon Linux 2

``` bash
sudo yum update -y
```

## 13.5 Install Docker

Amazon Linux 2023

``` bash
sudo dnf install docker -y
```

Amazon Linux 2

``` bash
sudo amazon-linux-extras install docker -y
```

## 13.6 Start Docker

``` bash
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
```

## 13.7 Configure Docker

``` bash
sudo usermod -aG docker ec2-user
newgrp docker
docker --version
docker ps
```

## 13.8 Install AWS CLI

Check:

``` bash
aws --version
```

If missing:

Amazon Linux 2023

``` bash
sudo dnf install awscli -y
```

Amazon Linux 2

``` bash
sudo yum install awscli -y
```

Verify:

``` bash
aws sts get-caller-identity
```

## 13.9 Login to Amazon ECR

``` bash
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
```

Expected:

    Login Succeeded

## 13.10 Prepare Project

``` bash
mkdir coffee-shop-app
cd coffee-shop-app
ls -la
```

Copy Dockerfile and application source.

## 13.11 Validate Dockerfile

``` bash
cat Dockerfile
```

## 13.12 Checklist

-   IAM role attached
-   AmazonSSMManagedInstanceCore
-   EC2InstanceProfileForImageBuilderECRContainerBuilds
-   Docker installed
-   AWS CLI installed
-   ECR login successful
-   Project copied

## 13.13 Next

Proceed to **Step 14 -- Build Docker Image**.
