# Capstone Production Project 3: Zero-Trust Security Gateway & Secret Vault
**Resume Project Title**: *Zero-Trust Microservice Security Gateway with OAuth2/OIDC, Automated SAST/DAST Security Scanning & HashiCorp Vault Integration*

---

## 1. Project Overview
A cloud-native security gateway enforcing strict **"Never Trust, Always Verify"** principles across internal microservices. Features automated JWT signature verification with cryptographic key rotation (JWKS), dynamic IP reputation rate-limiting, and centralized secret injection via HashiCorp Vault.

---

## 2. Security Architecture Diagram

```
[External Traffic (Untrusted)]
               │
               ▼
   [Cloudflare WAF / DDoS Shield]
               │ HTTPS (TLS 1.3 Strict)
               ▼
[Zero-Trust Security Gateway (Kong / Envoy)]
  ├─► Check 1: IP Rate Limiting (Redis Sliding Window)
  ├─► Check 2: JWT / OAuth2 Cryptographic Verification (RS256)
  ├─► Check 3: RBAC / ABAC Permissions Engine (Open Policy Agent)
  └─► Check 4: Payload Sanitization & Header Hardening
               │ mTLS (Mutual TLS with SPIFFE/SPIRE IDs)
               ▼
    [Internal Microservices Mesh]
               ▲
               │ Dynamic Secrets (Rotated every 1 hr)
      [HashiCorp Vault]
```

---

## 3. JWT Verification Middleware with JWKS Key Rotation (Node.js/TypeScript)

```typescript
import jwt from 'jsonwebtoken';
import jwksClient from 'jwks-rsa';
import { Request, Response, NextFunction } from 'express';

const client = jwksClient({
  jwksUri: 'https://auth.company.internal/.well-known/jwks.json',
  cache: true,
  rateLimit: true,
  jwksRequestsPerMinute: 10
});

function getKey(header: jwt.JwtHeader, callback: jwt.SigningKeyCallback) {
  client.getSigningKey(header.kid, (err, key) => {
    if (err || !key) return callback(err || new Error('Signing key not found'));
    const signingKey = key.getPublicKey();
    callback(null, signingKey);
  });
}

export function enforceZeroTrustAuth(req: Request, res: Response, next: NextFunction) {
  const authHeader = req.headers.authorization;
  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return res.status(401).json({ error: 'Missing or malformed Authorization header' });
  }

  const token = authHeader.split(' ')[1];
  jwt.verify(token, getKey, { algorithms: ['RS256'], issuer: 'https://auth.company.internal' }, (err, decoded) => {
    if (err) {
      return res.status(403).json({ error: 'Forbidden: Invalid or expired security token' });
    }
    (req as any).user = decoded;
    next();
  });
}
```

---

## 4. ATS-Ready Resume Bullets for this Project
* *Designed and deployed a Zero-Trust API Security Gateway processing 15,000+ requests/min with RS256 asymmetric JWT verification and automated public key rotation (JWKS).*
* *Integrated HashiCorp Vault to eliminate plaintext credentials from Git and configuration files, enforcing automated 60-minute dynamic database credential rotation.*
* *Instituted automated DevSecOps security gating into GitHub Actions CI/CD pipelines using Semgrep and Trivy, blocking builds exceeding CVSS 7.0 threshold.*
* *Constructed Redis-backed distributed rate-limiting middleware mitigating API brute-force and Layer-7 DDoS attacks.*
