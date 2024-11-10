Purpose: Steps to set up IAM for AWS Load Balancer Controller, including OIDC provider configuration and IAM policies.

# IAM Setup for AWS Load Balancer Controller

## 1. Configure IAM OIDC Provider
- Set up the OIDC provider to allow IAM roles for service accounts:

```bash
  export cluster_name=demo-cluster
  oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)
```

Confirm the provider exists:

```bash
aws iam list-open-id-connect-providers | grep $oidc_id | cut -d "/" -f4
```
If not present, associate the provider:
```bash
eksctl utils associate-iam-oidc-provider --cluster $cluster_name --approve
```

## 2. Download IAM Policy
- Download the IAM policy for AWS Load Balancer Controller:

```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
```

## 3. Create IAM Policy
- Create an IAM policy using the downloaded JSON:

```bash
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json
```

## 4. Create IAM Role
- Attach the policy to a service account role:

```bash
eksctl create iamserviceaccount \
  --cluster=demo-cluster \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::<your-aws-account-id>:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve
```

## 5. Deploy AWS Load Balancer Controller
- Install the controller using Helm:

```bash
helm repo add eks https://aws.github.io/eks-charts
```
```bash
helm repo update
```
```bash
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \            
  -n kube-system \
  --set clusterName=demo-cluster \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=us-east-1 \
  --set vpcId=<your-vpc-id>
```

## 6. Verify Controller Deployment
- Check if the controller is running:

```bash
kubectl get deployment -n kube-system aws-load-balancer-controller
```
