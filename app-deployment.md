Purpose: Instructions to deploy the sample application (2048 game) with Kubernetes resources.

# Application Deployment

## 1. Deploy Application Resources
- Deploy the 2048 game application along with its namespace, Deployment, Service, and Ingress resources:

```bash
  kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml
```

## 2. Verify Deployment
- To confirm the application has been deployed, run:

```bash
  kubectl get all -n game-2048
```
