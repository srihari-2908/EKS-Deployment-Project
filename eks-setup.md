Purpose: Step-by-step guide to create EKS cluster.

# EKS Cluster Setup

## 1. Configure AWS CLI, eksctl, and kubectl on Local
- Follow the prerequisites in the README to install these tools.

## 2. Create EKS Cluster
- Run the following command to create a cluster with Fargate:

```bash
  eksctl create cluster --name demo-cluster --region us-east-1 --fargate
```
- On successful creation, you can see the below output..
![image](https://github.com/user-attachments/assets/1df0d0ef-daa3-473e-b72b-17e9d4fd3204)

- Now you can also verify the newly created cluster from your AWS Console.
![image](https://github.com/user-attachments/assets/a67f9f4a-f596-4594-bb1c-2b8df8b86972)

## 3. Update kubeconfig
- After the cluster is created, configure kubectl to access the EKS cluster:

```bash
aws eks update-kubeconfig --name demo-cluster
```

## 4. Create Fargate Profile
- Use this command to create a Fargate profile for the game-2048 application:

```bash
eksctl create fargateprofile \
    --cluster demo-cluster \
    --region us-east-1 \
    --name alb-sample-app \
    --namespace game-2048
```

![image](https://github.com/user-attachments/assets/bf3d9f41-5d6c-4acd-b4cb-f89e6cd6c17b)

