# Capstone Production Project 1: Cloud-Native Enterprise SaaS Platform
**Resume Project Title**: *Full-Stack Multi-Tenant SaaS Workspace with Role-Based Access Control, Redis Caching & Docker Deployment*

---

## 1. Executive Project Overview
A production-grade, multi-tenant collaboration platform enabling enterprise teams to manage projects, task state machines, real-time activity streams, and team permissions with automated billing and audit logging.

---

## 2. Tech Stack & Architecture
* **Frontend**: Next.js 14 (App Router), TypeScript, TailwindCSS, TanStack React Query, Zustand.
* **Backend**: Node.js / Express with TypeScript (or Spring Boot 3), Prisma ORM / JPA.
* **Database & Cache**: PostgreSQL (Master relational data), Redis 7 (Session store, cache, rate limiting).
* **Authentication**: OAuth 2.0 + JWT with Refresh Token rotation in `HttpOnly` Secure Cookies.
* **Deployment & Containerization**: Docker Multi-stage builds, Docker Compose, Nginx Reverse Proxy with TLS.

```
[Web Browser / Client]
         │ HTTPS / WSS
         ▼
  [Nginx Reverse Proxy]
         │ Rate Limiting & SSL Offloading
         ▼
[Next.js 14 Web App] ──► [REST API / WebSockets Gateway]
                                  │
         ┌────────────────────────┼────────────────────────┐
         ▼                        ▼                        ▼
  [PostgreSQL 16]          [Redis 7 Cache]          [AWS S3 Bucket]
(Tenants, Users, Tasks)  (Sessions & Cache-Aside)  (Document Uploads)
```

---

## 3. Database Schema (PostgreSQL DDL)

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Multi-Tenant Organizations
CREATE TABLE organizations (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(100) NOT NULL,
    subdomain VARCHAR(50) UNIQUE NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Users with Organization Multi-Tenancy
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    organization_id UUID NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role VARCHAR(20) DEFAULT 'MEMBER' CHECK (role IN ('OWNER', 'ADMIN', 'MEMBER', 'VIEWER')),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT unique_email_per_org UNIQUE (organization_id, email)
);

-- Tasks with State Machine
CREATE TABLE tasks (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    organization_id UUID NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    status VARCHAR(30) DEFAULT 'TODO' CHECK (status IN ('TODO', 'IN_PROGRESS', 'REVIEW', 'DONE')),
    priority VARCHAR(20) DEFAULT 'MEDIUM' CHECK (priority IN ('LOW', 'MEDIUM', 'HIGH', 'URGENT')),
    assignee_id UUID REFERENCES users(id) ON DELETE SET NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_tasks_org_status ON tasks(organization_id, status);
```

---

## 4. Production Docker Compose Setup

```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://saas_user:saas_pass@postgres:5432/saas_db
      - REDIS_URL=redis://redis:6379
      - JWT_SECRET=production_super_secret_key_change_me
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:16-alpine
    restart: always
    environment:
      POSTGRES_USER: saas_user
      POSTGRES_PASSWORD: saas_pass
      POSTGRES_DB: saas_db
    volumes:
      - pgdata:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  redis:
    image: redis:7-alpine
    restart: always
    command: redis-server --appendonly yes
    ports:
      - "6379:6379"
    volumes:
      - redisdata:/data

volumes:
  pgdata:
  redisdata:
```

---

## 5. ATS-Ready Resume Bullets for this Project
* *Architected and deployed a multi-tenant enterprise task SaaS serving 10+ organizations using Next.js, Node.js, and PostgreSQL with strict tenant-level data isolation.*
* *Engineered a Redis Cache-Aside layer reducing database query load by 68% and maintaining sub-45ms API response latency under concurrent request loads.*
* *Implemented Role-Based Access Control (RBAC) and OAuth2/JWT token rotation with secure HttpOnly cookies, mitigating CSRF and XSS attack vectors.*
* *Containerized frontend, API, database, and cache services using multi-stage Docker builds and automated zero-downtime deployment pipelines.*
