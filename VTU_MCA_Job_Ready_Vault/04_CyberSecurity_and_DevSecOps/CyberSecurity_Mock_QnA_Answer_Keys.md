# Cyber Security & DevSecOps Mock Interview Q&A & Answer Keys

---

## 🎯 Question 1: What is the difference between Symmetric vs Asymmetric Encryption, and how are both combined in HTTPS (TLS 1.3)?
* **Category**: Applied Cryptography

### 💡 Answer Key
1. **Symmetric Encryption (e.g. AES-256-GCM)**:
   - Uses the **same secret key** for both encryption and decryption.
   - Extremely fast, hardware-accelerated (AES-NI instructions).
   - Problem: Secure key exchange problem (how to share the secret key over an untrusted network).
2. **Asymmetric Encryption (e.g. RSA-4096, ECDH / Curve25519)**:
   - Uses a mathematically related **Public Key** (for encryption/verification) and **Private Key** (for decryption/signing).
   - Solves key exchange, but computationally expensive (roughly 1,000x slower than symmetric).
3. **Hybrid Cryptography in HTTPS (TLS 1.3)**:
   - **Phase 1 (Key Exchange)**: The client and server use Asymmetric Cryptography (**Elliptic Curve Diffie-Hellman Ephemeral - ECDHE**) and Digital Certificates (X.509) to mutually authenticate and securely negotiate a shared secret key without ever transmitting the secret over the wire.
   - **Phase 2 (Data Transfer)**: Once the shared secret is established, all application payload data is encrypted using Symmetric Cryptography (**AES-256-GCM or ChaCha20-Poly1305**), achieving both maximum security and line-rate performance.

---

## 🎯 Question 2: Explain Insecure Direct Object References (IDOR). How would you detect and fix it in an enterprise REST API?
* **Category**: Application Security / Penetration Testing

### 💡 Answer Key
* **Definition**: An IDOR occurs when an application exposes a reference to an internal implementation object (such as a database primary key integer ID) in an API endpoint, and fails to verify whether the requesting authenticated user owns or has permission to access that object.
* **Attack Scenario**:
  - Attacker logs in as User A (`id = 105`).
  - Attacker requests: `GET /api/v1/invoices/999`.
  - If the server checks only `if (user.isAuthenticated())` and runs `SELECT * FROM invoices WHERE id = 999`, the attacker views a stranger's sensitive invoice.
* **Fix & Remediation**:
  1. **Enforce Tenant / User Ownership Scope**:
     ```python
     # SECURE: Always bind query to logged-in user context
     query = "SELECT * FROM invoices WHERE id = %s AND user_id = %s"
     cursor.execute(query, (invoice_id, request.user.id))
     ```
  2. **Use UUIDs / Non-enumerable Identifiers**: Replace sequential auto-incrementing integer IDs (`1, 2, 3...`) with random UUIDv4 (`e.g., 550e8400-e29b-41d4-a716-446655440000`) to eliminate trivial enumeration scripting.
  3. **Role-Based Access Control (RBAC) Interceptors**: Apply middleware checking policy permissions before hitting controller logic.

---

## 🎯 Question 3: How does Cross-Origin Resource Sharing (CORS) work? Is CORS a client-side or server-side security mechanism?
* **Category**: Web Security Architecture

### 💡 Answer Key
* **Core Insight**: **CORS is enforced strictly by the BROWSER (Client-side)**, not by the server or curl.
* **Mechanism**:
  - The Same-Origin Policy (SOP) prevents a script running on `https://evil.com` from reading response data from `https://mybank.com`.
  - When a web app makes a cross-origin HTTP request with custom headers or methods (`PUT`, `DELETE`), the browser automatically sends an HTTP `OPTIONS` **Preflight Request**.
  - The server responds with headers:
    * `Access-Control-Allow-Origin: https://app.example.com` (Never set `*` if `Allow-Credentials: true` is enabled).
    * `Access-Control-Allow-Methods: GET, POST, PUT, DELETE`.
    * `Access-Control-Allow-Headers: Content-Type, Authorization`.
  - If the browser does not see a matching `Access-Control-Allow-Origin`, it **blocks the response from reaching JavaScript code**.
