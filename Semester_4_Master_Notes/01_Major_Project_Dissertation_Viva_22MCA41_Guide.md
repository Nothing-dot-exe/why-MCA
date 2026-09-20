# Master Guide: Major Project, Dissertation & External Viva-Voce Defense
## Course Code: 22MCA41 / MMC401 (12-16 Credits) | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Course Architecture & Evaluation Scheme

* **Total Credits**: 12 to 16 Credits (Heaviest weighted course in the entire MCA curriculum).
* **Marks Distribution**: Total 100 Marks (Continuous Internal Evaluation - CIE: 50 Marks | Semester End Examination - SEE External Viva: 50 Marks).
* **VTU Evaluation Panel**: Conducted jointly by one **Internal Examiner** (College Project Guide) and one **External Examiner** (Appointed by VTU Registrar Evaluation).

---

# SECTION 1: VTU PROJECT REPORT STANDARDIZED TEMPLATE

Every VTU MCA Project Dissertation must strictly adhere to the following chapter organization format:

```
[ FRONT MATTER ]
1. Title Page (VTU Approved Template, BKIT College Crest, Student USN, Guide Name & Designation)
2. Certificate from College (Signed by Guide, HOD, and Principal)
3. Certificate from Industry/Company (Mandatory for sponsored/internship projects on official letterhead)
4. Declaration by the Student (Plagiarism clearance)
5. Acknowledgements
6. Abstract / Executive Summary (Max 300 words with 5-6 Keywords)
7. Table of Contents with Page Numbers
8. List of Figures (with Figure Number, Caption, and Page Number)
9. List of Tables
10. List of Abbreviations / Acronyms

[ MAIN BODY CHAPTERS ]
CHAPTER 1: INTRODUCTION
  1.1 Background and Domain Overview
  1.2 Problem Statement & Motivation
  1.3 Project Objectives (Specific, Measurable, Achievable)
  1.4 Scope and Limitations
  1.5 Organization of the Report

CHAPTER 2: LITERATURE SURVEY
  2.1 Review of Existing Systems (Minimum 10 IEEE/Springer/ScienceDirect papers)
  2.2 Comparative Analysis Table (Authors, Year, Methodology, Strengths, Limitations)
  2.3 Research Gap & Proposed Novelty

CHAPTER 3: SYSTEM REQUIREMENTS & FEASIBILITY ANALYSIS
  3.1 Functional Requirements (FR)
  3.2 Non-Functional Requirements (NFR: Latency, Scalability, Security, Throughput)
  3.3 Hardware Requirements & Deployment Environment
  3.4 Software Requirements (OS, Frameworks, Languages, Databases, APIs)
  3.5 Feasibility Study (Technical, Operational, Economic)

CHAPTER 4: SYSTEM DESIGN & ARCHITECTURE
  4.1 High-Level Architecture Diagram (Component & Tier Layout)
  4.2 Data Flow Diagrams (DFD Level 0, Level 1, Level 2)
  4.3 UML Diagrams (Use Case, Class Diagram, Sequence Diagram, Activity Diagram)
  4.4 Database Schema & Entity-Relationship (ER) Diagram (3NF Normalized)

CHAPTER 5: IMPLEMENTATION & CODE METHODOLOGY
  5.1 Algorithmic Formulations (Pseudocode and Flowcharts)
  5.2 Core Module Descriptions & API Endpoints
  5.3 Security & Authentication Mechanisms (JWT, OAuth2, RBAC)
  5.4 Code Snippets (Key algorithmic logic only, max 2-3 pages)

CHAPTER 6: SYSTEM TESTING & VALIDATION
  6.1 Testing Methodologies (Unit Testing, Integration Testing, System Testing)
  6.2 Test Cases & Results (Tabular format: Test ID, Input, Expected Output, Actual Output, Status)
  6.3 Performance Benchmarking & Evaluation Metrics (Accuracy, Latency, Throughput)

CHAPTER 7: RESULTS AND DISCUSSION
  7.1 System Screenshots with Descriptive Annotations
  7.2 Quantitative & Qualitative Results
  7.3 Comparative Analysis with Baseline Models

CHAPTER 8: CONCLUSION & FUTURE ENHANCEMENTS
  8.1 Project Conclusion & Summary of Deliverables
  8.2 Limitations Encountered
  8.3 Future Work & Roadmap

[ BACK MATTER ]
* References (Strict IEEE Citation Style: [1], [2], ...)
* Appendix A: User Manual / API Documentation (Swagger Specs)
* Appendix B: Plagiarism Report (Turnitin / DrillBit < 20% similarity index)
```

