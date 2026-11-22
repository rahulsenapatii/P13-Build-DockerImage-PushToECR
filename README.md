# Project 13: Build Docker Image and Push to Amazon ECR

## Goal
Package a lightweight Nginx web application into a Docker container image and publish it to a private Amazon Elastic Container Registry (ECR) repository in ap-south-1.

## Files
- index.html: Static HTML page served by Nginx.
- Dockerfile: Uses nginx:alpine and copies index.html into /usr/share/nginx/html/.

## Commands Used
1. Create ECR Repository:
   aws ecr create-repository --repository-name project-13-app --region ap-south-1

2. Authenticate Docker to ECR:
   ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
   aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin ${ACCOUNT_ID}.dkr.ecr.ap-south-1.amazonaws.com

3. Build, Tag, and Push Image:
   docker build -t project-13-app .
   docker tag project-13-app:latest ${ACCOUNT_ID}.dkr.ecr.ap-south-1.amazonaws.com/project-13-app:latest
   docker push ${ACCOUNT_ID}.dkr.ecr.ap-south-1.amazonaws.com/project-13-app:latest
