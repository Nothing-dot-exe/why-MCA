# VTU MCA 2022/2024 Scheme - Cloud Computing (22MCA25)
## Full Solved Examination Paper with Architecture, Docker, Kubernetes & AWS
Time: 3 Hours | Max Marks: 100

================================================================================
MODULE 1: CLOUD ARCHITECTURE & SERVICE MODELS (IaaS, PaaS, SaaS)
================================================================================

Q.1 (a) Compare Cloud Service Models: IaaS vs PaaS vs SaaS with Shared Responsibility Matrix. [10 Marks]
Answer:
Comparison Table:
Model | User Manages                          | Cloud Provider Manages
IaaS  | OS, Middleware, Runtime, Data, Apps  | Virtualization, Servers, Storage, Networking (AWS EC2)
PaaS  | Application Code, Data               | OS, Runtime, Middleware, Servers (AWS Elastic Beanstalk)
SaaS  | Only End-user Configurations & Data  | Everything (Gmail, Salesforce, Microsoft 365)

--------------------------------------------------------------------------------
Q.1 (b) Explain Containerization vs Virtualization. Write a production Dockerfile for a Python Web Application. [10 Marks]
Answer:
Differences:
- Virtual Machine (VM): Virtualizes hardware via Hypervisor (Type 1 or Type 2). Runs full guest OS. Boot time: minutes. Size: GBs.
- Container: Virtualizes OS kernel using Linux namespaces and cgroups. Shares host kernel. Boot time: milliseconds. Size: MBs.

Production Dockerfile:
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```
