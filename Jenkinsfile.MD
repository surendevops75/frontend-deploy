````markdown id="jenkins-eksdeploy-01"
# Jenkins Shared Library Deployment Pipeline

This Jenkins Pipeline demonstrates how to deploy applications to Kubernetes/EKS using a Jenkins Shared Library.

Instead of writing deployment logic directly in the Jenkinsfile, the deployment process is centralized inside a reusable shared library function called:

```groovy
EKSDeploy()
```

This approach allows multiple applications to reuse the same deployment process while only passing application-specific parameters.

---

# Complete Jenkinsfile

```groovy
// --------------------------------------------------
// Load Jenkins Shared Library
// --------------------------------------------------

@Library('jenkins-shared-library') _

// --------------------------------------------------
// Define Build Parameters
// --------------------------------------------------

properties([

  parameters([

    // Application version to deploy
    string(
      name: 'appVersion',
      description: 'Enter Application version'
    ),

    // Deployment environment
    choice(
      name: 'deploy_to',
      choices: ['dev', 'qa', 'prod'],
      description: 'Target environment'
    )

  ])
])

// --------------------------------------------------
// Build Configuration Map
// --------------------------------------------------

def configMap = [

  // Project Name
  project : "roboshop",

  // Application Component
  component : "frontend",

  // Environment Selection
  deploy_to : (params.deploy_to ?: 'dev'),

  // Image Version
  appVersion : (params.appVersion)
]

// --------------------------------------------------
// Print Deployment Information
// --------------------------------------------------

echo "Going to execute Jenkins shared library"

echo "ConfigMap: ${configMap}"

// --------------------------------------------------
// Execute Deployment Function
// --------------------------------------------------

EKSDeploy(configMap)
```

---

# Pipeline Flow

```text
User Starts Build
         │
         ▼
Select Environment
(dev / qa / prod)
         │
         ▼
Provide Application Version
         │
         ▼
Build ConfigMap
         │
         ▼
Load Shared Library
         │
         ▼
Call EKSDeploy()
         │
         ▼
Deploy Application to EKS
```

---

# Shared Library Import

```groovy
@Library('jenkins-shared-library') _
```

Loads the Jenkins Shared Library.

Purpose:

```text
Reuse Deployment Logic
Centralize CI/CD
Reduce Code Duplication
```

Example Shared Library Structure:

```text
jenkins-shared-library
│
├── vars
│   ├── EKSDeploy.groovy
│   ├── nodeJSEKSPipeline.groovy
│   └── helmDeploy.groovy
│
├── src
└── resources
```

---

# Build Parameters

The pipeline accepts user input before execution.

---

## Application Version

```groovy
string(
  name: 'appVersion'
)
```

Purpose:

```text
Specify Docker Image Version
```

Example:

```text
v1
v2
a1b2c3d4e
latest
```

Build Screen:

```text
Application Version:
[ v1 ]
```

---

## Deployment Environment

```groovy
choice(
  name: 'deploy_to'
)
```

Options:

```text
dev
qa
prod
```

Purpose:

```text
Select Target Kubernetes Environment
```

Example:

```text
Deploy To:
▼ dev
▼ qa
▼ prod
```

---

# ConfigMap

```groovy
def configMap = [...]
```

Creates a configuration object that is passed to the shared library.

Equivalent JSON:

```json
{
  "project": "roboshop",
  "component": "frontend",
  "deploy_to": "dev",
  "appVersion": "v1"
}
```

---

# Project

```groovy
project : "roboshop"
```

Represents the application project.

Used for:

```text
Namespace Selection
Helm Release Naming
Image Repository Structure
```

---

# Component

```groovy
component : "frontend"
```

Represents the application being deployed.

Examples:

```text
frontend
catalogue
cart
user
shipping
payment
```

---

# Environment

```groovy
deploy_to
```

Determines deployment target.

Example:

```text
dev
```

The shared library may use this value to:

```text
Select Cluster
Select Namespace
Select Values File
```

Example:

```text
values-dev.yaml
values-qa.yaml
values-prod.yaml
```

---

# Safe Default Value

```groovy
(params.deploy_to ?: 'dev')
```

Meaning:

```text
If deploy_to is empty
use dev
```

Example:

```text
User Input Missing
        │
        ▼
Default → dev
```

---

# Application Version

```groovy
appVersion
```

Usually corresponds to:

```text
Docker Image Tag
Git SHA
Release Version
```

Example:

```text
frontend:v1
frontend:v2
frontend:a1b2c3d4e
```

---

# Logging Deployment Details

```groovy
echo "ConfigMap: ${configMap}"
```

Example Output:

```text
ConfigMap:
[
 project:roboshop,
 component:frontend,
 deploy_to:dev,
 appVersion:v1
]
```

Benefits:

```text
Audit Trail
Troubleshooting
Debugging
```

---

# EKSDeploy Function

```groovy
EKSDeploy(configMap)
```

Calls a reusable deployment function from the Shared Library.

Actual implementation resides in:

```text
vars/EKSDeploy.groovy
```

---

# Typical EKSDeploy Workflow

The shared library usually performs:

```text
Authenticate to AWS
        │
        ▼
Connect to EKS Cluster
        │
        ▼
Update kubeconfig
        │
        ▼
Run Helm Upgrade
        │
        ▼
Verify Deployment
```

---

# Example Internal Flow

```groovy
aws eks update-kubeconfig

helm upgrade --install frontend

kubectl rollout status deployment/frontend
```

---

# Deployment Architecture

```text
Jenkins
   │
   ▼
Shared Library
   │
   ▼
EKSDeploy()
   │
   ▼
AWS EKS
   │
   ▼
Helm
   │
   ▼
Kubernetes Resources
```

---

# Example Deployment

User Inputs:

```text
appVersion = v2
deploy_to = qa
```

Generated ConfigMap:

```text
project    = roboshop
component  = frontend
deploy_to  = qa
appVersion = v2
```

Deployment:

```text
frontend:v2
        │
        ▼
QA Environment
```

---

# Real DevOps Use Cases

## Multi-Environment Deployments

```text
DEV
QA
UAT
PROD
```

using the same deployment code.

---

## Microservices

Reusable deployment for:

```text
frontend
catalogue
cart
user
shipping
payment
```

---

## GitOps-Friendly Deployments

Shared library updates Helm values and triggers deployments.

---

## Platform Engineering

Provides standardized deployment workflows across teams.

---

# Best Practices

✅ Use Shared Libraries

✅ Parameterize Deployments

✅ Keep Jenkinsfiles Lightweight

✅ Reuse Deployment Logic

✅ Support Multiple Environments

✅ Use Helm for Kubernetes Deployments

---

# Benefits

- Reusable Deployment Process
- Centralized Management
- Reduced Code Duplication
- Faster Onboarding
- Standardized EKS Deployments
- Easier Maintenance

---

# Why This Pipeline Is Important

This Jenkinsfile demonstrates enterprise-grade deployment practices:

- Jenkins Shared Libraries
- Parameterized Pipelines
- Kubernetes Deployments
- Amazon EKS Integration
- Reusable CI/CD Design

These concepts are widely used in:

- DevOps Engineering
- Platform Engineering
- Cloud-Native Applications
- Kubernetes Operations
- Enterprise CI/CD Platforms
````
