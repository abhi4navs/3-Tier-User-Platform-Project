# 3-Tier-User-Platform-Project

# 🚀 Cloud-Native 3-Tier Application on AWS EKS

<p align="center">
  <img src="https://img.shields.io/badge/AWS-EKS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white"/>
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white"/>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Amazon_ECR-FF9900?style=for-the-badge&logo=amazonecr&logoColor=white"/>
  <img src="https://img.shields.io/badge/Route_53-8C4FFF?style=for-the-badge&logo=amazonroute53&logoColor=white"/>
  <img src="https://img.shields.io/badge/ACM-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white"/>
  <img src="https://img.shields.io/badge/SonarQube-4E9BCD?style=for-the-badge&logo=sonarqube&logoColor=white"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Trivy-1904DA?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Checkov-7B61FF?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Gitleaks-000000?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white"/>
</p>

<p align="center">
  <b>Production-oriented Cloud-Native 3-Tier Application with an automated DevSecOps delivery pipeline.</b>
</p>

---

## 📖 Overview

This project demonstrates a production-oriented **3-Tier Cloud-Native Application** deployed on **Amazon Elastic Kubernetes Service (EKS)**.

The complete platform covers:

- Containerized application workloads
- Kubernetes orchestration
- Automated CI/CD with GitHub Actions
- Shift-left DevSecOps security
- Static code analysis
- Container vulnerability scanning
- Infrastructure security scanning
- Secret detection
- Private container registry
- AWS Application Load Balancer
- HTTPS with AWS Certificate Manager
- DNS management using Route 53
- Secure AWS authentication using GitHub OIDC
- Kubernetes persistent storage using Amazon EBS
- Secrets synchronization using AWS Secrets Manager

The goal of the project is to demonstrate how a modern application can move from **source code → security validation → containerization → QA → production deployment** using an automated delivery workflow.

---

# 🏗️ Architecture

The application follows a cloud-native 3-tier architecture running on Amazon EKS.

<p align="center">
  <img src="./docs/images/eks-architecture.png" alt="AWS EKS Full Stack Architecture" width="100%">
</p>

### High-Level Request Flow

```text
                         Internet User
                              │
                              ▼
                         Namecheap
                       Domain Registrar
                              │
                              ▼
                          Route 53
                         DNS Hosted Zone
                              │
                              ▼
                     AWS Application
                    Load Balancer (ALB)
                              │
                              ▼
                    Kubernetes Ingress
                              │
                              ▼
                    Application Service
                              │
                              ▼
                    ┌─────────────────┐
                    │   Application   │
                    │      Pods       │
                    └────────┬────────┘
                             │
                             │ SQL
                             ▼
                       MySQL StatefulSet
                             │
                             ▼
                        Amazon EBS
```

---

# 🌐 DNS & HTTPS Flow

The application is exposed through a custom domain with HTTPS.

```text
User Browser
     │
     │ HTTPS
     ▼
Namecheap
     │
     │ Nameserver delegation
     ▼
Route 53
     │
     │ DNS resolution
     ▼
AWS ALB
     │
     │ TLS termination
     ▼
Kubernetes Ingress
     │
     ▼
Application Service
     │
     ▼
Application Pods
```

### AWS Networking Components

| Component | Responsibility |
|---|---|
| **Namecheap** | Domain registration |
| **Route 53** | DNS management |
| **AWS ALB** | External traffic distribution |
| **AWS ACM** | SSL/TLS certificate |
| **AWS Load Balancer Controller** | Provisions and manages ALB |
| **Kubernetes Ingress** | Host/path-based routing |
| **Kubernetes Service** | Internal application networking |

---

# 🔄 DevSecOps CI/CD Pipeline

The project implements a multi-stage CI/CD pipeline using **GitHub Actions**.

<p align="center">
  <img src="./docs/images/cicd-pipeline.png" alt="DevSecOps CI/CD Pipeline" width="100%">
</p>

The pipeline follows a controlled flow from feature development to QA and finally production.

```text
Feature Branch
      │
      ▼
   QA Branch
      │
      ▼
 GitHub Actions
      │
      ├───────────────┐
      │               │
      ▼               ▼
  Gitleaks         Checkov
      │               │
      └───────┬───────┘
              ▼
      Security & Quality
           Checks
              │
              ▼
         SonarQube
              │
              ▼
       Application Build
              │
              ▼
         Docker Build
              │
        ┌─────┴─────┐
        ▼           ▼
      Trivy        SBOM
      Scan       Generation
        │           │
        └─────┬─────┘
              ▼
         Amazon ECR
              │
              ▼
      Update K8s Manifest
              │
              ▼
          EKS - QA
              │
              ▼
        Manual QA Testing
              │
        ┌─────┴─────┐
        ▼           ▼
     Bugfix       Sign-off
     Branch          │
        │             ▼
        └──────►  Main Branch
                     │
                     ▼
                Production CD
                     │
                     ▼
                  EKS Prod
```

