# 27 – Complete Jenkinsfile (Production-Ready Declarative Pipeline)
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/ecb05a14-0ed8-4955-8d19-3be296a59423" />

## Overview

This guide provides a production-ready Jenkins Declarative Pipeline for deploying the Ritual Roast application to Amazon ECS Fargate.

## Prerequisites

- Jenkins with Pipeline, Git, Docker plugins
- Docker and AWS CLI v2 installed
- IAM Role or AWS credentials with ECR/ECS permissions
- Amazon ECR repository: `ritual-roast-app`
- ECS Cluster: `ritual-roast-cluster`
- ECS Service: `ritual-roast-service`

---

## Environment Variables

| Variable | Example |
|---|---|
| AWS_REGION | us-east-1 |
| ECR_REPOSITORY | ritual-roast-app |
| ECS_CLUSTER | ritual-roast-cluster |
| ECS_SERVICE | ritual-roast-service |
| TASK_FAMILY | ritual-roast-app-task |

---

## Example Jenkinsfile

```groovy
pipeline {
  agent any

  environment {
    AWS_REGION = 'us-east-1'
    AWS_ACCOUNT_ID = '<ACCOUNT_ID>'
    ECR_REPOSITORY = 'ritual-roast-app'
    IMAGE_TAG = "${BUILD_NUMBER}"
    ECS_CLUSTER = 'ritual-roast-cluster'
    ECS_SERVICE = 'ritual-roast-service'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/Vishvanath-Patil/Amazon-ECS-Fargate---Hands-on-Project.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t ritual-roast-app:${IMAGE_TAG} .'
      }
    }

    stage('Login to Amazon ECR') {
      steps {
        sh '''
        aws ecr get-login-password --region $AWS_REGION |         docker login --username AWS --password-stdin         $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
        '''
      }
    }

    stage('Push Image') {
      steps {
        sh '''
        docker tag ritual-roast-app:${IMAGE_TAG}         $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPOSITORY:${IMAGE_TAG}

        docker push         $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPOSITORY:${IMAGE_TAG}
        '''
      }
    }

    stage('Deploy to ECS') {
      steps {
        sh '''
        aws ecs update-service           --cluster $ECS_CLUSTER           --service $ECS_SERVICE           --force-new-deployment
        '''
      }
    }
  }

  post {
    success {
      echo 'Deployment completed successfully.'
    }
    failure {
      echo 'Deployment failed. Review Jenkins console logs.'
    }
    always {
      cleanWs()
    }
  }
}
```

---

## Pipeline Flow

1. Checkout source code
2. Build Docker image
3. Authenticate to Amazon ECR
4. Push Docker image
5. Trigger ECS rolling deployment
6. Verify service health

---

## Production Recommendations

- Use immutable image tags (never `latest`)
- Store AWS credentials in Jenkins Credentials or use an EC2 IAM Role
- Add SonarQube, Trivy, and unit-test stages before deployment
- Register a new ECS Task Definition revision when changing image tags
- Enable Slack/Email notifications
- Monitor ECS and CloudWatch during deployments

## Next Steps

- Integrate SonarQube
- Add Trivy image scanning
- Automate Task Definition revision updates
- Implement Blue/Green deployments with AWS CodeDeploy
