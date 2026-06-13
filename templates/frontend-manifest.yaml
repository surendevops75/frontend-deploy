# Frontend Deployment on Kubernetes (ConfigMap + Deployment + Service + TargetGroupBinding)

This Kubernetes manifest deploys the Frontend component of the Roboshop application.

The deployment consists of:

- ConfigMap for Nginx Configuration
- Deployment for Frontend Pods
- Service for Internal Communication
- AWS TargetGroupBinding for External Access

This architecture follows a common production pattern where:

- Nginx serves static frontend content
- Nginx routes API requests to backend microservices
- Kubernetes manages pod lifecycle
- AWS Load Balancer forwards traffic to frontend pods

---

# Architecture Overview

```text
Users
   │
   ▼
AWS Application Load Balancer
   │
   ▼
Target Group
   │
   ▼
TargetGroupBinding
   │
   ▼
Frontend Service
   │
   ▼
Frontend Pods
   │
   ▼
Nginx
   │
   ├────────► Catalogue Service
   │
   ├────────► User Service
   │
   ├────────► Cart Service
   │
   ├────────► Shipping Service
   │
   └────────► Payment Service
```

---

# Kubernetes Resources

This manifest creates:

```text
ConfigMap
Deployment
Service
TargetGroupBinding
```

---

# ConfigMap

```yaml
kind: ConfigMap
```

The ConfigMap stores the Nginx configuration.

Purpose:

```text
Externalize Configuration
Avoid Rebuilding Images
Easy Updates
```

Instead of baking nginx.conf into the Docker image:

```text
Docker Image
      │
      ▼
Mount ConfigMap
      │
      ▼
nginx.conf
```

---

# ConfigMap Name

```yaml
name: frontend
```

This ConfigMap is later mounted into the container.

---

# Nginx Configuration

Stored inside:

```yaml
data:
  nginx.conf
```

Purpose:

```text
Serve Frontend Application
Reverse Proxy Backend APIs
Expose Health Endpoint
```

---

# Listening Port

```nginx
listen 8080;
```

Nginx listens on:

```text
8080
```

inside the container.

Why not port 80?

Because Kubernetes Services and Load Balancers can expose any external port while containers run on different internal ports.

---

# Reverse Proxy Configuration

Nginx forwards API requests to backend services.

---

## Catalogue Service

```nginx
location /api/catalogue/
```

Routes requests to:

```text
catalogue:8080
```

Example:

```text
/api/catalogue/products
```

---

## User Service

```nginx
location /api/user/
```

Routes requests to:

```text
user:8080
```

Handles:

```text
Login
Registration
Authentication
```

---

## Cart Service

```nginx
location /api/cart/
```

Routes requests to:

```text
cart:8080
```

Handles:

```text
Shopping Cart Operations
```

---

## Shipping Service

```nginx
location /api/shipping/
```

Routes requests to:

```text
shipping:8080
```

Handles:

```text
Shipping Cost Calculation
Address Validation
```

---

## Payment Service

```nginx
location /api/payment/
```

Routes requests to:

```text
payment:8080
```

Handles:

```text
Payment Processing
Transaction Validation
```

---

# Image Handling

```nginx
location /images/
```

Provides image caching.

---

## Cache Duration

```nginx
expires 5s;
```

Browser caches images for:

```text
5 Seconds
```

Benefits:

```text
Reduced Requests
Improved Performance
Lower Latency
```

---

## Placeholder Images

```nginx
try_files $uri /images/placeholder.jpg;
```

If image is missing:

```text
Requested Image
      │
      ▼
Not Found
      │
      ▼
placeholder.jpg
```

---

# Health Endpoint

```nginx
location /health
```

Uses:

```nginx
stub_status on;
```

Provides Nginx statistics.

Useful for:

```text
Kubernetes Probes
Monitoring
Load Balancer Health Checks
```

---

# Deployment

```yaml
kind: Deployment
```

Deployment manages frontend pods.

Responsibilities:

```text
Pod Creation
Rolling Updates
Self Healing
Scaling
```

---

# Deployment Name

```yaml
name: frontend
```

Creates:

```text
frontend ReplicaSet
frontend Pods
```

---

# Labels

```yaml
project: roboshop
component: frontend
tier: frontend
```

Used for:

```text
Pod Identification
Service Discovery
Monitoring
Traffic Routing
```

---

# Replica Count

```yaml
replicas: 1
```

Current state:

```text
1 Pod
```

Production example:

```yaml
replicas: 3
```

Benefits:

```text
High Availability
Load Distribution
```

---

# Selector

```yaml
matchLabels
```

Connects Deployment to Pods.

Kubernetes uses these labels to identify managed pods.

---

# Container

```yaml
containers:
```

Defines the frontend container.

---

# Dynamic Image Tagging

