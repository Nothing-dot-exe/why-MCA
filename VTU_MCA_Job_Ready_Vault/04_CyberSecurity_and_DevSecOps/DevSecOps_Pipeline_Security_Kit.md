# DevSecOps Pipeline Security Kit
**Concept**: Shifting Security Left into the CI/CD Pipeline.

---

## 1. The 4 Layers of Automated Pipeline Security

```
[Developer Code Commit]
         │
         ├─► Layer 1: Secret Scanning (Pre-commit / CI) ──► GitLeaks / Trufflehog
         ├─► Layer 2: Static Application Security (SAST) ──► Semgrep / SonarQube
         ├─► Layer 3: Software Composition Analysis (SCA) ─► Trivy / Snyk / Dependabot
         ├─► Layer 4: Dynamic Application Security (DAST) ─► OWASP ZAP (on staging)
         ▼
[Production Deployment (Signed with Cosign & Verified)]
```

---

## 2. DevSecOps GitHub Actions Workflow Configuration
Save as `.github/workflows/devsecops-gate.yml`:

```yaml
name: DevSecOps Quality Gate

on: [push, pull_request]

jobs:
  secret-scan:
    name: GitLeaks Secret Scan
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: gitleaks/gitleaks-action@v2
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  sast-semgrep:
    name: Semgrep Static Security Analysis
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Semgrep Rules
        run: |
          docker run --rm -v "${PWD}:/src" returntocorp/semgrep \
            semgrep --config "p/security-audit" --config "p/owasp-top-ten" --error

  container-vulnerability-trivy:
    name: Trivy Container Image Scan
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build Docker Image locally
        run: docker build -t local-app:${{ github.sha }} .
      - name: Scan Image for High & Critical CVEs
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: local-app:${{ github.sha }}
          format: 'table'
          exit-code: '1' # Fail build if CRITICAL CVE is detected
          ignore-unfixed: true
          severity: 'CRITICAL,HIGH'
```
