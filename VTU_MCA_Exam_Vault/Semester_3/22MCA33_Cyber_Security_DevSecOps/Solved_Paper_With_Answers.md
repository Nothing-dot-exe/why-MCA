# VTU MCA 2022/2024 Scheme - Cyber Security & DevSecOps (22MCA33)
## Full Solved Examination Paper with Cryptography, CI/CD Security & OWASP Top 10
Time: 3 Hours | Max Marks: 100

================================================================================
MODULE 1: CRYPTOGRAPHY & ASYMMETRIC ENCRYPTION (RSA)
================================================================================

Q.1 (a) Explain RSA Algorithm. Given prime numbers p = 7, q = 11, and public exponent e = 13:
(i) Calculate modulus n and Euler totient phi(n).
(ii) Compute private key d.
(iii) Encrypt plaintext M = 9 and decrypt the resulting ciphertext. [10 Marks]
Answer:
Step 1: Compute n and phi(n):
n = p * q = 7 * 11 = 77.
phi(n) = (p - 1) * (q - 1) = (7 - 1) * (11 - 1) = 6 * 10 = 60.

Step 2: Calculate private key d:
d * e = 1 (mod phi(n)) => 13 * d = 1 (mod 60).
Using Extended Euclidean Algorithm:
13 * 37 = 481 = (8 * 60) + 1 = 1 (mod 60).
Hence, private key d = 37.

Step 3: Encryption of M = 9:
Ciphertext C = M^e mod n = 9^13 mod 77.
9^2 = 81 = 4 (mod 77)
9^4 = 4^2 = 16 (mod 77)
9^8 = 16^2 = 256 = 25 (mod 77)
9^13 = 9^8 * 9^4 * 9^1 = 25 * 16 * 9 (mod 77)
25 * 16 = 400 = 15 (mod 77)
15 * 9 = 135 = 58 (mod 77).
Ciphertext C = 58.

Step 4: Decryption:
Plaintext M = C^d mod n = 58^37 mod 77 = 9. (Successfully Decrypted!)

--------------------------------------------------------------------------------
Q.1 (b) Explain DevSecOps Pipeline Integration (SAST, DAST, SCA, Container Scanning). [10 Marks]
Answer:
DevSecOps embeds security testing across the entire CI/CD lifecycle (Shift-Left Security):
1. Code Phase (Pre-commit): Git secrets scanning (TruffleHog, Gitleaks) prevents credential leaks.
2. Build Phase (SAST & SCA):
   - SAST (Static Application Security Testing): Scans source code for vulnerabilities (SonarQube).
   - SCA (Software Composition Analysis): Scans third-party dependencies for CVEs (Snyk, OWASP Dependency Check).
3. Package Phase: Scans container images for base image vulnerabilities (Trivy, Grype).
4. Test / Deploy Phase (DAST): Dynamic testing on running application endpoints (OWASP ZAP).
