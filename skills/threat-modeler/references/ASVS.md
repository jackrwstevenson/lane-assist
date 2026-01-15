# OWASP Application Security Verification Standard (ASVS)

**Version:** 5.0.0 (May 2024)

**Official Sources:**
- Primary Documentation: https://owasp.org/www-project-application-security-verification-standard/
- GitHub Repository: https://github.com/OWASP/ASVS
- Latest Release: https://github.com/OWASP/ASVS/releases/tag/v5.0.0

## Overview

The OWASP ASVS is a framework of ~350 security requirements for designing, developing, and testing modern web applications and web services. It provides a basis for testing technical security controls and requirements for secure development.

## Key Changes in v5.0.0

- **Comprehensive Restructure**: Revised layout and refined requirements
- **Cloud-Native & API-First**: Updated for modern architectures
- **DevOps Integration**: Better alignment with SDLC phases
- **Precise Language**: Clearer requirement definitions
- **New Website**: https://asvs.dev for easy browsing

## Requirement Format

Requirements use hierarchical numbering: `<chapter>.<section>.<requirement>`

Example: `1.2.5` = Chapter 1, Section 2, Requirement 5

## Security Requirement Categories

### Chapter 1: Architecture, Design and Threat Modeling

**Objective:** Secure architecture from the foundation

**Key Requirements:**
- Document security architecture and controls
- Perform threat modeling (STRIDE, PASTA, attack trees)
- Separate tiers physically/logically (presentation, business, data)
- Security controls at appropriate trust boundaries
- Security design patterns (principle of least privilege, defense in depth)

**Common Issues:**
- No threat model or security design documentation
- Business logic in presentation layer
- Trust boundaries not identified
- Shared infrastructure without isolation
- No security review during design phase

---

### Chapter 2: Authentication

**Objective:** Verify users are who they claim to be

**Key Requirements:**
- Passwords stored with bcrypt, scrypt, Argon2, or PBKDF2
- Multi-factor authentication for privileged accounts
- Account lockout after failed attempts (5-10 attempts)
- Password complexity requirements enforced
- Secure password reset process (token-based, time-limited)
- Session management (secure, random session IDs)
- Session timeout (15 minutes for sensitive apps)
- Logout invalidates all tokens
- Resist credential stuffing (rate limiting, CAPTCHA)

**Common Vulnerabilities:**
- Passwords stored with weak hashing (MD5, SHA1, plain text)
- No MFA for admin accounts
- Unlimited login attempts
- Predictable password reset tokens
- Session fixation vulnerabilities
- Sessions don't expire
- Logout doesn't invalidate tokens

**Secure Implementations:**
- Password hashing: bcrypt (cost factor 12+), Argon2id
- Session IDs: 128+ bits of entropy from CSPRNG
- Session storage: HttpOnly, Secure, SameSite cookies
- Rate limiting: 5 attempts per minute per IP/account
- Password reset: Time-limited token (15 min), single-use

---

### Chapter 3: Session Management

**Objective:** Maintain secure user sessions

**Key Requirements:**
- New session ID after authentication
- Session IDs never in URLs
- Logout fully terminates session
- Session timeout enforced server-side
- Concurrent session limits (optional)
- Session cookies: Secure, HttpOnly, SameSite attributes
- Session invalidation on password change
- Protect against session fixation/hijacking

**Common Vulnerabilities:**
- Session ID in URL parameters
- Session cookies without Secure flag
- Missing HttpOnly (vulnerable to XSS)
- No SameSite (CSRF vulnerable)
- Sessions persist after logout
- Reuse of session IDs
- Client-side session timeout only

**Secure Implementations:**
- Cookie flags: `Set-Cookie: session=...; Secure; HttpOnly; SameSite=Strict`
- Session rotation: Generate new ID on privilege changes
- Timeout: Idle timeout (15-30 min), absolute timeout (8-12 hours)
- Storage: Server-side session store (Redis, database)

---

### Chapter 4: Access Control

**Objective:** Enforce authorization and prevent unauthorized access

**Key Requirements:**
- Deny by default (whitelist approach)
- Access control checks on every request
- Authorization enforced server-side
- Prevent Insecure Direct Object References (IDOR)
- Directory traversal protection
- Enforce least privilege
- CORS policy properly configured
- Prevent forced browsing to restricted resources

