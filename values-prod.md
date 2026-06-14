````markdown id="helm-values-prod-01"
# Production Values File (values-prod.yaml)

This file contains production-specific configuration values for the application.

Helm uses values files to separate environment-specific settings from Kubernetes manifests. Instead of hardcoding configuration inside Deployment manifests, values are injected during deployment.

This approach allows the same Helm chart to be reused across:

- Development (dev)
- Quality Assurance (qa)
- User Acceptance Testing (uat)
- Production (prod)

by simply changing the values file.

---

# Complete Configuration

```yaml
configMap:

  # Cart service endpoint
  CART_ENDPOINT: "cart:8080"

  # Database service hostname
  DB_HOST: "mysql"
```

---

# What is values-prod.yaml?

`values-prod.yaml` contains production environment settings.

Example deployment:

```bash
helm upgrade --install cart \
-f values-prod.yaml .
```

Helm reads:

```text
Chart Templates
        +
values-prod.yaml
        ↓
Final Kubernetes Manifest
```

---

# Architecture Overview

```text
values-prod.yaml
        │
        ▼
Helm Chart
        │
        ▼
ConfigMap Template
        │
        ▼
Kubernetes ConfigMap
        │
        ▼
Application Container
```

---

# ConfigMap Section

```yaml
configMap:
```

This section contains application configuration values.

These values are typically injected into:

```yaml
env:
envFrom:
```

inside Kubernetes Deployments.

Example:

```yaml
envFrom:
- configMapRef:
    name: cart
```

---

# CART_ENDPOINT

```yaml
CART_ENDPOINT: "cart:8080"
```

Defines the Cart service endpoint.

Meaning:

```text
Service Name : cart
Port         : 8080
```

Kubernetes DNS automatically resolves:

```text
cart
```

to:

```text
cart.roboshop.svc.cluster.local
```

---

# Why Use Service Names?

Instead of:

```yaml
CART_ENDPOINT: "10.10.10.5:8080"
```

use:

```yaml
CART_ENDPOINT: "cart:8080"
```

Benefits:

✅ Service Discovery

✅ No Hardcoded IPs

✅ Pod Replacement Safety

✅ Easier Scaling

---

# Request Flow

```text
Application
     │
     ▼
cart:8080
     │
     ▼
Kubernetes Service
     │
     ▼
Cart Pods
```

---

# DB_HOST

```yaml
DB_HOST: "mysql"
```

Defines the database hostname.

Kubernetes resolves:

```text
mysql
```

to:

```text
mysql.roboshop.svc.cluster.local
```

---

# Database Connection Flow

```text
Application
      │
      ▼
mysql
      │
      ▼
MySQL Service
      │
      ▼
MySQL Pod
```

---

# Why Use Service Names for Databases?

Instead of:

```yaml
DB_HOST: "192.168.10.20"
```

use:

```yaml
DB_HOST: "mysql"
```

Benefits:

```text
Dynamic Service Discovery
No IP Management
Automatic Failover Support
Cloud Native Design
```

---

# Helm Template Example

A ConfigMap template might look like:

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: application

data:

  CART_ENDPOINT: "{{ .Values.configMap.CART_ENDPOINT }}"

  DB_HOST: "{{ .Values.configMap.DB_HOST }}"
```

---

# Rendered Output

After Helm deployment:

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: application

data:

  CART_ENDPOINT: "cart:8080"

  DB_HOST: "mysql"
```

---

# Environment-Specific Values

## Development

```yaml
configMap:

  CART_ENDPOINT: "cart-dev:8080"

  DB_HOST: "mysql-dev"
```

---

## QA

```yaml
configMap:

  CART_ENDPOINT: "cart-qa:8080"

  DB_HOST: "mysql-qa"
```

---

## Production

```yaml
configMap:

  CART_ENDPOINT: "cart:8080"

  DB_HOST: "mysql"
```

---

# Configuration Flow

```text
values-prod.yaml
        │
        ▼
Helm
        │
        ▼
ConfigMap
        │
        ▼
Environment Variables
        │
        ▼
Application
```

---

# Real DevOps Use Cases

## Microservices Communication

Applications communicate using:

```text
Service Names
```

instead of:

```text
Static IP Addresses
```

---

## Multi-Environment Deployments

Different values files for:

```text
DEV
QA
UAT
PROD
```

while using the same Helm chart.

---

## GitOps

Tools like:

- :contentReference[oaicite:0]{index=0}

deploy environment-specific values automatically.

---

# Best Practices

✅ Store environment-specific settings in values files

✅ Use Kubernetes Service Names

✅ Avoid hardcoded IP addresses

✅ Separate production and non-production values

✅ Keep templates generic and reusable

---

# Benefits

- Environment Separation
- Reusable Helm Charts
- Easier Configuration Management
- Cloud Native Service Discovery
- GitOps Friendly
- Production Ready

---

# Why values-prod.yaml Is Important

The values file allows the same Helm chart to be deployed across multiple environments without modifying Kubernetes manifests.

It provides:

- Configuration Flexibility
- Environment Isolation
- Simplified Deployments
- Better Maintainability

This is a fundamental concept in:

- Helm
- Kubernetes
- GitOps
- DevOps Automation
- Platform Engineering
````
