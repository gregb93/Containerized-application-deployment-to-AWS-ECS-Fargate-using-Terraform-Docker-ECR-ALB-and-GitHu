# CloudDeploy Pipeline — Automated Docker CI/CD Deployment to AWS ECS

## Project Overview

CloudDeploy Pipeline is a hands-on DevOps portfolio project demonstrating containerization, continuous integration, continuous deployment, and infrastructure management using AWS and Terraform.

The project automates the process of building a Python Flask application into a Docker image, pushing the image to Amazon Elastic Container Registry (ECR), and deploying it to Amazon Elastic Container Service (ECS) using AWS Fargate.

## Technology Stack

- **Python / Flask:** Web application with a health-check endpoint
- **Docker:** Application containerization
- **GitHub Actions:** Automated CI/CD workflow
- **AWS IAM / OIDC:** Secure GitHub Actions authentication without long-lived AWS access keys
- **Amazon ECR:** Docker image registry
- **Amazon ECS / Fargate:** Serverless container hosting
- **Terraform:** Infrastructure as Code for managing the existing ECS cluster and service

## CI/CD Workflow

1. Developer pushes application code to GitHub.
2. GitHub Actions executes the deployment workflow.
3. GitHub Actions authenticates to AWS using OIDC.
4. Docker builds the application image.
5. The image is pushed to Amazon ECR.
6. GitHub Actions registers an updated ECS task definition and deploys it to the ECS service.
7. ECS Fargate runs the updated application container.

## Infrastructure Management

Terraform tracks and manages the existing ECS cluster and ECS service. The infrastructure was imported into Terraform state, validated, and checked for configuration drift.

GitHub Actions manages application deployments, while Terraform manages the ECS infrastructure configuration.

## Application Health Check

The Flask application exposes a `/health` endpoint that returns a JSON health status. This endpoint was tested against the running ECS Fargate application.

## Architecture Diagram

![AWS ECS Fargate Architecture](diagram1.png)

## Troubleshooting & Lessons Learned

### Issue: ECS Fargate Container Image Pull Failure

**Problem:**

During deployment, an ECS Fargate task encountered a `CannotPullContainerError`, preventing the application container from starting successfully.

**Root Cause:**

The ECS task execution role lacked the necessary IAM permissions to retrieve the container image from Amazon Elastic Container Registry (ECR).

**Resolution:**

Corrected the IAM permissions associated with the ECS task execution role to allow the required ECR image-pull operations.

**Lessons Learned:**

- Understood the importance of IAM permissions in ECS container deployments.
- Learned the difference between the ECS task role and task execution role.
- Gained experience troubleshooting container startup failures.
- Reinforced the importance of least-privilege IAM access.
