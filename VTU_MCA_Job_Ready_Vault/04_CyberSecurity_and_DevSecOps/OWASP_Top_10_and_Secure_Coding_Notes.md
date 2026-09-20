# OWASP Top 10 Vulnerabilities & Secure Coding Notes
**Reference**: OWASP Top 10 Standard & Enterprise Application Security Guidelines.

---

## 1. The OWASP Top 10 Security Risks Breakdown

| Rank | Vulnerability Name | Root Cause | Real-World Attack Scenario | Defensive Code Mitigation |
| :--- | :--- | :--- | :--- | :--- |
| **A01** | **Broken Access Control** | Missing authorization checks on objects/APIs | Changing `GET /api/orders/102` to `103` reveals another user's invoice (IDOR). | Enforce object-level ownership checks: `SELECT * FROM orders WHERE id = $1 AND user_id = $2`. |
| **A02** | **Cryptographic Failures** | Weak algorithms (MD5, SHA1) or plain HTTP | Intercepting credentials over unsecured Wi-Fi. | Force TLS 1.3, use Argon2id / bcrypt for passwords, AES-256-GCM for data at rest. |
| **A03** | **Injection (SQLi / Command)** | Untrusted user input treated as executable syntax | Entering `' OR 1=1 --` into username bypasses authentication. | Strictly use Parameterized Queries / Prepared Statements; never concatenate strings. |
| **A04** | **Insecure Design** | Architectural flaws lacking threat modeling | Unlimited login attempts enable credential stuffing. | Implement rate limiting, CAPTCHA, exponential backoff, and MFA. |
| **A05** | **Security Misconfiguration** | Default passwords, verbose debug stack traces | Exposing `/actuator/env` or `DEBUG=True` leaking DB passwords. | Disable debug modes in production, harden cloud bucket ACLs, strip server banner headers. |
| **A06** | **Vulnerable Components** | Outdated NPM/Pip/Maven packages with CVEs | Exploitation of unpatched Log4j (Log4Shell) or Lodash prototype pollution. | Automated CI scanning with `npm audit`, `trivy`, and Dependabot. |
| **A07** | **Identification & Auth Failures** | Permitting weak passwords, session fixation | Session ID does not rotate upon login; attacker hijacks pre-auth session. | Re-generate session IDs on login, enforce password entropy, rotate JWTs regularly. |
| **A08** | **Software & Data Integrity** | Deserializing untrusted data, unverified plugins | Attacker injects malicious serialized Python pickle object triggering Remote Code Execution (RCE). | Avoid native deserialization (`pickle`, Java `readObject`); use JSON with schema validation. |
| **A09** | **Logging & Monitoring Failures** | Breaches go undetected for months | Attacker probes admin endpoints with zero alert or audit trail. | Log security events (login fail, role escalation) with centralized SIEM (Wazuh/Splunk). |
| **A10** | **Server-Side Request Forgery (SSRF)** | Server fetches user-supplied URL without filtering | Attacker passes `http://169.254.169.254/latest/meta-data/` to extract AWS IAM credentials. | Restrict outgoing webhooks, block private IP ranges (`10.0.0.0/8`, `127.0.0.0/8`, link-local). |

---

## 2. Insecure vs Secure Code Examples

### Example A: SQL Injection Remediation (Python)
```python
# ❌ VULNERABLE (Vulnerable to SQL Injection)
def get_user_bad(cursor, user_input):
    query = f"SELECT * FROM users WHERE username = '{user_input}'"
    cursor.execute(query) # Attacker inputs: admin' --
    return cursor.fetchall()

# ✅ SECURE (Parameterized Query)
def get_user_good(cursor, user_input):
    query = "SELECT * FROM users WHERE username = %s"
    cursor.execute(query, (user_input,)) # Input is treated strictly as literal data
    return cursor.fetchall()
```

### Example B: Secure Password Hashing (Python / Bcrypt)
```python
import bcrypt

# Hash password with salt work factor 12
def hash_password(plain_text_password: str) -> str:
    salt = bcrypt.gensalt(rounds=12)
    hashed = bcrypt.hashpw(plain_text_password.encode('utf-8'), salt)
    return hashed.decode('utf-8')

# Verify during authentication
def verify_password(plain_text_password: str, hashed_password: str) -> bool:
    return bcrypt.checkpw(plain_text_password.encode('utf-8'), hashed_password.encode('utf-8'))
```