```yaml
image: "{{ .Values.deployment.imageRepo }}:{{ .Values.deployment.imageVersion }}"
```

This is a Helm template.

Example:

```text
Image Repository:
160885265516.dkr.ecr.us-east-1.amazonaws.com/roboshop/frontend

Version:
a1b2c3d4e
```

Final Image:

```text
160885265516.dkr.ecr.us-east-1.amazonaws.com/roboshop/frontend:a1b2c3d4e
```

Benefits:

```text
Versioned Deployments
Easy Rollbacks
GitOps Friendly
```

---

# Image Pull Policy

```yaml
imagePullPolicy: Always
```

Kubernetes always pulls the latest image.

Useful for:

```text
Frequent Deployments
CI/CD Pipelines
```

---

# Container Port

```yaml
containerPort: 8080
```

Port exposed inside the container.

Flow:

```text
Service
   │
   ▼
Pod
   │
   ▼
Nginx:8080
```

---

# Readiness Probe

```yaml
readinessProbe
```

Checks:

```text
Can this pod receive traffic?
```

Endpoint:

```text
/health
```

If probe fails:

```text
Pod Removed From Service
```

---

# Liveness Probe

```yaml
livenessProbe
```

Checks:

```text
Is the application alive?
```

If probe fails:

```text
Pod Restarted
```

Benefits:

```text
Self Healing
Automatic Recovery
```

---

# ConfigMap Volume Mount

```yaml
volumeMounts
```

Mounts nginx.conf into:

```text
/etc/nginx/nginx.conf
```

Flow:

```text
ConfigMap
     │
     ▼
Volume
     │
     ▼
Container
```

Benefits:

```text
Dynamic Configuration
No Image Rebuilds
```

---

# Service

```yaml
kind: Service
```

Provides a stable endpoint for frontend pods.

---

# Service Name

```yaml
name: frontend
```

Accessible inside cluster as:

```text
frontend
```

or

```text
frontend.roboshop.svc.cluster.local
```

---

# Service Selector

```yaml
selector
```

Selects frontend pods using labels.

Flow:

```text
Service
     │
     ▼
Matching Labels
     │
     ▼
Frontend Pods
```

---

# Service Port

```yaml
port: {{ .Values.service.servicePort }}
```

Configured dynamically using Helm values.

Example:

```yaml
servicePort: 80
```

---

# Target Port

```yaml
targetPort: 8080
```

Traffic forwarded to:

```text
Nginx Container Port
```

---

# TargetGroupBinding

```yaml
kind: TargetGroupBinding
```

AWS Load Balancer Controller resource.

Purpose:

```text
Connect Existing AWS Target Group
With Kubernetes Service
```

---

# Service Reference

```yaml
serviceRef:
  name: frontend
```

Traffic from Target Group is routed to:

```text
frontend Service
```

---

# Target Group ARN

```yaml
targetGroupARN
```

References existing AWS Target Group.

Example:

```text
arn:aws:elasticloadbalancing:...
```

---

# Target Type

```yaml
targetType: ip
```

Registers:

```text
Pod IPs
```

instead of:

```text
Node IPs
```

Benefits:

```text
Direct Pod Routing
Better Performance
Reduced Hops
```

---

# Complete Request Flow

```text
User
  │
  ▼
Application Load Balancer
  │
  ▼
Target Group
  │
  ▼
TargetGroupBinding
  │
  ▼
Frontend Service
  │
  ▼
Frontend Pod
  │
  ▼
Nginx
  │
  ▼
Backend Microservice
```

---

# Real DevOps Use Cases

## E-Commerce Platforms

Frontend routes traffic to:

```text
Catalogue
User
Cart
Shipping
Payment
```

---

## Microservices Architecture

Single frontend gateway for multiple backend services.

---

## Kubernetes Production Deployments

Supports:

```text
Rolling Updates
Health Checks
Self Healing
Autoscaling
```

---

# Best Practices

✅ Store Nginx configuration in ConfigMap

✅ Use Readiness and Liveness Probes

✅ Use Helm templating

✅ Route traffic through Services

✅ Use TargetGroupBinding for ALB integration

✅ Keep frontend and backend separated

---

# Benefits

- Scalable Architecture
- Cloud Native Design
- Easy Configuration Management
- Production-Ready Deployments
- AWS Load Balancer Integration
- Microservice-Friendly Routing

---

# Why This Configuration Is Important

This deployment demonstrates several advanced Kubernetes concepts:

- ConfigMaps
- Deployments
- Services
- Health Checks
- Helm Templates
- AWS TargetGroupBinding
- Nginx Reverse Proxy
- Microservice Routing

These concepts are commonly used in:

- DevOps Engineering
- Platform Engineering
- Kubernetes Operations
- Cloud-Native Applications
- Production EKS Clusters