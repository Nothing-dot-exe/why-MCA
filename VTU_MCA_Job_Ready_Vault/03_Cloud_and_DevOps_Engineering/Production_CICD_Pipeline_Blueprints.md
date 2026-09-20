# Production CI/CD Pipeline Blueprints (GitHub Actions)
**Purpose**: Ready-to-use, enterprise-grade GitHub Actions workflows for continuous integration, automated testing, container security scanning, and cloud deployment.

---

## Blueprint 1: Complete CI Pipeline (Lint, Test, Build & Security Scan)
Place in `.github/workflows/ci.yml`:

```yaml
name: Continuous Integration & Security Gate

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  lint-and-test:
    name: Code Quality & Unit Tests
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Setup Node.js Runtime
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'

      - name: Install Dependencies
        run: npm ci

      - name: Run Linter (ESLint)
        run: npm run lint

      - name: Execute Unit & Integration Tests with Coverage
        run: npm run test:coverage

      - name: Upload Test Coverage Artifact
        uses: actions/upload-artifact@v4
        with:
          name: coverage-report
          path: coverage/

  security-scan:
    name: SAST & Secret Scanning
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Scan for Hardcoded Secrets (GitLeaks)
        uses: gitleaks/gitleaks-action@v2
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Run Trivy Vulnerability Scanner (Filesystem)
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          ignore-unfixed: true
          severity: 'CRITICAL,HIGH'
          format: 'table'

  build-container:
    name: Build & Push Docker Image
    needs: [lint-and-test, security-scan]
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Log in to GitHub Container Registry (GHCR)
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and Push Docker Image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:latest
            ghcr.io/${{ github.repository }}:${{ github.sha }}
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

---

## Blueprint 2: Kubernetes Continuous Deployment (GitOps / ArgoCD)
Place in `.github/workflows/deploy-k8s.yml`:

```yaml
name: Deploy to Kubernetes Cluster

on:
  workflow_run:
    workflows: ["Continuous Integration & Security Gate"]
    branches: [main]
    types: [completed]

jobs:
  deploy:
    name: Trigger Rolling Deployment
    if: ${{ github.event.workflow_run.conclusion == 'success' }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Helm / K8s Manifests
        uses: actions/checkout@v4

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      - name: Update Kubeconfig for EKS
        run: aws eks update-kubeconfig --name production-cluster --region us-east-1

      - name: Rolling Update Deployment
        run: |
          kubectl set image deployment/enterprise-api \
            api-container=ghcr.io/${{ github.repository }}:${{ github.sha }} \
            --namespace=production
          kubectl rollout status deployment/enterprise-api --namespace=production --timeout=120s
```