**Common Vulnerabilities:**
- IDOR: `/api/user/123` accessible to any authenticated user
- Path traversal: `../../../../etc/passwd`
- Client-side authorization checks only
- Missing authorization on API endpoints
- Horizontal privilege escalation
- Vertical privilege escalation
- CORS misconfiguration (wildcard origins with credentials)

**Secure Implementations:**
- Check user owns resource: `if (resource.owner_id != current_user.id) return 403`
- Use indirect references: UUIDs instead of sequential IDs
- Path validation: Reject `..` and absolute paths
- RBAC/ABAC: Role-based or attribute-based access control
- CORS: Whitelist specific origins, avoid `Access-Control-Allow-Origin: *`

---

### Chapter 5: Validation, Sanitization and Encoding

**Objective:** Prevent injection attacks through input handling

**Key Requirements:**
- Validate all input (type, length, format, range)
- Whitelist validation where possible
- Sanitize input before use
- Context-aware output encoding
- Parameterized queries for SQL
- Avoid eval() and dynamic code execution
- Validate file uploads (type, size, content)
- Protect against XML External Entity (XXE)

**Common Vulnerabilities:**
- SQL Injection: `SELECT * FROM users WHERE id = ' + userId`
- XSS: Unescaped user input in HTML
- Command Injection: `exec('ping ' + userInput)`
- Path Traversal: Unvalidated file paths
- XXE: Unrestricted XML parsing
- LDAP, XPath, NoSQL injection
- Server-Side Template Injection (SSTI)
- Server-Side Request Forgery (SSRF)

**Secure Implementations:**
- SQL: Parameterized queries/prepared statements
  ```sql
  SELECT * FROM users WHERE id = ?  -- Bind userId as parameter
  ```
- XSS Prevention: HTML entity encoding, CSP headers
- Command Injection: Avoid shell execution, use native APIs
- File Upload: Validate MIME type, scan for malware, store outside webroot
- XXE: Disable external entities in XML parsers
- SSRF: Whitelist allowed hosts, block private IP ranges

---

### Chapter 6: Stored Cryptography

**Objective:** Protect data at rest using cryptography

**Key Requirements:**
- Industry-standard algorithms only (AES-256, RSA-2048+)
- Secure key management (HSM, key vault, KMS)
- No hardcoded cryptographic keys
- Use authenticated encryption (AES-GCM, ChaCha20-Poly1305)
- Secure random number generation
- Key rotation policies
- Encrypt sensitive data in databases
- Proper initialization vectors (random, unique per encryption)

**Common Vulnerabilities:**
- Weak algorithms: DES, 3DES, RC4, MD5, SHA1
- Hardcoded encryption keys in code
- Reused IVs (deterministic encryption)
- ECB mode (pattern-leaking)
- Weak key derivation
- Keys stored alongside encrypted data
- Insecure random: `Math.random()`, `rand()`

**Secure Implementations:**
- Encryption: AES-256-GCM with random IVs
- Key derivation: PBKDF2-HMAC-SHA256 (100k+ iterations), Argon2
- Key storage: AWS KMS, Azure Key Vault, HashiCorp Vault, HSM
- Random: OS-provided CSPRNG (SecureRandom, crypto.randomBytes)
- At-rest encryption: Database TDE, filesystem encryption

---

### Chapter 7: Error Handling and Logging

**Objective:** Fail securely and detect security incidents

**Key Requirements:**
- Generic error messages to users (no stack traces)
- Detailed error logging for debugging (server-side only)
- Log security events (authentication, authorization failures)
- No sensitive data in logs
- Tamper-resistant logs
- Log injection prevention
- Alerting for critical security events
- Log retention policy

**Common Vulnerabilities:**
- Stack traces exposed to users
- Verbose error messages revealing system details
- Passwords/tokens in logs
- SQL queries with PII in logs
- No logging of security events
- Logs modifiable by application user
- Log injection (CRLF injection)

**Secure Implementations:**
- User-facing: "An error occurred. Please contact support."
- Server logs: Full stack trace + context for debugging
- Security events: Login attempts, permission denials, suspicious activity
- Sanitize: Remove/mask sensitive data before logging
- Structured logging: JSON format prevents injection
- Log to centralized system: SIEM (Splunk, ELK, Datadog)

---

### Chapter 8: Data Protection

**Objective:** Protect sensitive data throughout its lifecycle