---

# 🛡️ DevSecOps Security

Security is integrated directly into the CI/CD lifecycle.

Instead of deploying first and scanning later, security checks are executed **before deployment**.

## 🔐 Security Tools

| Tool | Purpose |
|---|---|
| **Gitleaks** | Detect leaked secrets and credentials |
| **Checkov** | Scan Terraform, Kubernetes and IaC configurations |
| **Trivy** | Filesystem and container image vulnerability scanning |
| **SonarQube** | Static code analysis and quality checks |
| **SBOM** | Software dependency/component visibility |

---

## 🔑 Gitleaks

Gitleaks scans the repository for accidentally committed secrets.

```text
Source Code
     │
     ▼
  Gitleaks
     │
 ┌───┴────┐
 ▼        ▼
Found    Clean
 │         │
 ❌        ✅
Fail     Continue
```

Detects potential:

- API keys
- Passwords
- Tokens
- Private keys
- Cloud credentials

---

## 🧱 Checkov

Checkov validates Infrastructure-as-Code and Kubernetes configuration against security best practices.

```text
Terraform
Kubernetes YAML
Docker Configuration
        │
        ▼
      Checkov
        │
        ▼
 Security Validation
```

---

## 🐳 Trivy

Trivy is used at multiple stages of the pipeline.

```text
Repository
     │
     ▼
Filesystem Scan
     │
     ▼
Docker Build
     │
     ▼
Container Image
     │
     ▼
Image Vulnerability Scan
     │
     ▼
Amazon ECR
```

---

## 📊 SonarQube

SonarQube analyzes application source code for:

- Bugs
- Vulnerabilities
- Code smells
- Maintainability issues
- Code quality problems

```text
Application Source
        │
        ▼
    SonarQube
        │
        ▼
   Quality Gate
        │
        ▼
 Build / Deployment
```

---

# ☸️ Kubernetes Architecture

The application is deployed using Kubernetes workloads inside Amazon EKS.

### Application Layer

```text
                Kubernetes Cluster
                       │
              ┌────────┴────────┐
              │                 │
          Frontend            Backend
           Pods                Pods
              │                 │
              └────────┬────────┘
                       │
                       ▼
                    MySQL
                  StatefulSet
                       │
                       ▼
                  Amazon EBS
```

### Kubernetes Components

- Deployment
- StatefulSet
- Service
- Ingress
- ConfigMap
- Secret
- PersistentVolumeClaim
- StorageClass
- Namespace

---

# 🔐 Secrets Management

Application secrets are separated from source code.

The project uses **AWS Secrets Manager** with the **External Secrets Operator** to synchronize secrets into Kubernetes.

```text
AWS Secrets Manager
        │
        │ IRSA
        ▼
External Secrets Operator
        │
        ▼
Kubernetes Secret
        │
        ▼
Application / MySQL Pods
```

### Benefits

- No secrets inside Git
- Centralized secret management
- AWS IAM-based access
- Automatic synchronization
- Reduced credential exposure

---

# 🔑 GitHub OIDC Authentication

The GitHub Actions workflow uses **OIDC-based authentication** with AWS IAM.

Long-lived AWS access keys are avoided.

```text
GitHub Actions
      │
      │ OIDC Token
      ▼
   AWS IAM
      │
      │ Temporary Credentials
      ▼
 AWS Resources
```

### Security Benefits

- No long-lived AWS credentials
- Temporary AWS credentials
- IAM-based authorization
- Reduced secret management
- Least-privilege access

---

# 📦 Containerization

Application components are containerized using Docker.

```text
Application Source
       │
       ▼
    Dockerfile
       │
       ▼
   Docker Build
       │
       ▼
 Container Image
       │
       ▼
   Trivy Scan
       │
       ▼
    Amazon ECR
       │
       ▼
    Amazon EKS
```

---

# 🏷️ Image Versioning

Container images are tagged using the **Git commit SHA**.

Example:

```text
frontend:a81f92c
backend:a81f92c
```

This provides:

