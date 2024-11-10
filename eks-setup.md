Purpose: Step-by-step guide to create EKS cluster.

# EKS Cluster Setup

## 1. Configure AWS CLI, eksctl, and kubectl on Local
- Follow the prerequisites in the README to install these tools.

## 2. Create EKS Cluster
- Run the following command to create a cluster with Fargate:

  ```bash
  eksctl create cluster --name demo-cluster --region us-east-1 --fargate
  
## 3. Update kubeconfig
- After the cluster is created, configure kubectl to access the EKS cluster:

  ```bash
aws eks update-kubeconfig --name demo-cluster

## 4. Create Fargate Profile
- Use this command to create a Fargate profile for the game-2048 application:

  ```bash
eksctl create fargateprofile \
    --cluster demo-cluster \
    --region us-east-1 \
    --name alb-sample-app \
    --namespace game-2048
```