**Key Requirements:**
- Classify data by sensitivity
- Encrypt sensitive data at rest and in transit
- No sensitive data in URL parameters or HTTP headers
- Secure data in memory (clear after use)
- No sensitive data in client-side storage
- Proper cache control headers
- Data retention and disposal policies
- Personal data handling (GDPR/CCPA compliance)

**Common Vulnerabilities:**
- Passwords/tokens in URLs
- PII in localStorage/sessionStorage
- Sensitive data in browser cache
- Data cached by CDN indefinitely
- No data deletion mechanism
- Sensitive data in GET requests
- Credit cards stored without PCI-DSS compliance

**Secure Implementations:**
- Transport: TLS 1.2+ for all sensitive data
- Storage: Encrypt PII, credit cards, health data
- URLs: POST for sensitive operations, no secrets in query strings
- Cache headers: `Cache-Control: no-store` for sensitive pages
- Memory: Overwrite sensitive variables, use SecureString
- Client storage: Never store sensitive data, use httpOnly cookies

---

### Chapter 9: Communication

**Objective:** Secure all network communications

**Key Requirements:**
- TLS 1.2+ for all connections
- Strong cipher suites only
- Valid, trusted certificates
- HTTP Strict Transport Security (HSTS)
- Certificate pinning (for mobile/high-security apps)
- No mixed content (HTTP/HTTPS)
- Secure WebSocket connections (WSS)

**Common Vulnerabilities:**
- TLS 1.0/1.1 (deprecated, vulnerable)
- Weak ciphers (RC4, 3DES, export ciphers)
- Self-signed certificates in production
- Missing HSTS header
- Certificate validation disabled
- Mixed content (HTTPS page loading HTTP resources)

