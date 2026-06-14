# Frontend Deployment Values File

This Helm values file contains deployment-specific configuration for the Frontend application.

The values are injected into Kubernetes manifests during Helm deployment, allowing the same chart to be reused across multiple environments without modifying the templates.

This file defines:

- Docker Image Repository
- Docker Image Version
- Kubernetes Service Port

---

# Complete Configuration

```yaml
deployment:

  # Docker image repository
  imageRepo: 160885265516.dkr.ecr.us-east-1.amazonaws.com/roboshop/frontend

  # Docker image tag/version
  imageVersion: IMAGE_VERSION

service:

  # Kubernetes Service Port
  servicePort: 80
```

---

# Architecture Overview

```text
values.yaml
     │
     ▼
Helm Chart
     │
     ▼
Deployment Template
     │
     ▼
Kubernetes Deployment
     │
     ▼
Frontend Pod
```

---

# Deployment Section

```yaml
deployment:
```

Contains container deployment configuration.

This section is commonly referenced inside:

```yaml
deployment.yaml
```

Helm template.

Example:

```yaml
image: "{{ .Values.deployment.imageRepo }}:{{ .Values.deployment.imageVersion }}"
```

---

# Image Repository

```yaml
imageRepo: 160885265516.dkr.ecr.us-east-1.amazonaws.com/roboshop/frontend
```

Defines the Docker image repository.

Repository Structure:

```text
AWS Account ID
        │
        ▼
160885265516
        │
        ▼
Amazon ECR
        │
        ▼
roboshop/frontend
```

Complete Repository:

```text
160885265516.dkr.ecr.us-east-1.amazonaws.com/roboshop/frontend
```

---

# Amazon ECR

The image is stored in:

- :contentReference[oaicite:0]{index=0}

Purpose:

```text
Store Docker Images
Version Control
Container Distribution
```

---

# Image Version

```yaml
imageVersion: IMAGE_VERSION
```

Represents the Docker image tag.

Usually replaced dynamically during deployment.

Example:

```text
v1
```

or

```text
a1b2c3d4e
```

or

```text
latest
```

---

# Example Image

Repository:

```text
160885265516.dkr.ecr.us-east-1.amazonaws.com/roboshop/frontend
```

Version:

```text
a1b2c3d4e
```

Final Image:

```text
160885265516.dkr.ecr.us-east-1.amazonaws.com/roboshop/frontend:a1b2c3d4e
```

---

# CI/CD Integration

Most CI/CD pipelines replace:

```yaml
IMAGE_VERSION
```

during deployment.

Example:

```bash
helm upgrade --install frontend \
  --set deployment.imageVersion=${APP_VERSION}
```

Result:

```yaml
imageVersion: a1b2c3d4e
```

---

# Why Dynamic Image Tags?

Instead of:

```yaml
imageVersion: latest
```

use:

```yaml
imageVersion: a1b2c3d4e
```

Benefits:

✅ Immutable Deployments

✅ Easy Rollbacks

✅ Traceability

✅ Better Release Management

---

# Service Section

```yaml
service:
```

Contains Kubernetes Service configuration.

---

# Service Port

```yaml
servicePort: 80
```

Defines the port exposed by the Kubernetes Service.

Traffic Flow:

```text
User
  │
  ▼
Load Balancer
  │
  ▼
Service:80
  │
  ▼
Pod:8080
```

---

# Service Port vs Container Port

## Service Port

```yaml
servicePort: 80
```

External cluster-facing port.

Used by:

```text
Services
Ingress
Load Balancers
Target Groups
```

---

## Container Port

Inside Deployment:

```yaml
containerPort: 8080
```

Port used by Nginx inside the container.

---

# Example Service Template

```yaml
ports:

- protocol: TCP

  port: {{ .Values.service.servicePort }}

  targetPort: 8080
```

---

# Rendered Service

After Helm renders:

```yaml
ports:

- protocol: TCP

  port: 80

  targetPort: 8080
```

Meaning:

```text
Service receives traffic on 80
        │
        ▼
Forwards to container on 8080
```

---

# Configuration Flow

```text
values.yaml
      │
      ▼
Helm
      │
      ▼
Deployment Template
      │
      ▼
Container Image
```

```text
values.yaml
      │
      ▼
Helm
      │
      ▼
Service Template
      │
      ▼
Service Port
```

---

# Example Deployment Command

```bash
helm upgrade --install frontend \
  -f values-prod.yaml \
  --set deployment.imageVersion=a1b2c3d4e .
```

Helm generates:

```yaml
image:
  160885265516.dkr.ecr.us-east-1.amazonaws.com/roboshop/frontend:a1b2c3d4e
```

---

# Real DevOps Use Cases

## CI/CD Pipelines

Automatically deploy new image versions.

Example:

```text
Build Image
      │
      ▼
Push To ECR
      │
      ▼
Update Helm Value
      │
      ▼
Deploy To Kubernetes
```

---

## GitOps

Tools like:

- :contentReference[oaicite:1]{index=1}

track image version changes and deploy automatically.

---

## Multi-Environment Deployments

Same chart:

```text
DEV
QA
UAT
PROD
```

Different image versions.

---

# Best Practices

✅ Store image details in values files

✅ Use immutable image tags

✅ Avoid using `latest`

✅ Separate service configuration from templates

✅ Use CI/CD to update image versions

---

# Benefits

- Reusable Helm Charts
- Environment Flexibility
- Easy Version Management
- GitOps Friendly
- CI/CD Integration
- Simplified Deployments

---

# Why This Configuration Is Important

This values file controls two critical aspects of deployment:

1. Which container image gets deployed
2. Which port the application is exposed on

By externalizing these values, Helm enables:

- Consistent Deployments
- Versioned Releases
- Environment-Specific Configuration
- Automated CI/CD Workflows

These concepts are fundamental for:

- Kubernetes Deployments
- Helm Charts
- DevOps Automation
- Platform Engineering
- Cloud-Native Applications
````
