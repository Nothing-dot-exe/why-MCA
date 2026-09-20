# Master Study Notes: Cyber Security & DevSecOps
## Course Code: 22MCA33 / MMC303 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: Information Security Fundamentals (CIA Triad, Security Threats, Attacks, Passive vs Active Attacks, Vulnerabilities, Malicious Software - Trojans, Worms, Ransomware, Zero-day).
* **Module 2**: Applied Cryptography (Symmetric Encryption - DES & AES, Asymmetric Cryptography - RSA Algorithm with Numericals, Diffie-Hellman Key Exchange, Hash Functions - SHA-256, HMAC, Digital Signatures, PKI & X.509 Certificates).
* **Module 3**: Network & Application Security (Firewalls - Packet Filtering, Stateful, Next-Gen, IDS/IPS, Virtual Private Networks - IPsec, SSL/TLS Handshake Architecture, OWASP Top 10 Vulnerabilities - SQLi, XSS, CSRF, SSRF, Broken Access Control & Mitigations).
* **Module 4**: DevSecOps Framework & Secure Software Lifecycle (Shifting Security Left, CI/CD Pipeline Security Integration, Static Application Security Testing - SAST, Dynamic Application Security Testing - DAST, Software Composition Analysis - SCA, Container Security - Docker Bench, Trivy, Kubernetes RBAC).
* **Module 5**: Incident Response, Digital Forensics & Cyber Laws (Incident Response Lifecycle - NIST SP 800-61, Digital Evidence Acquisition, Chain of Custody, IT Act 2000 & Amendments, GDPR, ISO/IEC 27001 Compliance).

---

# MODULE 1: INFORMATION SECURITY FUNDAMENTALS

## 1.1 The CIA Triad & Parkerian Hexad
```
              [ Confidentiality ]
                 /           \
                /    C I A    \
               /     TRIAD     \
      [ Integrity ] --------- [ Availability ]
```
1. **Confidentiality**: Ensuring sensitive data is shielded from unauthorized access (Enforced via: AES-256 encryption, access control lists, least privilege).
2. **Integrity**: Guaranteeing data is accurate, consistent, and unaltered throughout its lifecycle (Enforced via: Cryptographic hashes like SHA-256, digital signatures).
3. **Availability**: Ensuring systems and information are accessible to authorized personnel whenever required (Enforced via: High-availability clustering, DDoS mitigation, disaster recovery).
* *Parkerian Hexad Additions*: **Authenticity** (proof of identity), **Utility** (usefulness of data), and **Possession/Control** (physical/logical custody).

## 1.2 Threat vs. Vulnerability vs. Risk
* **Threat**: Any potential occurrence, malicious or accidental, that can harm an asset (e.g., hacker, malware, earthquake).
* **Vulnerability**: A flaw or weakness in system security procedures, design, or implementation that can be exploited by a threat (e.g., unpatched Apache Log4j, buffer overflow).
* **Exploit**: A piece of software, data chunk, or sequence of commands that takes advantage of a vulnerability.
* **Risk**: The probability and operational impact of a threat exploiting a vulnerability:
  $$\text{Risk} = \text{Threat} \times \text{Vulnerability} \times \text{Impact (Asset Value)}$$

---

# MODULE 2: APPLIED CRYPTOGRAPHY

## 2.1 Symmetric vs. Asymmetric Cryptography
| Metric | Symmetric Cryptography | Asymmetric Cryptography |
| :--- | :--- | :--- |
| **Keys** | Single shared secret key for encryption & decryption | Key pair: Public key (encrypt) + Private key (decrypt) |
| **Algorithms** | AES (128/256-bit), 3DES, ChaCha20 | RSA (2048/4096-bit), ECC (Elliptic Curve), Diffie-Hellman |
| **Performance** | Extremely fast; suited for bulk payload encryption | Computationally heavy (~1000x slower than AES) |
| **Key Distribution**| Challenging: requires a secure out-of-band channel | Simple: Public key is freely distributed; private key kept secret |

## 2.2 RSA Algorithm (Rivest-Shamir-Adleman) — Step-by-Step
1. Select two distinct prime numbers: $p$ and $q$.
2. Compute modulus: $n = p \times q$.
3. Compute Euler's Totient function: $\phi(n) = (p - 1)(q - 1)$.
4. Choose public exponent $e$ such that $1 < e < \phi(n)$ and $\gcd(e, \phi(n)) = 1$.
5. Compute private exponent $d$ such that:
   $$d \cdot e \equiv 1 \pmod{\phi(n)} \quad \Longleftrightarrow \quad d = e^{-1} \pmod{\phi(n)}$$
