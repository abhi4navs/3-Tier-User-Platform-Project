# 3-Tier-User-Platform-Project

# ☁️ Cloud-Native 3-Tier Application on AWS EKS

<p align="center">

<img src="https://img.shields.io/badge/AWS-EKS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white">
<img src="https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white">
<img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white">
<img src="https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white">

</p>

<p align="center">

<img src="https://img.shields.io/badge/SonarQube-4E9BCD?style=flat-square&logo=sonarqube&logoColor=white">
<img src="https://img.shields.io/badge/Trivy-1904DA?style=flat-square">
<img src="https://img.shields.io/badge/Gitleaks-red?style=flat-square">
<img src="https://img.shields.io/badge/Checkov-7B61FF?style=flat-square">

</p>

<p align="center">
<b>Production-oriented 3-Tier Application with Kubernetes, AWS EKS and DevSecOps CI/CD</b>
</p>

---

## 📌 Overview

This project demonstrates the deployment of a **cloud-native 3-tier application** on **Amazon Elastic Kubernetes Service (EKS)** using modern DevOps and DevSecOps practices.

The application is containerized using Docker and deployed through an automated GitHub Actions CI/CD pipeline.

Security is integrated throughout the software delivery lifecycle using:

- 🔐 Gitleaks
- 🛡️ Checkov
- 🐳 Trivy
- 📊 SonarQube
- 📦 SBOM generation

The production environment uses:

- Amazon EKS
- Amazon ECR
- AWS Application Load Balancer
- Route 53
- AWS Certificate Manager
- AWS IAM
- GitHub OIDC
- Kubernetes
- Amazon EBS
- AWS Secrets Manager

---

# 🏗️ Complete Architecture

<p align="center">
  <img src="./docs/images/EKS-Full-Stack.png" alt="AWS EKS Full Stack Architecture" width="100%">
</p>

### Architecture Flow

```text
User
  │
  ▼
Namecheap
  │
  ▼
Route 53
  │
  ▼
AWS Application Load Balancer
  │
  ▼
Kubernetes Ingress
  │
  ▼
Application Pods
  │
  ├── Frontend
  │
  └── Backend
        │
        ▼
      MySQL
        │
        ▼
   Amazon EBS
