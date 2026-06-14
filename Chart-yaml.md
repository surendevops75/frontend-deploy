````markdown
# Helm Chart Metadata (Chart.yaml)

This file is the metadata definition for a Helm chart.

Every Helm chart must contain a `Chart.yaml` file because it provides essential information about the chart such as:

- Chart Name
- Chart Version
- Application Version
- Description
- Dependencies
- Maintainer Information

Helm uses this file to identify, package, install, and manage applications deployed to Kubernetes.

---

# Complete Manifest

```yaml
apiVersion: v2

# Helm chart name
name: frontend

# Helm chart version
version: 1.0.0

# Chart description
description: frontend installation

# Application version
appVersion: v1
```

---

# What is Chart.yaml?

`Chart.yaml` is the main metadata file of a Helm chart.

Think of it as:

```text
package.json  → NodeJS
pom.xml       → Maven
requirements.txt → Python

Chart.yaml    → Helm
```

Without this file:

```text
helm install
helm package
helm dependency
```

will not work.

---

# Architecture Overview

```text
Helm Chart
    │
    ├── Chart.yaml
    ├── values.yaml
    ├── templates/
    └── charts/
            │
            ▼
      Kubernetes Resources
```

---

# API Version

```yaml
apiVersion: v2
```

Defines the Helm chart specification version.

Current versions:

```text
v1 → Helm 2
v2 → Helm 3
```

Since Helm 3 is the current standard:

```yaml
apiVersion: v2
```

should always be used.

Benefits:

- Dependency management improvements
- Better packaging
- Helm 3 compatibility

---

# Chart Name

```yaml
name: frontend
```

Specifies the chart name.

Example:

```text
frontend
```

This name is used during:

```bash
helm install frontend .
```

and

```bash
helm package .
```

Generated package:

```text
frontend-1.0.0.tgz
```

---

# Chart Version

```yaml
version: 1.0.0
```

Represents the Helm chart version.

This version tracks changes to:

```text
Templates
Helm Logic
Values
Chart Structure
```

Format:

```text
MAJOR.MINOR.PATCH
```

Examples:

```text
1.0.0
1.0.1
1.1.0
2.0.0
```

---

# When to Update Chart Version?

Update when:

```text
Template Changes
Value Changes
Chart Logic Changes
Dependency Changes
```

Example:

```text
Added Ingress
Added ConfigMap
Modified Deployment
```

Version change:

```text
1.0.0 → 1.0.1
```

---

# Description

```yaml
description: frontend installation
```

Provides a human-readable description of the chart.

Purpose:

```text
Documentation
Repository Browsing
Chart Discovery
```

Example:

```yaml
description: Roboshop Frontend Helm Chart
```

---

# Application Version

```yaml
appVersion: v1
```

Represents the application version deployed by the chart.

This is different from:

```yaml
version
```

---

# Difference Between version and appVersion

## Chart Version

```yaml
version: 1.0.0
```

Tracks:

```text
Helm Chart Changes
```

Examples:

```text
Deployment Template
Service Template
ConfigMap Template
```

---

## Application Version

```yaml
appVersion: v1
```

Tracks:

```text
Application Release Version
```

Examples:

```text
frontend:v1
frontend:v2
frontend:v3
```

---

# Example Scenario

Application upgraded:

```text
frontend:v1
      ↓
frontend:v2
```

Chart remains same:

```yaml
version: 1.0.0
```

Only:

```yaml
appVersion: v2
```

changes.

---

Another scenario:

Application remains same:

```text
frontend:v2
```

But deployment template modified.

Example:

```text
Added Readiness Probe
Added ConfigMap
Added Resources
```

Then:

```yaml
version: 1.1.0
```

changes while:

```yaml
appVersion: v2
```

remains unchanged.

---

# Typical Helm Chart Structure

```text
frontend/
│
├── Chart.yaml
├── values.yaml
│
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   └── ingress.yaml
│
└── charts/
```

---

# How Helm Uses Chart.yaml

## Install

```bash
helm install frontend .
```

Helm reads:

```text
Chart.yaml
values.yaml
templates/
```

and generates Kubernetes manifests.

---

## Package

```bash
helm package .
```

Creates:

```text
frontend-1.0.0.tgz
```

---

## Repository

```bash
helm repo add
```

Charts are identified using:

```text
name
version
```

from Chart.yaml.

---

# Real DevOps Use Cases

## Kubernetes Application Packaging

Package:

```text
Frontend
Backend
Monitoring
Logging
```

applications as reusable charts.

---

## GitOps

Tools like:

- :contentReference[oaicite:0]{index=0}

track Helm chart versions and deploy applications automatically.

---

## CI/CD Pipelines

Build process:

```text
Code
  │
  ▼
Docker Image
  │
  ▼
Helm Chart
  │
  ▼
Kubernetes Deployment
```

---

# Best Practices

✅ Use `apiVersion: v2`

✅ Follow Semantic Versioning

✅ Keep chart version and application version separate

✅ Use meaningful descriptions

✅ Version charts consistently

✅ Store all Kubernetes templates under `templates/`

---

# Benefits

- Standardized Kubernetes Packaging
- Easy Version Management
- Reusable Deployments
- GitOps Friendly
- CI/CD Integration
- Environment Consistency

---

# Why Chart.yaml Is Important

`Chart.yaml` is the identity card of a Helm chart.

It defines:

- Chart Metadata
- Version Information
- Application Version
- Packaging Information

Understanding Chart.yaml is essential for:

- DevOps Engineers
- Kubernetes Administrators
- Platform Engineers
- Cloud Engineers
- GitOps Implementations
````