6. **Encryption**: Given plaintext message $M < n$:
   $$C = M^e \pmod n$$
7. **Decryption**: Given ciphertext $C$:
   $$M = C^d \pmod n$$

---

# MODULE 3: NETWORK & APPLICATION SECURITY (OWASP TOP 10)

## 3.1 SSL/TLS 1.3 Handshake Protocol
```
Client                                                   Server
  |                                                        |
  | -------- 1. ClientHello (CipherSuites, KeyShare) ----> |
  |                                                        |
  | <------- 2. ServerHello (SelectedSuite, KeyShare) ---- |
  | <------- EncryptedExtensions, Certificate ------------ |
  | <------- CertificateVerify, Finished ----------------- |
  |                                                        |
  | [Both derive symmetric Session Keys using ECDHE]       |
  |                                                        |
  | -------- 3. Finished --------------------------------> |
  |                                                        |
  | <======= 4. Encrypted Application Data (AES-GCM) =====>|
```

## 3.2 OWASP Top 10 Core Attacks & Fixes
* **SQL Injection (SQLi)**: Attacker injects malicious SQL statements into user inputs.
  * *Vulnerable*: `query = "SELECT * FROM users WHERE user = '" + input + "'"`
  * *Remediation*: **Parameterized Queries / Prepared Statements**:
    ```python
    cursor.execute("SELECT * FROM users WHERE user = %s AND pass = %s", (username, password))
    ```
* **Cross-Site Scripting (XSS)**: Malicious JavaScript executed in victim's browser session.
  * *Stored XSS*: Injected script permanently stored in DB.
  * *Reflected XSS*: Script reflected off web server in search results/URL parameters.
  * *Remediation*: Context-aware output encoding (HTML escaping) and implementing **Content Security Policy (CSP)** HTTP header.
* **Cross-Site Request Forgery (CSRF)**: Forcing an authenticated user to execute unwanted actions.
  * *Remediation*: Anti-CSRF Synchronizer Tokens & `SameSite=Strict` cookie flags.

---

# MODULE 4: DEVSECOPS FRAMEWORK & SECURE CI/CD

## 4.1 DevSecOps "Shift-Left" Pipeline
```
 [ PLAN & CODE ]  -->  [ BUILD & TEST ]  -->  [ RELEASE & DEPLOY ]  -->  [ MONITOR ]
       |                      |                       |                       |
 Pre-commit Hooks        SAST Analysis           DAST Scanning           SIEM Logging
 Secret Scanning        SCA (Dependencies)      Container Auditing      WAF Monitoring
 (TruffleHog)           (SonarQube, Snyk)       (OWASP ZAP, Trivy)      (Splunk, ELK)
```

## 4.2 Automated Security Pipeline Tools
1. **SAST (Static Application Security Testing)**: White-box static code analysis without executing the program (e.g., SonarQube, Semgrep).
2. **SCA (Software Composition Analysis)**: Scans third-party packages and open-source libraries for known vulnerabilities (CVEs) in `package.json` or `pom.xml` (e.g., Snyk, Dependabot).
3. **DAST (Dynamic Application Security Testing)**: Black-box testing of running applications to uncover runtime security flaws (e.g., OWASP ZAP, Burp Suite Enterprise).
4. **Container & IaC Security**: Scans Docker images and Terraform scripts for configuration drift and base-image vulnerabilities (e.g., Trivy, Checkov).

---

# MODULE 5: INCIDENT RESPONSE & CYBER FORENSICS

## 5.1 NIST Incident Response Lifecycle (NIST SP 800-61)
1. **Preparation**: Establishing incident handling policies, training CSIRT team, deploying EDR agents.
2. **Detection & Analysis**: Identifying deviations from baselines via IDS alerts, SIEM logs, and anomaly detection.
3. **Containment, Eradication & Recovery**:
   * *Short-term Containment*: Isolating affected subnets, revoking compromised API credentials.
   * *Eradication*: Removing malware artifacts, disabling backdoors.
   * *Recovery*: Restoring clean systems from known good backups, validating system integrity.
4. **Post-Incident Activity (Lessons Learned)**: Root Cause Analysis (RCA), updating prevention mechanisms.