- Traceability
- Reproducible deployments
- Version identification
- Easier debugging
- Faster rollback
- Avoidance of ambiguous `latest` tags

---

# 💾 Persistent Database Storage

MySQL is deployed using a Kubernetes StatefulSet with persistent storage.

```text
MySQL Pod
    │
    ▼
PersistentVolumeClaim
    │
    ▼
StorageClass
    │
    ▼
Amazon EBS
```

The persistent volume allows database data to survive Pod recreation.

---

# ⚡ CI/CD Performance Optimization

The CI/CD pipeline was optimized to reduce unnecessary execution overhead.

## Self-Hosted GitHub Actions Runner

A self-hosted runner helps reduce:

- Cold-start delays
- Repeated environment setup
- Dependency installation overhead
- Unnecessary initialization time

### Pipeline Optimization

```text
Before Optimization
        │
        ▼
    ~5 minutes
        │
        ▼
After Optimization
        │
        ▼
   ~2.5 minutes
```

**Approximate improvement: ~50%**

---

# ⚙️ Parallel Pipeline Execution

Independent security and validation jobs are executed in parallel where possible.

```text
                    CI Pipeline
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
     Gitleaks         Checkov          Trivy
        │               │               │
        └───────────────┼───────────────┘
                        ▼
                    Linting
                        │
                        ▼
                   SonarQube
                        │
                        ▼
                  Docker Build
```

This reduces overall pipeline waiting time compared with fully sequential execution.

---

# 🚀 Deployment Strategy

The deployment flow separates QA validation from production deployment.

```text
Feature Branch
      │
      ▼
   QA Branch
      │
      ▼
 Automated Security
     Scanning
      │
      ▼
    Build Image
      │
      ▼
    ECR - QA
      │
      ▼
    Deploy EKS
      │
      ▼
   QA Testing
      │
      ▼
 QA Sign-off
      │
      ▼
  Merge to Main
      │
      ▼
 Production Pipeline
      │
      ▼
 Promote Image
      │
      ▼
  ECR - Production
      │
      ▼
   Deploy EKS
      │
      ▼
 Production
```

---

# ↩️ Rollback Strategy

Because images are tagged using Git commit SHA, previous versions can be identified and redeployed.

### Check rollout history

```bash
kubectl rollout history deployment/backend
```

### Roll back deployment

```bash
kubectl rollout undo deployment/backend
```

### Monitor rollout

```bash
kubectl rollout status deployment/backend
```

### Verify Pods

```bash
kubectl get pods -n production
```

---

# 🧪 Kubernetes Troubleshooting Commands

### Check cluster nodes

```bash
kubectl get nodes
```

### Check all Pods

```bash
kubectl get pods -A
```

### Check deployments

```bash
kubectl get deployments
```

### Check services

```bash
kubectl get svc
```

### Check ingress

```bash
kubectl get ingress
```

### View application logs

```bash
kubectl logs <pod-name>
```

### Describe a Pod

```bash
kubectl describe pod <pod-name>
```

### Check cluster events

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

# 🛠️ Technology Stack

## ☁️ Cloud & Infrastructure

