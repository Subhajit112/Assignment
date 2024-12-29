# Cloud Native Web Application

This project demonstrates a cloud-native web application deployed on Kubernetes with Prometheus monitoring.

## Prerequisites

- Docker
- Kubernetes cluster (EKS)
- kubectl
- Terraform
- AWS CLI

## Deployment Steps

1. **Infrastructure Setup**

```bash
# Initialize and apply Terraform configuration
cd terraform
terraform init
terraform apply
aws eks update-kubeconfig --region <REGION> --name <CLUSTER_NAME>
```

2. **Build and Push Docker Image**

```bash
# Build the Docker image
docker build -t web-app .

# Tag and push to your container registry
docker tag web-app:latest <dockerhub>registry/web-app:latest
docker push <dockerhub>/web-app:latest
```

3. **Deploy to Kubernetes**

```bash
# Update kubeconfig for EKS cluster
aws eks update-kubeconfig --region <Region> --name <cluster name>

# Deploy the application
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

4. **Setup Monitoring**

```bash
# Deploy Prometheus
kubectl create namespace monitoring
kubectl apply -f k8s/prometheus-config.yaml
```

## Accessing the Application

1. Get the LoadBalancer URL:
```bash
kubectl get svc web-app
```

2. Open the URL in your browser to access the application.

## Monitoring

Access Prometheus:
```bash
kubectl port-forward svc/prometheus-server 9090:9090 -n monitoring
```

Visit `http://localhost:9090` to access the Prometheus dashboard.

## Architecture

- Static page served by Nginx
- Kubernetes deployment with 2 replicas
- LoadBalancer service for external access
- Prometheus monitoring
- AWS EKS cluster managed by Terraform

## Monitoring Metrics

The following metrics are available in Prometheus:
- HTTP request rate
- Response times
- Error rates
- Pod health status
- Resource utilization