---

# SECTION 2: TOP 20 VTU EXTERNAL VIVA-VOCE QUESTIONS & MODEL ANSWERS

### Q1: What is the primary problem statement of your project and why is it significant?
* **Model Answer**: State the exact pain point concisely without technical jargon first, followed by the societal or industrial impact. Example: *"Traditional hospital triage relies on manual scoring which delays ICU admissions by 45 minutes on average. Our project automates real-time patient risk stratification using an ensemble XGBoost model deployed on edge nodes, reducing triage latency to under 3 seconds while maintaining 94.2% sensitivity."*

### Q2: What is the novel contribution of your project compared to existing solutions found in your literature survey?
* **Model Answer**: Highlight the exact gap identified in existing IEEE papers and how your design addresses it. (e.g., lower compute latency, improved hybrid architecture, privacy-preserving federated training, or optimized microservice caching).

### Q3: Why did you choose this specific database (e.g., PostgreSQL vs. MongoDB)?
* **Model Answer**: Justify via data structure and access patterns. *"We chose PostgreSQL because our core billing and appointment data requires strict ACID compliance and relational integrity with multi-table joins. However, for real-time unstructured sensor telemetry, we leveraged Redis as an in-memory cache, achieving sub-millisecond read latency."*

### Q4: Explain the architectural design pattern used in your application.
* **Model Answer**: Mention the pattern (e.g., Microservices, MVC, Hexagonal/Clean Architecture, Event-Driven with Kafka). Explain how decoupling was achieved and how services communicate (e.g., RESTful JSON APIs, gRPC, or async message queues).

### Q5: What were the most significant non-functional requirements (NFRs) and how did you measure them?
* **Model Answer**: Mention security (TLS 1.3, AES-256 at rest), latency (P99 response times under 200ms tested via Apache JMeter with 500 concurrent users), and availability (health check probes with automatic Kubernetes restart).

### Q6: How did you test your system and handle edge cases?
* **Model Answer**: Detail Unit testing (JUnit/Pytest with >80% line coverage), Integration testing (Mocking DB calls with Mockito/unittest.mock), and API testing (Postman collections validating HTTP 400, 401, 404, and 500 responses).

### Q7: If your database crashes or the server goes down, how does your system recover?
* **Model Answer**: Explain automated backup snapshots, WAL (Write-Ahead Logging) replay, container restart policies (`restart: always` in Docker), and health check endpoints.

### Q8: What is your plagiarism percentage and which tool was used?
* **Model Answer**: Mention the exact report generated through VTU-mandated software (Turnitin / DrillBit), confirming similarity index is strictly below the 20% VTU threshold.

---

# SECTION 3: VIVA-VOCE DEFENSE CHECKLIST

* [ ] Working, live software demo hosted on cloud or localhost with pre-loaded realistic sample data.
* [ ] Minimum 3 spiral-bound or hard-bound copies signed by Guide, HOD, and Principal.
* [ ] 12-15 slide presentation highlighting: Title $\to$ Motivation $\to$ Architecture $\to$ Demo Screenshots $\to$ Test Cases $\to$ Results $\to$ References.
* [ ] Codebase clean on GitHub with well-documented `README.md` and Docker setup script.
