# Capstone Production Project 2: Cloud GitOps & Observability Platform
**Resume Project Title**: *Autonomous GitOps Cloud Infrastructure on AWS EKS with ArgoCD, Terraform IaC, and Prometheus/Grafana Telemetry*

---

## 1. Project Overview
An enterprise-scale, production-ready cloud platform where infrastructure provisioning is 100% automated via Terraform, application deployments are synchronised via GitOps (ArgoCD), and full-stack cluster telemetry is monitored via Prometheus and Grafana dashboards.

---

## 2. Infrastructure Architecture & Data Flow

```
[Developer Git Push]
         │
         ▼
[GitHub Actions CI] ──► [Build & Scan Docker Image] ──► [Push to GHCR / ECR]
                                                              │
[Infrastructure Repo (Helm)] ◄────────────────────────────────┘
         │
         ▼
[ArgoCD GitOps Operator (EKS Cluster)]
         │ Detects Git commit drift & auto-syncs
         ▼
[Kubernetes Pods & Services] ◄── [AWS ALB Ingress Controller]
         │
         ▼ Metrics Exporters
[Prometheus TSDB] ──► [Grafana Dashboards] ──► [Alertmanager PagerDuty/Slack]
```

---

## 3. Kubernetes Deployment & Service Manifest (`deployment.yaml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cloud-api-deployment
  namespace: production
  labels:
    app: cloud-api
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  selector:
    matchLabels:
      app: cloud-api
  template:
    metadata:
      labels:
        app: cloud-api
    spec:
      containers:
        - name: api
          image: ghcr.io/org/cloud-api:v1.2.0
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 8080
          resources:
            requests:
              cpu: "250m"
              memory: "512Mi"
            limits:
              cpu: "500m"
              memory: "1Gi"
          readinessProbe:
            httpGet:
              path: /healthz
              port: 8080
            initialDelaySeconds: 10
            periodSeconds: 5
          livenessProbe:
            httpGet:
              path: /live
              port: 8080
            initialDelaySeconds: 15
            periodSeconds: 10
---
apiVersion: v1
kind: Service
metadata:
  name: cloud-api-service
  namespace: production
spec:
  type: ClusterIP
  selector:
    app: cloud-api
  ports:
    - port: 80
      targetPort: 8080
```

---

## 4. Terraform EKS Cluster Blueprint (`main.tf`)

```hcl
terraform {
  required_version = ">= 1.5.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  backend "s3" {
    bucket         = "prod-terraform-state-vault"
    key            = "eks/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}

provider "aws" {
  region = "us-east-1"
}

module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.0.0"

  name = "prod-eks-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["us-east-1a", "us-east-1b", "us-east-1c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

  enable_nat_gateway   = true
  single_nat_gateway   = true
  enable_dns_hostnames = true
}
```

---

## 5. ATS-Ready Resume Bullets for this Project
* *Constructed an automated GitOps deployment pipeline using ArgoCD and Kubernetes (AWS EKS), reducing deployment cycle times by 75% while maintaining zero downtime across rolling releases.*
* *Provisioned modular cloud infrastructure across multi-AZ VPCs using Terraform with remote S3 state locking and automated drift detection.*
* *Instrumented end-to-end cluster observability with Prometheus and Grafana, deploying real-time alerting for pod crash loops, memory saturation, and elevated HTTP 5xx error rates.*
* *Engineered self-healing Kubernetes workloads configured with horizontal pod autoscalers (HPA), graceful pod disruption budgets, and automated health probes.*