## 5.2 Chain of Custody in Digital Forensics
* Chronological documentation recording the collection, transfer, analysis, and disposition of physical and electronic evidence.
* **Order of Volatility (RFC 3227)**:
  1. CPU registers & cache
  2. Routing tables, ARP cache, process table, kernel memory
  3. Physical RAM (Memory dump)
  4. Temporary file systems (`/tmp`, swap space)
  5. Disk / Non-volatile storage
  6. Remote logging data & archival media

---

# 🎯 High-Yield VTU Exam Solved Questions (10-Mark Model Answers)

### Question 1: RSA Numerical (VTU Dec 2023 / Jan 2024 - 10 Marks)
**Given $p = 7$, $q = 11$, and encryption key $e = 13$:**
1. Compute the private key $d$.
2. Encrypt plaintext message $M = 9$.
3. Verify by decrypting ciphertext $C$ back to $M$.

**Step-by-Step Solution:**
1. **Compute Modulus ($n$)**:
   $$n = p \times q = 7 \times 11 = 77$$
2. **Compute Totient ($\phi(n)$)**:
   $$\phi(n) = (p - 1)(q - 1) = (6)(10) = 60$$
3. **Verify $e$**: $\gcd(13, 60) = 1$ (Valid public key).
4. **Calculate Private Key ($d$)**:
   $$d \times e \equiv 1 \pmod{\phi(n)} \implies 13d \equiv 1 \pmod{60}$$
   Using Extended Euclidean Algorithm:
   $$60 = 13(4) + 8 \implies 8 = 60 - 13(4)$$
   $$13 = 8(1) + 5 \implies 5 = 13 - 8(1)$$
   $$8 = 5(1) + 3 \implies 3 = 8 - 5(1)$$
   $$5 = 3(1) + 2 \implies 2 = 5 - 3(1)$$
   $$3 = 2(1) + 1 \implies 1 = 3 - 2(1)$$
   Substituting back: $13(37) = 481 = 8 \times 60 + 1 \equiv 1 \pmod{60}$.
   $$\mathbf{d = 37}$$
5. **Encryption ($C = M^e \pmod n$)**:
   $$C = 9^{13} \pmod{77}$$
   Using modular exponentiation:
   * $9^2 = 81 \equiv 4 \pmod{77}$
   * $9^4 = 4^2 = 16 \pmod{77}$
   * $9^8 = 16^2 = 256 = 3 \times 77 + 25 \equiv 25 \pmod{77}$
   * $9^{13} = 9^8 \times 9^4 \times 9^1 = 25 \times 16 \times 9 \pmod{77}$
   * $25 \times 16 = 400 = 5 \times 77 + 15 \equiv 15 \pmod{77}$
   * $C = 15 \times 9 = 135 = 1 \times 77 + 58 \equiv \mathbf{58} \pmod{77}$
   $$\mathbf{C = 58}$$
6. **Decryption ($M = C^d \pmod n$)**:
   $$M = 58^{37} \pmod{77} = \mathbf{9} \quad \text{(Verified verified identical to original plaintext)}$$

---

### Question 2: DevSecOps vs. Traditional DevOps (10 Marks)
* Explain the paradigm shift from traditional DevOps to DevSecOps, detailing 4 key automated security gates in a modern Jenkins/GitLab CI pipeline.

**Model Answer:**
1. **Core Distinction**:
   * *DevOps*: Prioritizes velocity and rapid continuous delivery (CI/CD); security is treated as an isolated post-production gatekeeper, resulting in delayed deployments and costly vulnerability patches.
   * *DevSecOps*: Embeds security governance seamlessly into every phase of the software delivery lifecycle as shared code ("Security as Code"), catching vulnerabilities at commit-time ("Shift Left").
2. **Automated Pipeline Gates**:
   * **Gate 1 (Pre-commit Secret Scanning)**: Tool like GitGuardian or TruffleHog checks git commits for hardcoded AWS access keys, JWT secrets, or DB credentials. Blocks commit if high-entropy secrets are detected.
   * **Gate 2 (Static Code Quality & SAST)**: SonarQube analyzes AST for OWASP Top 10 flaws (e.g. unescaped SQL queries). Fails CI build if "Blocker" or "Critical" vulnerabilities exceed 0.
   * **Gate 3 (SCA Dependency Audit)**: Snyk or OWASP Dependency-Check analyzes `package-lock.json` against the National Vulnerability Database (NVD). Halts build if packages have CVSS $\ge 7.0$ (High/Critical CVEs).
   * **Gate 4 (Container Vulnerability Scan)**: Trivy scans Docker container images before pushing to Amazon ECR/Docker Hub, validating base OS package security.
