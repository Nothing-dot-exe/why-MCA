# Cloud & DevOps Toolchain & Linux Systems Engineering Notes
**Core Stack**: Linux (Ubuntu/Debian/RHEL), Docker, Kubernetes, Helm, Terraform, AWS, GitHub Actions.

---

## 1. High-Yield Linux Systems Administration & Troubleshooting

### Critical Commands Every DevOps Engineer Must Know
| Tool / Command | What It Diagnoses | Example Usage |
| :--- | :--- | :--- |
| `top` / `htop` | CPU/Memory utilization per process | Real-time process monitoring |
| `iostat -xz 1` | Disk I/O bottlenecks & %utilization | Identify saturated disk storage |
| `vmstat 1` | System memory, swap, context switches, CPU | Gauge memory pressure and CPU wait |
| `netstat -tulpn` or `ss -tulpn` | Listening network ports and associated PIDs | Identify which process is holding a port |
| `journalctl -u <service> -f` | Systemd service logs in real time | Live troubleshooting of daemon failures |
| `curl -Iv https://example.com` | TLS handshake, HTTP status, header inspection | Network connectivity and SSL diagnosis |
| `lsof -i :<port>` | Lists open files and network sockets on a port | Kill stuck processes holding port 80/443 |

### The Linux Filesystem Hierarchy
* `/etc`: System-wide configuration files (`/etc/hosts`, `/etc/nginx/nginx.conf`, `/etc/systemd/system`).
* `/var/log`: Centralized application and OS logs (`syslog`, `auth.log`, `nginx/access.log`).
* `/proc`: Virtual pseudo-filesystem exposing kernel state and process runtime information (`/proc/cpuinfo`, `/proc/meminfo`, `/proc/<pid>/status`).

---

## 2. Docker & Containerization Best Practices

### Multi-Stage Dockerfile (Minimizing Attack Surface & Image Size)
* **Problem**: Building inside a single Docker image leaves compilers, SDKs, package managers, and secret build arguments inside the final container, ballooning image size to 1.5+ GB.
* **Solution**: Multi-stage build separates the build environment from the minimal runtime environment (Alpine / Distroless).

```dockerfile
# Stage 1: Build & Compile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

# Stage 2: Minimal Production Runtime
FROM node:20-alpine AS runner
WORKDIR /app
# Run as non-root user for container security
USER node
COPY --chown=node:node --from=builder /app/package*.json ./
COPY --chown=node:node --from=builder /app/node_modules ./node_modules
COPY --chown=node:node --from=builder /app/dist ./dist

EXPOSE 3000
CMD ["node", "dist/main.js"]
```

---

## 3. Kubernetes (K8s) Architecture & Core Objects

```
                ┌──────────────────────────────────────────────┐
                │          Kubernetes Control Plane            │
                │  API Server │ etcd │ Scheduler │ Controller  │
                └──────────────────────┬───────────────────────┘
                                       │
                ┌──────────────────────┴───────────────────────┐
                ▼                                              ▼
       [Worker Node 1]                                [Worker Node 2]
 ┌───────────────────────────┐                  ┌───────────────────────────┐
 │ kubelet │ kube-proxy      │                  │ kubelet │ kube-proxy      │
 │  ┌─────────────────────┐  │                  │  ┌─────────────────────┐  │
 │  │ Pod A (App Container)│ │                  │  │ Pod B (App Container)│ │
 │  └─────────────────────┘  │                  │  └─────────────────────┘  │
 └───────────────────────────┘                  └───────────────────────────┘
```

### Core K8s Objects
1. **Pod**: Smallest deployable unit in K8s. Represents one or more co-located containers sharing network namespaces (`localhost`) and storage volumes.
2. **Deployment**: Manages declarative state of Pods. Automates replica counts, rolling updates, and rollbacks.
3. **Service**: Stable network endpoint (Virtual IP / DNS) abstracting ephemeral Pod IP addresses.
   - `ClusterIP`: Internal-only communication inside cluster.
   - `NodePort`: Exposes service on a static high port (`30000-32767`) on each worker node.
   - `LoadBalancer`: Provisions an external cloud load balancer (AWS ALB / GCP LB).
4. **Ingress**: Layer 7 HTTP/HTTPS reverse proxy routing traffic based on host header and URI path.
5. **ConfigMap & Secret**: Injects configuration and encrypted credentials as environment variables or mounted files without modifying container images.

---

## 4. Infrastructure as Code (IaC): Terraform

* **State File (`terraform.tfstate`)**: Stores the mapping between declared HCL resources and real cloud infrastructure. Always store remotely (AWS S3 + DynamoDB state locking).
* **Core Workflow**:
  - `terraform init`: Downloads cloud provider plugins (AWS/Azure/GCP).
  - `terraform plan`: Generates execution plan showing additions, changes, and destructions.
  - `terraform apply`: Provisions actual cloud resources via provider APIs.
