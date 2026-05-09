# DevOps Portfolio

[![CI/CD Pipeline](https://github.com/balbirsingh08/devops-portfolio/actions/workflows/ci.yml/badge.svg)](https://github.com/balbirsingh08/devops-portfolio/actions/workflows/ci.yml)
[![GitHub Actions](https://img.shields.io/badge/GitHub-Actions-2088FF?style=flat-square&logo=github)](https://github.com/features/actions)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=flat-square&logo=terraform&logoColor=white)](https://www.terraform.io/)
[![AWS](https://img.shields.io/badge/AWS-FF9900?style=flat-square&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)

Production-ready DevOps portfolio showcasing modern infrastructure-as-code practices, containerization, orchestration, and CI/CD automation.

## 🏗️ Architecture Overview

```
                          ┌─────────────────────────────────┐
                          │     GitHub / Git Repository     │
                          └────────────────┬────────────────┘
                                           │
                          ┌────────────────▼─────────────────┐
                          │   GitHub Actions CI/CD Pipeline  │
                          │  (lint, test, build, push, scan) │
                          └────────────────┬─────────────────┘
                                           │
                    ┌──────────────────────┼──────────────────────┐
                    │                      │                      │
                    ▼                      ▼                      ▼
            ┌────────────────┐    ┌─────────────────┐   ┌──────────────┐
            │  GHCR Registry │    │  Jenkins (Alt)  │   │ Docker Build │
            │  (Container)   │    │     Pipeline    │   │   Locally    │
            └────────┬───────┘    └─────────────────┘   └──────┬───────┘
                     │                                          │
                     └──────────────────┬───────────────────────┘
                                        │
                          ┌─────────────▼──────────────┐
                          │   Deploy to EKS/GKE       │
                          │  (Kubernetes Cluster)     │
                          └─────────────┬──────────────┘
                                        │
            ┌───────────────────────────┼───────────────────────────┐
            │                           │                           │
            ▼                           ▼                           ▼
    ┌──────────────┐          ┌──────────────────┐        ┌───────────────┐
    │ NGINX Ingress│          │ Kubernetes Pods  │        │  HPA Scaler   │
    │   + TLS      │          │ (App Replicas)   │        │ (CPU/Memory)  │
    └──────────────┘          └──────────────────┘        └───────────────┘
            │                           │                           │
            └───────────────┬───────────┴─────────────────┬─────────┘
                            │                             │
                    ┌───────▼─────┐          ┌────────────▼────────┐
                    │  Services   │          │  Monitoring Stack  │
                    │  (DB, Redis)│          │  (Prom + Grafana)  │
                    └─────────────┘          └─────────────────────┘
```

## 📋 Project Structure

```
devops-portfolio/
├── ci-cd/                          # CI/CD Pipeline Definitions
│   └── .github/workflows/
│       ├── ci.yml                  # Build, test, lint, security scan
│       └── cd.yml                  # Deploy to staging and production
│   └── Jenkinsfile                 # Jenkins declarative pipeline (alternative)
│
├── docker/                         # Containerization
│   ├── Dockerfile                  # Multi-stage build
│   ├── docker-compose.yml          # Local development stack
│   ├── nginx.conf                  # Reverse proxy configuration
│   └── .dockerignore
│
├── kubernetes/                     # Kubernetes Manifests
│   ├── deployment.yaml             # Deployment with rolling updates
│   ├── service.yaml                # Service discovery (ClusterIP + LB)
│   ├── ingress.yaml                # NGINX ingress controller + TLS
│   ├── hpa.yaml                    # Horizontal Pod Autoscaling
│   ├── configmap.yaml              # Non-sensitive config
│   └── secret.yaml                 # Sensitive data (use sealed-secrets in prod)
│
├── terraform/                      # Infrastructure as Code
│   ├── main.tf                     # EKS cluster provisioning
│   ├── variables.tf                # Input variables
│   ├── outputs.tf                  # Output values
│   ├── backend.tf                  # S3 remote state configuration
│   └── modules/
│       └── vpc/
│           ├── main.tf
│           ├── variables.tf
│           └── outputs.tf
│
├── ansible/                        # Configuration Management
│   ├── playbook.yml                # Server provisioning
│   ├── inventory/
│   │   └── hosts.ini               # Host inventory
│   └── roles/
│       ├── docker/
│       ├── monitoring/
│       └── security/
│
├── monitoring/                     # Observability & Alerting
│   ├── prometheus.yml              # Prometheus scrape configs
│   ├── grafana-dashboard.json      # Pre-built Grafana dashboard
│   └── alertmanager.yml            # Alert routing and rules
│
├── scripts/                        # Operational Scripts
│   ├── deploy.sh                   # Deployment automation
│   ├── healthcheck.sh              # Application health checks
│   └── rollback.sh                 # Rollback procedures
│
├── .pre-commit-config.yaml         # Pre-commit hooks
├── CONTRIBUTING.md                 # Contribution guidelines
├── LICENSE                         # MIT License
└── README.md                       # This file
```

## 🎯 What Each Component Demonstrates

### 🔄 CI/CD (`/ci-cd`)
- **GitHub Actions**: Multi-stage pipeline with:
  - YAML linting and validation
  - Unit test execution
  - Docker image build and push to GHCR
  - Security scanning (Trivy)
  - Automated deployment to staging/prod
- **Jenkins**: Declarative pipeline as an alternative with similar stages
- **Key Skills**: Workflow automation, artifact management, deployment strategies

### 🐳 Docker (`/docker`)
- **Multi-stage Dockerfile**: Optimized image size (~100MB final vs 500MB+)
  - Builder stage for compilation/dependencies
  - Runtime stage with minimal base image
- **docker-compose.yml**: Full-stack local development with services:
  - Application server
  - PostgreSQL database
  - Redis cache
  - NGINX reverse proxy
- **Key Skills**: Container optimization, local development environments

### ☸️ Kubernetes (`/kubernetes`)
- **Deployment**: Rolling update strategy with resource limits/requests
- **Service**: ClusterIP (internal) and LoadBalancer (external)
- **Ingress**: NGINX ingress controller with TLS/SSL termination
- **HPA**: Auto-scaling based on CPU/memory metrics
- **ConfigMap/Secret**: Configuration and secret management
- **Key Skills**: Container orchestration, network policies, scaling

### 🏗️ Terraform (`/terraform`)
- **EKS Cluster**: AWS Elastic Kubernetes Service provisioning
- **VPC Module**: Reusable module for network infrastructure
- **S3 Backend**: Remote state management with locking
- **Variables/Outputs**: Parameterized infrastructure
- **Key Skills**: IaC best practices, modular design, state management

### 🤖 Ansible (`/ansible`)
- **Playbooks**: Server provisioning and configuration
- **Roles**: Modular, reusable configuration roles
- **Inventory**: Multi-environment host management
- **Key Skills**: Configuration management, idempotency, automation

### 📊 Monitoring (`/monitoring`)
- **Prometheus**: Metric collection and time-series database
- **Grafana**: Pre-built dashboards with key metrics:
  - CPU, memory, disk usage
  - Application request rates
  - Error rates and response times
- **Alertmanager**: Alert routing, grouping, and notifications
- **Key Skills**: Observability, alerting, SLOs/SLIs

### 🔧 Scripts (`/scripts`)
- **deploy.sh**: Blue-green or rolling deployment automation
- **healthcheck.sh**: Application readiness verification
- **rollback.sh**: Safe rollback procedures
- **Key Skills**: Operational automation, reliability

## 🚀 Quick Start

### Prerequisites
- Docker & Docker Compose
- kubectl
- Terraform ≥ 1.0
- Helm
- Pre-commit hooks

### Local Development

```bash
# Clone repository
git clone https://github.com/balbirsingh08/devops-portfolio.git
cd devops-portfolio

# Install pre-commit hooks
pre-commit install

# Run complete stack locally
docker-compose up -d

# Verify services
curl http://localhost:8080/health
curl http://localhost:5432  # PostgreSQL
curl http://localhost:6379  # Redis
```

### Kubernetes Deployment

```bash
# Validate manifests
kubectl apply -f kubernetes/ --dry-run=client

# Deploy to cluster
kubectl apply -f kubernetes/

# Check deployment status
kubectl get deployments,services,ingress -w

# View pod logs
kubectl logs -f deployment/myapp
```

### Terraform Infrastructure

```bash
# Initialize backend
terraform init

# Plan infrastructure changes
terraform plan -out=tfplan

# Apply infrastructure
terraform apply tfplan

# Retrieve outputs
terraform output
```

## 🔐 Security Practices Demonstrated

- ✅ Container image scanning (Trivy)
- ✅ Secret management (Kubernetes secrets + sealed-secrets)
- ✅ RBAC for Kubernetes access control
- ✅ Network policies and ingress TLS
- ✅ Resource quotas and pod security policies
- ✅ Pre-commit hooks for code quality
- ✅ Terraform state encryption (S3 versioning + KMS)

## 📈 Monitoring & Observability

- **Metrics**: CPU, memory, disk, network I/O
- **Logs**: Centralized logging with CloudWatch/ELK
- **Traces**: Distributed tracing (OpenTelemetry compatible)
- **Dashboards**: Pre-built Grafana dashboards
- **Alerts**: Prometheus AlertManager with email/Slack integration

## 📚 Tech Stack

| Component | Technology | Purpose |
|-----------|-----------|----------|
| Version Control | Git + GitHub | Source management |
| CI/CD | GitHub Actions + Jenkins | Automation |
| Containerization | Docker | Image building |
| Orchestration | Kubernetes (EKS) | Container management |
| IaC | Terraform | Infrastructure provisioning |
| Configuration Mgmt | Ansible | Server provisioning |
| Monitoring | Prometheus + Grafana | Observability |
| Ingress | NGINX Ingress Controller | Traffic routing |
| Logging | CloudWatch/ELK | Log aggregation |

## 🔗 Live Demo

**Staging Environment**: [https://staging.myapp.io](https://staging.myapp.io) *(placeholder)*

**Monitoring Dashboard**: [https://monitoring.myapp.io/grafana](https://monitoring.myapp.io/grafana) *(placeholder)*

## 📖 Documentation

- [CI/CD Pipeline Guide](./ci-cd/README.md)
- [Kubernetes Best Practices](./kubernetes/README.md)
- [Terraform Modules](./terraform/README.md)
- [Deployment Procedures](./scripts/README.md)

## 🤝 Contributing

Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines on:
- Code standards
- Testing requirements
- Pull request process
- Commit message conventions

## 📝 License

This project is licensed under the MIT License - see [LICENSE](./LICENSE) file for details.

## 👤 Author

**Balbir Singh**
- GitHub: [@balbirsingh08](https://github.com/balbirsingh08)

---

**Last Updated**: 2026-05-09 | **Version**: 1.0.0
