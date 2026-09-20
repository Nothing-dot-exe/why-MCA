# Master Study Notes: Cloud Computing & DevOps
## Course Code: 22MCA25 / Professional Elective | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: NIST Cloud Computing Definition, 5 Essential Characteristics, Service Models (IaaS, PaaS, SaaS), Shared Responsibility Model, Deployment Models (Public, Private, Hybrid, Community).
* **Module 2**: Virtualization Architectures (Type 1 vs. Type 2 Hypervisors), Containers vs. Virtual Machines, Docker Ecosystem, Writing Optimized Multi-Stage `Dockerfile`, Docker Compose.
* **Module 3**: Kubernetes (K8s) Cluster Architecture (Control Plane vs Worker Nodes), Pods, Deployments, Services (ClusterIP, NodePort, LoadBalancer), Ingress & Secrets.
* **Module 4**: AWS Core Cloud Architecture (VPC, Public/Private Subnets, Internet & NAT Gateways, EC2, S3 Storage Classes, IAM Policies, Lambda Serverless).
* **Module 5**: DevOps & CI/CD Automation, Infrastructure as Code (Terraform IaC), GitHub Actions CI/CD Blueprints, Container Security Scanning (Trivy), Prometheus & Grafana Monitoring.

---

# MODULE 1: CLOUD FOUNDATIONS & SERVICE MODELS

## 1.1 NIST 5 Essential Characteristics of Cloud Computing
1. **On-Demand Self-Service**: Unilateral provisioning of computing capabilities (server time, network storage) without human interaction with cloud providers.
2. **Broad Network Access**: Capabilities available over the network and accessed through standard mechanisms by heterogeneous thin/thick clients (laptops, phones).
3. **Resource Pooling**: Multi-tenant model serving multiple consumers dynamically allocated from pooled physical and virtual resources.
4. **Rapid Elasticity**: Capabilities provisioned and released rapidly, elastically, to scale outward and inward with demand.
5. **Measured Service**: Resource usage monitored, controlled, and billed transparently based on actual metrics (Pay-As-You-Go).

---

## 1.2 The Shared Responsibility Model

```
+-------------------------------------------------------------+
|               SaaS (e.g. Gmail, Salesforce)                 |
| Customer: Data & Access Management                          |
| Provider: Application, OS, Hardware, Network, Facilities    |
+-------------------------------------------------------------+
|               PaaS (e.g. AWS Elastic Beanstalk, Vercel)     |
| Customer: Application Code & Data                           |
| Provider: OS, Runtime, Middleware, Hardware, Data Center    |
+-------------------------------------------------------------+
|               IaaS (e.g. AWS EC2, Azure VMs, GCP Compute)   |
| Customer: OS, Patches, Networking Rules, Runtime, Apps      |
| Provider: Physical Hardware, Hypervisor, Data Center Cooling|
+-------------------------------------------------------------+
```

---

# MODULE 2: VIRTUALIZATION & DOCKER

## 2.1 Hypervisors: Type 1 vs. Type 2
* **Type 1 (Bare-Metal)**: Runs directly on physical hardware without underlying host OS (e.g., VMware ESXi, KVM, Xen, Microsoft Hyper-V). High performance, enterprise standard.
* **Type 2 (Hosted)**: Runs on top of an existing host operating system as an application (e.g., Oracle VirtualBox, VMware Workstation). Higher overhead.

---

## 2.2 Containers vs. Virtual Machines

| Feature | Virtual Machines (VMs) | Docker Containers |
| :--- | :--- | :--- |
| **Architecture** | Includes full Guest OS + Hypervisor | Shares Host OS Kernel via Linux namespaces & cgroups |
| **Startup Time** | Minutes (Booting full OS) | Milliseconds |
| **Storage Footprint**| Gigabytes (GBs) | Megabytes (MBs) |
| **Isolation** | Hardware-level isolation (Very High) | Process-level isolation |
| **Performance** | Native virtualization penalty | Near bare-metal CPU/memory speed |

---

## 2.3 Optimized Multi-Stage Production `Dockerfile`
```dockerfile
# Stage 1: Build & Dependencies
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .

# Stage 2: Minimal Production Runner
FROM node:20-alpine AS runner
WORKDIR /app
# Run as non-root user for enterprise DevSecOps security
USER node
COPY --from=builder --chown=node:node /app /app

EXPOSE 3000
ENV NODE_ENV=production
CMD ["node", "server.js"]
```

---

# MODULE 3: KUBERNETES (K8S) ORCHESTRATION

## 3.1 Master (Control Plane) Components:
* **API Server (`kube-apiserver`)**: The front door to the cluster. All internal/external communications go through REST JSON API.
* **etcd**: Consistent, highly available distributed key-value store for all cluster state data.
* **Controller Manager (`kube-controller-manager`)**: Runs controller loops (Node Controller, Replication Controller, Endpoint Controller).
* **Scheduler (`kube-scheduler`)**: Watches newly created Pods without assigned nodes and selects the optimal worker node based on resource requests.

## 3.2 Worker Node Components:
* **`kubelet`**: Primary node agent ensuring that containers described in `PodSpecs` are running and healthy.
* **`kube-proxy`**: Network proxy managing IP translation and routing rules (iptables/IPVS) for Kubernetes Services.
* **Container Runtime**: Software executing containers (e.g., `containerd`, `CRI-O`).

---

# MODULE 4: AWS CLOUD INFRASTRUCTURE

## 4.1 VPC Architecture (Public vs. Private Subnets)
* **VPC (Virtual Private Cloud)**: Logically isolated virtual network inside an AWS Region.
* **Public Subnet**: Subnet whose route table has a route to the **Internet Gateway (IGW)** (`0.0.0.0/0 -> igw-xxxx`). Used for Load Balancers and Web Bastion hosts.
* **Private Subnet**: Subnet with no direct route to the Internet. Instances talk outbound via a **NAT Gateway** located in the public subnet. Used for Databases (RDS) and internal microservices.

---

# MODULE 5: DEVSECOPS & CI/CD AUTOMATION

## 5.1 GitHub Actions CI/CD Pipeline (`.github/workflows/deploy.yml`)
```yaml
name: Production DevSecOps Pipeline

on:
  push:
    branches: [ main ]

jobs:
  security-and-build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Static Code Analysis (SAST)
        run: echo "Running SonarQube analysis..."

      - name: Build Docker Image
        run: docker build -t myapp:${{ github.sha }} .

      - name: Vulnerability Scanning with Trivy
        run: |
          docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
          aquasec/trivy:latest image --severity HIGH,CRITICAL myapp:${{ github.sha }}

      - name: Deploy to Kubernetes Cluster
        if: success()
        run: echo "Applying kubectl rollout to production EKS..."
```
