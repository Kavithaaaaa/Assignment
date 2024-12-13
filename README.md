# DevOps Assignment: Deploying a React Application to AWS EKS

## Overview

This project demonstrates the deployment of a ReactJS application on an AWS EKS cluster. It involves creating the necessary AWS infrastructure, containerizing the application, and configuring Kubernetes resources for seamless deployment. The application is exposed to the internet via an ALB (Application Load Balancer), ensuring scalability and high availability.

## Steps Taken

### 1. **VPC Creation**
- Created a custom VPC with two public subnets and two private subnets.
- Attached an Internet Gateway (IGW) to the public subnets.
- Configured a NAT Gateway for private subnets to allow outbound internet access for resources like EKS node groups.

### 2. **ECR Repository**
- Created an ECR repository to store the Docker image of the React application.

### 3. **ReactJS Application**
- Cloned the ReactJS application from [AhmedM1011/task](https://github.com/AhmedM1011/task.git).
- Dockerized the application with the following features:
  - Used Node.js for building the app.
  - Served the app using Nginx for production-ready deployment.
- Built the Docker image locally.
- Pushed the Docker image to the ECR repository.

### 4. **IAM Roles and Security Groups**
- Created two IAM roles:
  1. Role for EKS Cluster.
  2. Role for EKS Node Group.
- Configured security groups to allow communication between the ALB, EKS nodes, and the application pods within the VPC CIDR.

### 5. **EKS Cluster and Node Group**
- Deployed an EKS cluster with nodes residing only in private subnets to ensure security.
- Used eksctl or Terraform for streamlined EKS creation.

### 6. **Kubernetes Resources**
- Created Kubernetes manifests for deploying and exposing the application:
  - **Deployment**: Manages the application pods using the Docker image stored in ECR.
  - **Service**: Configured as a `ClusterIP` service to route traffic internally within the cluster.
  - **Ingress**: Configured an ALB ingress to expose the service to the internet and route traffic based on HTTP rules.

### 7. **ALB Configuration**
- Configured an ALB to:
  - Route internet traffic to the EKS cluster.
  - Use a health check to monitor the application pod’s availability.
  - Serve the application on HTTP port 80.

## How to Test

1. **Accessing the Application**:
   - Retrieve the ALB's DNS name (from AWS Console or CLI).
   - Open the DNS name in a browser or Postman to access the application.

2. **Verification Steps**:
   - Ensure the ALB is healthy by checking the target group status.
   - Verify the React application loads correctly in the browser.

3. **Kubernetes Validation**:
   - Run `kubectl get pods` to ensure the pod is running.
   - Run `kubectl describe ingress` to confirm the ingress configuration.

## Installation Details

### **Pre-Requisites**
1. **AWS CLI**: Install the AWS CLI and configure it with appropriate credentials.
   
   aws configure

2. kubectl: Install kubectl to manage the Kubernetes cluster

   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

3. eksctl: Install eksctl to simplify EKS cluster creation.

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin/

4. Helm: Install Helm for managing Kubernetes charts.

curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

