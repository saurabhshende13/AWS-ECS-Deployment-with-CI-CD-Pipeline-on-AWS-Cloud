# AWS-ECS-Deployment-with-CI-CD-Pipeline-on-AWS-Cloud

![architecture](Steps/project.png)

This project demonstrates the deployment of a Docker image into an Amazon ECS (Elastic Container Service) Cluster and the creation of a CI/CD pipeline for automating the entire process. The project utilizes various AWS resources and services including Amazon ECR (Elastic Container Registry), ECS, CodeCommit, CodeBuild, CodeDeploy, and CodePipeline.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Architecture](#architecture)
- [Steps](#steps)
  - [Step 1: Create ECR Repository](#step-1-create-ecr-repository)
  - [Step 2: Create AWS User](#step-2-create-aws-user)
  - [Step 3: Create and Push Image to AWS ECR Repository](#step-3-create-and-push-image-to-aws-ecr-repository)
  - [Step 4: Create Security Groups](#step-4-create-security-groups)
  - [Step 5: Create ECS Fargate Cluster](#step-5-create-ecs-fargate-cluster)
  - [Step 6: Create Task Definition](#step-6-create-task-definition)
  - [Step 7: Create ECS Service with Application Load Balancer](#step-7-create-ecs-service-with-application-load-balancer)
  - [Step 8: Create CodeCommit Repository](#step-8-create-codecommit-repository)
  - [Step 9: Create CodeBuild Project](#step-9-create-codebuild-project)
  - [Step 10: Create CodePipeline](#step-10-create-codepipeline)
- [License](#license)

## Prerequisites

- AWS Account
- AWS CLI installed and configured
- Docker installed

## Architecture

The architecture diagram and reference files for this project can be found in the [GitHub repository](https://github.com/saurabhshende13/AWS-ECS-Deployment-with-CI-CD-Pipeline-on-AWS-Cloud.git).

## Steps

### Step 1: Create ECR Repository

Create an ECR repository to store your Docker images.

1. Go to the ECR console.
2. Click on "Create repository".

![step1a](Steps/Step-1a.png)

4. Follow the instructions to create the repository.
5. Click on "View push commands" to get the commands for pushing your Docker image to ECR.

![step1b](Steps/Step1b.png)

### Step 2: Create AWS User

Create an AWS user with the necessary permissions to access ECR and ECS.

1. Go to the IAM console.
2. Click on "Users" and then "Add user".
3. Assign `AmazonEC2ContainerRegistryFullAccess` policy to the user.

![step2a](Steps/Step2a.png)

5. Configure security credentials for the user to be used for uploading Docker images.

### Step 3: Create and Push Image to AWS ECR Repository

Install necessary tools and push your Docker image to ECR.

```bash
# Update and install dependencies
sudo apt update -y
sudo apt install zip unzip git -y
sudo apt install docker.io -y

# Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Configure AWS CLI
sudo aws configure
# Provide the access key and secret key here

# Clone the repository
git clone https://github.com/saurabhshende13/AWS-ECS-Deployment-with-CI-CD-Pipeline-on-AWS-Cloud.git

# Navigate to project directory
cd AWS-ECS-Deployment-with-CI-CD-Pipeline-on-AWS-Cloud

# Build the Docker image
sudo docker build -t devildevops .

# Fix Docker permission error
sudo usermod -aG docker $USER
newgrp docker
sudo chmod 666 /var/run/docker.sock

# Push the Docker image to ECR
sudo aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <your-account-id>.dkr.ecr.us-east-1.amazonaws.com
sudo docker tag devildevops:latest <your-account-id>.dkr.ecr.us-east-1.amazonaws.com/devildevops:latest
sudo docker push <your-account-id>.dkr.ecr.us-east-1.amazonaws.com/devildevops:latest
```

### Step 4: Create Security Groups

Create security groups for the Application Load Balancer (ALB).

![step4a](Steps/Step-4a.png)

![step4a](Steps/Step4b.png)

### Step 5: Create ECS Fargate Cluster

Create an ECS Fargate cluster to run your containers.

![step5a](Steps/Step5a.png)

### Step 6: Create Task Definition

Define the task that specifies which Docker image to use and the necessary resources.

![step6a](Steps/Step6a.png)

![step6b](Steps/Step6b.png)

![step6c](Steps/Step6c.png)

### Step 7: Create ECS Service with Application Load Balancer

Create an ECS service that uses the task definition and is associated with the ALB.

**Note:** Update the ALB security group with the ALB-to-ECS security group.

![step7a](Steps/Step7a.png)

![step7b](Steps/Step7b.png)

![step7c](Steps/Step7c.png)

![step7d](Steps/Step7d.png)

![step7e](Steps/Step7e.png)

### Step 8: Create CodeCommit Repository

Create a CodeCommit repository and push your code to it.

![step8a](Steps/Step8a.png)

Provide user below permissions.

![step8b](Steps/Step8b.png)

Create https git credentials for user to push the code.

![step8c](Steps/Step8c.png)

Clone the codecommit repository and push you code

![step8d](Steps/Step8d.png)

### Step 9: Create CodeBuild Project

Create a CodeBuild project to build and test your code.

![step9a](Steps/Step9a.png)

![step9b](Steps/Step9b.png)

![step9c](Steps/Step9c.png)

![step9d](Steps/Step9d.png)

Give below permission to IAM User.

![step9e](Steps/Step9e.png)

### Step 10: Create CodePipeline

Create a CodePipeline to automate the CI/CD process, including CodeCommit, CodeBuild, and ECS deployment steps.

![step10a](Steps/Step10a.png)

![step10b](Steps/Step10b.png)

Add variables in the build section to replace values in `buildspec.yml`.

![step10c](Steps/Step10c.png)

![step10d](Steps/Step10d.png)

![step10e](Steps/Step10e.png)


## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
