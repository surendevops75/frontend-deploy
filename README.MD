# 🚀 RoboShop Frontend Deployment

![Helm](https://img.shields.io/badge/Package_Manager-Helm-blue)
![Kubernetes](https://img.shields.io/badge/Platform-Kubernetes-green)
![Jenkins](https://img.shields.io/badge/CI%2FCD-Jenkins-red)
![DevOps](https://img.shields.io/badge/Automation-DevOps-orange)

## 📌 Overview

This repository contains deployment configurations for the RoboShop Frontend service using Helm Charts, Kubernetes, and Jenkins CI/CD pipelines.

The project demonstrates how frontend applications can be deployed consistently across multiple environments using Infrastructure as Code and automated deployment workflows.

---

## 🏗️ Architecture

```text
Developer
    │
    ▼
 GitHub Repository
    │
    ▼
 Jenkins Pipeline
    │
    ▼
 Helm Chart
    │
    ▼
 Kubernetes Cluster
    │
    ▼
 Frontend Service
```

---

## 📂 Repository Structure

```text
.
├── templates/
│   ├── manifest.yaml
│   └── frontend-manifest.md
│
├── Chart.yaml
├── Chart-yaml.md
│
├── Jenkinsfile
├── Jenkinsfile-frontend.MD
│
├── values.yaml
├── values.md
│
├── values-dev.yaml
├── values-dev.md
│
├── values-prod.yaml
└── values-prod.md
```

---

## 📁 Components

### templates/

Contains Kubernetes resource templates used by Helm.

Resources include:

- Deployment
- Service
- ConfigMaps
- Ingress
- Horizontal Pod Autoscaler

---

### manifest.yaml

Main Kubernetes manifest template.

Responsibilities:

- Application deployment
- Service configuration
- Resource definitions
- Environment integration

---

### Chart.yaml

Defines Helm chart metadata.

Includes:

- Chart name
- Version
- Description
- Application version

---

### values.yaml

Default configuration values used by Helm.

Contains:

- Replica count
- Image information
- Resource limits
- Environment variables

---

### values-dev.yaml

Development environment configuration.

Examples:

- Lower resource allocation
- Development image tags
- Testing configurations

---

### values-prod.yaml

Production environment configuration.

Examples:

- High availability settings
- Production image tags
- Optimized resource allocation

---

### Jenkinsfile

CI/CD pipeline definition.

Responsibilities:

- Source code checkout
- Deployment validation
- Helm chart deployment
- Environment-specific releases

---

## 🔄 Deployment Workflow

```text
Code Commit
     │
     ▼
GitHub Repository
     │
     ▼
Jenkins Pipeline
     │
     ▼
Helm Chart Validation
     │
     ▼
Helm Deployment
     │
     ▼
Kubernetes Cluster
     │
     ▼
Frontend Application
```

---

## 🚀 Helm Deployment

Deploy using default values:

```bash
helm install frontend .
```

Deploy using development values:

```bash
helm install frontend . -f values-dev.yaml
```

Deploy using production values:

```bash
helm install frontend . -f values-prod.yaml
```

Upgrade deployment:

```bash
helm upgrade frontend . -f values-prod.yaml
```

---

## 🛠 Technologies Used

- Helm
- Kubernetes
- Jenkins
- YAML
- CI/CD
- RoboShop Microservices

---

## 📚 Learning Objectives

This repository helps you understand:

- Helm Chart Development
- Kubernetes Deployments
- Environment Management
- Jenkins Pipelines
- CI/CD Automation
- Infrastructure as Code
- DevOps Best Practices

---

## 👨‍💻 Ideal For

- DevOps Engineers
- Platform Engineers
- Cloud Engineers
- Kubernetes Administrators
- Site Reliability Engineers (SREs)

---

## 🔒 DevOps Practices Demonstrated

- Infrastructure as Code
- Automated Deployments
- Environment Separation
- Configuration Management
- CI/CD Automation
- Kubernetes-native Deployments

---

## ⭐ Support

If you find this repository useful:

- Star the repository ⭐
- Fork the project 🍴
- Share it with fellow DevOps Engineers 🚀

---

## 📖 Related Technologies

Helm • Kubernetes • Jenkins • CI/CD • DevOps • Deployment Automation • Cloud Native • Infrastructure as Code