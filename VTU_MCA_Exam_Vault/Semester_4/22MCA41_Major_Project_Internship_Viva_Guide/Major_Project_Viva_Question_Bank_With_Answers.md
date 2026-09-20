# VTU MCA 2022/2024 Scheme - Major Project & Viva-Voce Master Guide (22MCA41)
## Complete Guide to Dissertation Writing, Architecture Design, & University Viva Questions
Max Marks: 100 | External University Viva Defense

================================================================================
1. DISSERTATION REPORT FORMAT & GUIDELINES (VTU STANDARDS)
================================================================================
According to VTU Regulations (Clause 24OMC6.0 & 22OMC6.0), the project dissertation must follow strict formatting:
- Font: Times New Roman 12pt, 1.5 Line Spacing.
- Margins: Left 1.5 inches, Right/Top/Bottom 1.0 inch.
- Chapters:
  1. Introduction & Problem Statement
  2. Literature Survey (Minimum 10 IEEE/Springer research papers)
  3. Software Requirement Specification (SRS) & System Architecture
  4. System Design (UML Class, Sequence, Component, Deployment diagrams)
  5. Implementation Details & Core Algorithm Code
  6. Testing & Validation (Unit, Integration, Performance metrics)
  7. Results, Conclusion & Future Scope

================================================================================
2. TOP 10 FREQUENTLY ASKED UNIVERSITY VIVA QUESTIONS & MODEL ANSWERS
================================================================================

Q1. What is the novelty or key USP of your project compared to existing commercial solutions?
Model Answer:
"Existing systems either lack real-time latency optimization or rely on costly proprietary infrastructure. Our project introduces an event-driven microservices architecture using FastAPI, Docker, and Redis caching that cuts response time by 45% and runs on low-cost commodity hardware with end-to-end JWT security."

Q2. How did you choose your database architecture (SQL vs NoSQL)?
Model Answer:
"For transactional integrity, user authentication, and financial billing records, we chose PostgreSQL to ensure ACID compliance. For sensor telemetry and unstructured log streams, we utilized MongoDB/Redis to support high-throughput write operations with low latency."

Q3. How did you test and secure your project against attacks?
Model Answer:
"We implemented role-based access control (RBAC) with bcrypt password hashing and short-lived JWT access tokens with refresh rotation. Input validation is strictly enforced using Pydantic models to prevent SQL injection and Cross-Site Scripting (XSS). Automated test suites were executed using pytest achieving >85% code coverage."
