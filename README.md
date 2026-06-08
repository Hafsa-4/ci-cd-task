# CI/CD Pipeline Task

## Overview
This project demonstrates a complete CI/CD pipeline using GitHub Actions, Jenkins, Docker, AWS EC2, and AWS S3.

## Application
Simple Node.js web app with `/health` endpoint returning:
```json
{"status": "UP", "message": "App is running"}
```

## Pipeline 1 - GitHub Actions Validation
- Checkout code
- Install dependencies
- Run basic tests
- Validate Docker build

## Pipeline 2 - Jenkins Build & Test
- Pull latest code from GitHub
- Build Docker image
- Run container locally
- Check /health endpoint

## Pipeline 3 - Jenkins Deploy
- Connect to AWS EC2
- Stop and remove old container
- Run new Docker container on EC2
- Upload deployment log to S3

## Setup Steps
1. Create EC2 instances (Jenkins server + Deploy server)
2. Install Jenkins on server 1
3. Install Docker on both servers
4. Configure Jenkins credentials (Docker Hub + EC2 SSH key)
5. Create GitHub repo with app code
6. Configure Jenkins pipeline job
7. Push code to trigger pipelines

## URLs
- App Health Check: http://100.31.161.88:8080/health
- Jenkins: http://18.209.102.78:8080

## Author
Hafsa