**Secure Implementations:**
- TLS: Version 1.2+ with forward secrecy (ECDHE)
- Cipher suites: AES-GCM, ChaCha20-Poly1305
- HSTS: `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`
- Certificate: From trusted CA (Let's Encrypt, DigiCert, etc.)
- Certificate pinning: Pin public key or intermediate CA

---

### Chapter 10: Malicious Code

**Objective:** Prevent malicious code in the application

**Key Requirements:**
- Code review process
- Automated security scanning (SAST, DAST)
- Dependency vulnerability scanning
- Secure development environment
- Code signing
- Integrity checks on deployed code
- Protection against supply chain attacks
- Remove debug code/backdoors from production

**Common Vulnerabilities:**
- Vulnerable dependencies (Log4Shell, Heartbleed)
- Backdoors left in code
- Debug endpoints in production
- No integrity verification
- Unsigned binaries
- Compromised build pipeline
- Easter eggs/undocumented features

**Secure Implementations:**
- Dependency scanning: Snyk, Dependabot, npm audit, OWASP Dependency-Check
- SAST: SonarQube, Semgrep, Checkmarx, Veracode
- DAST: OWASP ZAP, Burp Suite, HCL AppScan
- SCA: Software Composition Analysis for third-party code
- Code signing: Sign releases with trusted certificate
- SBOM: Software Bill of Materials (CycloneDX, SPDX)

---

### Chapter 11: Business Logic

**Objective:** Prevent logic flaws and business rule bypasses

**Key Requirements:**
- Business rules enforced server-side
- Sequential process flows enforced
- Transaction limits and velocity checks
- Prevent resource exhaustion
- Anti-automation (CAPTCHA, rate limiting)
- Time-sensitive operations have deadlines
- Atomic operations to prevent race conditions

**Common Vulnerabilities:**
- Price manipulation in checkout
- Negative quantity in orders
- Race conditions in financial transactions
- Workflow step skipping
- Replay attacks
- Time-of-check to time-of-use (TOCTOU)
- Insufficient anti-automation

**Secure Implementations:**
- Validation: Server-side price verification
- State management: Track workflow state, reject out-of-order steps
- Rate limiting: Limit requests per user/IP (e.g., 100 req/min)
- Idempotency: Use idempotency keys for critical operations
- Transactions: Use database transactions, locks for race conditions
- CAPTCHA: Google reCAPTCHA, hCaptcha for sensitive operations

---

### Chapter 12: File and Resources

**Objective:** Secure file uploads and resource handling

**Key Requirements:**
- Validate file type, size, content
- Sanitize filenames
- Store uploaded files outside webroot
- Anti-malware scanning
- Prevent path traversal in file operations
- Limit file size to prevent DoS
- Generate unique filenames
- Set proper Content-Type headers
- Content Security Policy for uploaded content

**Common Vulnerabilities:**
- Arbitrary file upload (shell upload)
- XXE in XML/SVG uploads
- Path traversal: `../../../etc/passwd`
- Filename injection: `; rm -rf /`
- Executable files served with wrong MIME type
- ZIP bombs, XML bombs (DoS)
- Unrestricted file inclusion (LFI/RFI)

**Secure Implementations:**
- Validation: Check magic bytes, not just extension
- Storage: Outside docroot, randomized names (UUID)
- Serving: Set `Content-Disposition: attachment`, correct `Content-Type`
- Size limits: E.g., 10MB max
- Scanning: ClamAV, VirusTotal API
- Path operations: Canonicalize paths, reject `..`

---

### Chapter 13: API and Web Service

**Objective:** Secure REST, GraphQL, SOAP APIs

**Key Requirements:**
- Authentication on all API endpoints
- Authorization checks per-endpoint
- Rate limiting to prevent abuse
- Input validation for all API parameters
- RESTful URL structure (no verbs in URLs)
- Proper HTTP methods (GET, POST, PUT, DELETE)
- API versioning
- CORS properly configured
- GraphQL query depth/complexity limits

**Common Vulnerabilities:**
- Unauthenticated API endpoints
- Missing rate limiting (API abuse)
- Mass assignment vulnerabilities
- GraphQL query complexity attacks
- No API input validation
- Verbose error messages (info disclosure)
- CORS misconfiguration

**Secure Implementations:**
- Authentication: JWT, OAuth 2.0, API keys
- Rate limiting: 1000 requests/hour per user, 10000/day
- Input validation: JSON schema validation
- CORS: Whitelist origins, avoid wildcards
- GraphQL: Query depth limit (5-10), complexity analysis
- API Gateway: Centralized auth, rate limiting, logging

---

### Chapter 14: Configuration

**Objective:** Ensure secure deployment configuration

**Key Requirements:**
- Remove default credentials
- Disable unnecessary features/endpoints
- Secure admin interfaces
- Error messages don't reveal versions
- Disable directory listing
- Remove test code/endpoints
- Security headers configured
- HTTP methods restricted (only needed ones)
- X-Powered-By header removed

**Common Vulnerabilities:**
- Default admin/admin credentials
- Debug mode in production
- Directory listing enabled
- Test endpoints accessible
- Verbose server headers
- Unnecessary HTTP methods (TRACE, OPTIONS)
- Missing security headers

**Secure Implementations:**
- Headers:
  - `X-Content-Type-Options: nosniff`
  - `X-Frame-Options: DENY`
  - `Content-Security-Policy: default-src 'self'`
  - `Referrer-Policy: no-referrer`
  - `Permissions-Policy: geolocation=(), camera=()`
- Server config: Remove `X-Powered-By`, custom error pages
- Admin: Separate network/IP whitelist, strong auth

---

## Verification Levels (Pre-v5.0 Context)

Historically, ASVS had three levels. While v5.0 restructured this, understanding the concepts helps prioritize:

- **Level 1**: Opportunistic security (basic protection)
- **Level 2**: Standard security (most applications)
- **Level 3**: High assurance (critical applications)

## Testing Approaches

- **SAST** (Static Analysis): Code review, automated scanners
- **DAST** (Dynamic Analysis): Runtime testing, penetration testing
- **IAST** (Interactive): Combines SAST + DAST during testing
- **SCA** (Composition Analysis): Third-party dependency scanning
- **Manual Testing**: Penetration testing, code review

## Tools for ASVS Testing

- **Scanners**: OWASP ZAP, Burp Suite, Acunetix, Nessus
- **SAST**: SonarQube, Semgrep, CodeQL, Checkmarx
- **Dependency**: OWASP Dependency-Check, Snyk, npm audit
- **Fuzzing**: AFL, LibFuzzer, Radamsa

## Compliance Mapping

ASVS helps achieve:
- **PCI-DSS**: Requirements 6.5, 6.6
- **HIPAA**: Technical safeguards
- **SOC 2**: Security trust service criteria
- **ISO 27001**: Information security controls
- **GDPR**: Security of processing (Article 32)

## Additional Resources

- **OWASP Top 10**: Maps to ASVS requirements
- **OWASP SAMM**: Software Assurance Maturity Model
- **OWASP Testing Guide**: Detailed testing procedures
- **CycloneDX**: SBOM format for ASVS compliance tracking

---

*Last Updated: May 2024 (ASVS v5.0.0)*