| Technology | Role |
|---|---|
| ![AWS](https://img.shields.io/badge/AWS-FF9900?logo=amazonaws&logoColor=white) | Cloud Platform |
| ![EKS](https://img.shields.io/badge/Amazon_EKS-FF9900?logo=amazonaws&logoColor=white) | Kubernetes Cluster |
| ![ECR](https://img.shields.io/badge/Amazon_ECR-FF9900?logo=amazonaws&logoColor=white) | Container Registry |
| ![ALB](https://img.shields.io/badge/AWS_ALB-FF9900?logo=amazonaws&logoColor=white) | Load Balancing |
| ![Route53](https://img.shields.io/badge/Route_53-8C4FFF?logo=amazonroute53&logoColor=white) | DNS |
| ![ACM](https://img.shields.io/badge/AWS_ACM-FF9900?logo=amazonaws&logoColor=white) | SSL/TLS |
| ![EBS](https://img.shields.io/badge/Amazon_EBS-FF9900?logo=amazonaws&logoColor=white) | Persistent Storage |

---

## ☸️ Containers & Orchestration

| Technology | Purpose |
|---|---|
| ![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white) | Containerization |
| ![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white) | Container Orchestration |

---

## 🔄 CI/CD

| Technology | Purpose |
|---|---|
| ![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?logo=githubactions&logoColor=white) | CI/CD Automation |
| Git | Version Control |
| GitHub | Source Code Management |

---

## 🛡️ DevSecOps

| Tool | Purpose |
|---|---|
| ![SonarQube](https://img.shields.io/badge/SonarQube-4E9BCD?logo=sonarqube&logoColor=white) | Static Code Analysis |
| Trivy | Vulnerability Scanning |
| Gitleaks | Secret Detection |
| Checkov | IaC Security |
| SBOM | Software Component Visibility |

---

## 🗄️ Application & Data

| Technology | Purpose |
|---|---|
| React | Frontend |
| Node.js | Backend |
| MySQL | Database |
| Kubernetes StatefulSet | Database Workload |
| Amazon EBS | Persistent Storage |

---

# 📁 Project Structure

```text
.
├── .github/
│   └── workflows/
│       ├── qa.yml
│       └── production.yml
│
├── frontend/
│   ├── Dockerfile
│   └── ...
│
├── backend/
│   ├── Dockerfile
│   └── ...
│
├── k8s/
│   ├── namespace.yaml
│   ├── configmap.yaml
│   ├── ingress.yaml
│   ├── frontend/
│   ├── backend/
│   └── mysql/
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
│
├── docs/
│   └── images/
│       ├── cicd-pipeline.png
│       └── eks-architecture.png
│
├── Dockerfile
├── .dockerignore
├── .gitignore
└── README.md
```

---

# 📊 DevSecOps Pipeline Coverage

| Stage | Tool | Objective |
|---|---|---|
| Source | GitHub | Version control |
| Secret Scan | Gitleaks | Prevent credential leaks |
| IaC Scan | Checkov | Detect infrastructure misconfigurations |
| Filesystem Scan | Trivy | Detect dependency vulnerabilities |
| Linting | ESLint | Code quality |
| Testing | Unit / Integration Tests | Application validation |
| Code Analysis | SonarQube | Quality & maintainability |
| Container Build | Docker | Application packaging |
| Image Scan | Trivy | Container CVE detection |
| SBOM | SBOM Generator | Dependency visibility |
| Registry | Amazon ECR | Secure image storage |
| Deployment | Amazon EKS | Kubernetes deployment |
| Authentication | GitHub OIDC | Secure AWS access |

---

# 🔒 Security Principles

This project follows the following security principles:

```text
                    DevSecOps
                       │
       ┌───────────────┼───────────────┐
       ▼               ▼               ▼
   Secure Code     Secure Images    Secure IaC
       │               │               │
   SonarQube          Trivy          Checkov
   Gitleaks           SBOM
       │               │
       └───────────────┼───────────────┘
                       ▼
                 Secure Deployment
                       │
                  GitHub OIDC
                       │
                       ▼
                    AWS IAM
```

---

# 🎯 Key Highlights

- ☁️ Deployed a 3-tier application on **Amazon EKS**
- ☸️ Designed Kubernetes-based application architecture
- 🐳 Containerized workloads using Docker
- 🔄 Implemented automated CI/CD using GitHub Actions
- 🛡️ Integrated DevSecOps security gates
- 🔑 Implemented GitHub OIDC with AWS IAM
- 📦 Used Amazon ECR for private container image storage
- 🌐 Configured Route 53 for DNS management
- 🔒 Configured HTTPS using AWS ACM
- ⚖️ Exposed workloads through AWS Application Load Balancer
- 🔐 Managed application secrets using AWS Secrets Manager
- 💾 Implemented persistent MySQL storage using Amazon EBS
- ⚡ Optimized CI/CD execution using self-hosted runners
- 🔀 Parallelized independent security and validation stages
- 🏷️ Implemented Git commit SHA-based container tagging
- ↩️ Enabled version-based rollback strategy

---

# 🚧 Future Improvements

```text
[ ] Argo CD / GitOps
[ ] Prometheus Monitoring
[ ] Grafana Dashboards
[ ] Centralized Logging
[ ] Horizontal Pod Autoscaling
[ ] Kubernetes Network Policies
[ ] Kyverno / OPA Policies
[ ] Blue-Green Deployment
[ ] Canary Deployment
[ ] Automated Rollback
[ ] Full Terraform Infrastructure
```

---



<p align="center">

<img src="https://img.shields.io/badge/Cloud--Native-DevSecOps-blue?style=for-the-badge"/>
<img src="https://img.shields.io/badge/AWS-EKS-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Kubernetes-Production--Ready-blue?style=for-the-badge"/>

</p>

<p align="center">
  ⭐ If you find this project useful, consider giving the repository a star.
</p>
