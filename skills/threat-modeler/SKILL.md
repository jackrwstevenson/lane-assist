---
name: threat-modeler
description: Scan projects for security threats and generate countermeasures based on OWASP MASVS (mobile apps) and OWASP ASVS (web apps/APIs). Performs systematic security analysis and provides actionable remediation steps.
---

# Threat Modeler

## Overview

This skill performs systematic security threat modeling for software projects. It identifies vulnerabilities based on OWASP standards and provides specific, actionable countermeasures.

**Standards Used:**
- **OWASP MASVS v2.1.0** (January 2025) — Mobile Application Security Verification Standard
- **OWASP ASVS v5.0.0** (May 2024) — Application Security Verification Standard for web apps and APIs

## When to Use

- Security audit of existing codebase
- Pre-deployment security review
- Threat assessment for new features
- User mentions "security", "threats", "vulnerabilities", or "threat model"

## Threat Modeling Process

### Phase 1: Project Classification

1. **Identify Application Type** — Determine which standard(s) apply:
   - **Mobile app** (iOS, Android, React Native, Flutter, etc.) → Use MASVS
   - **Web application** → Use ASVS
   - **API/Backend service** → Use ASVS
   - **Hybrid** (mobile + backend) → Use both MASVS and ASVS

2. **Map Technology Stack** — Document:
   - Languages and frameworks
   - Authentication mechanisms
   - Data storage approaches
   - Network communication patterns
   - Third-party dependencies
   - Platform-specific features

### Phase 2: Systematic Security Scan

Analyze the codebase against relevant OWASP categories. For each category, examine code systematically.

#### For Mobile Apps (MASVS v2.1.0)

Scan against these control groups:

1. **MASVS-STORAGE** — Secure storage of sensitive data on device (data-at-rest)
   - Hardcoded secrets, API keys, credentials
   - Insecure local storage (SharedPreferences, UserDefaults, files)
   - Lack of encryption for sensitive data
   - Logs containing sensitive information

2. **MASVS-CRYPTO** — Cryptographic functionality protecting sensitive data
   - Weak algorithms (MD5, SHA1, DES, RC4)
   - Hardcoded encryption keys
   - Insecure random number generation
   - Custom/broken crypto implementations

3. **MASVS-AUTH** — Authentication and authorization mechanisms
   - Weak password policies
   - Insecure session management
   - Missing multi-factor authentication
   - Authorization bypass vulnerabilities
   - Token storage issues

4. **MASVS-NETWORK** — Secure network communication (data-in-transit)
   - Missing TLS/SSL
   - Certificate validation disabled
   - Insecure protocols (HTTP, FTP)
   - Man-in-the-middle vulnerabilities
   - Certificate pinning missing (for sensitive apps)

5. **MASVS-PLATFORM** — Secure interaction with platform and other apps
   - Insecure IPC (Inter-Process Communication)
   - WebView vulnerabilities
   - Unsafe deep linking
   - Excessive permissions
   - Insecure data sharing between apps

6. **MASVS-CODE** — Security best practices for data processing
   - Input validation failures
   - Injection vulnerabilities
   - Insecure deserialization
   - Memory corruption risks
   - Outdated dependencies with known CVEs

7. **MASVS-RESILIENCE** — Resilience to reverse engineering and tampering
   - Lack of code obfuscation (when needed)
   - Missing root/jailbreak detection (for sensitive apps)
   - No integrity checks
   - Debug symbols in production builds

8. **MASVS-PRIVACY** — Privacy controls protecting user privacy
   - Excessive data collection
   - Missing privacy disclosures
   - Lack of user consent mechanisms
   - Tracking without user knowledge
   - Data retention issues

#### For Web Apps/APIs (ASVS v5.0.0)

Scan against ASVS chapters. Common critical areas include:

1. **Authentication** — User identity verification
   - Password storage (must use bcrypt, scrypt, Argon2)
   - Session management vulnerabilities
   - Missing account lockout
   - Credential stuffing risks

2. **Access Control** — Authorization and permission enforcement
   - Insecure direct object references (IDOR)
   - Missing authorization checks
   - Path traversal vulnerabilities
   - Privilege escalation risks

3. **Input Validation** — Protection against injection attacks
   - SQL injection vulnerabilities
   - Cross-Site Scripting (XSS)
   - Command injection
   - XML/JSON injection
   - Server-Side Request Forgery (SSRF)

4. **Cryptography** — Secure cryptographic operations
   - Weak algorithms or key sizes
   - Insecure key management
   - Missing encryption for sensitive data
   - Improper certificate validation

5. **Error Handling & Logging** — Secure error management
   - Information disclosure in error messages
   - Stack traces exposed to users
   - Insufficient logging for security events
   - Logs containing sensitive data

6. **Data Protection** — Secure data handling
   - Sensitive data in URLs or logs
   - Missing encryption at rest
   - Insecure backup mechanisms
   - Data leakage through caches

7. **Communications** — Secure network interactions
   - Missing HTTPS enforcement
   - Weak TLS configuration
   - Missing security headers (CSP, HSTS, X-Frame-Options)
   - CORS misconfigurations

8. **Business Logic** — Application-specific security flaws
   - Race conditions
   - Business rule bypasses
   - State manipulation vulnerabilities

9. **File & Resource Handling** — Secure file operations
   - Unrestricted file uploads
   - Path traversal in file operations
   - XXE (XML External Entity) vulnerabilities
   - Unsafe file parsing

10. **Configuration** — Secure deployment settings
    - Default credentials
    - Unnecessary features enabled
    - Directory listing enabled
    - Debug mode in production

### Phase 3: Threat Assessment

For each identified issue:

1. **Document the Threat**
   - Location (file path and line numbers)
   - OWASP category
   - Severity (Critical, High, Medium, Low)
   - Attack vector description
   - Potential impact

2. **Prioritize by Risk**
   - Critical: Directly exploitable, high impact
   - High: Exploitable with some effort, significant impact
   - Medium: Requires specific conditions, moderate impact
   - Low: Difficult to exploit or minimal impact

### Phase 4: Countermeasures

For each threat, provide:

1. **Specific Remediation Steps** — Concrete code changes needed
2. **Code Examples** — Show secure implementations
3. **OWASP References** — Link to specific MASVS/ASVS controls
4. **Testing Guidance** — How to verify the fix

## Output Format

Create a structured threat model report:

```markdown
# Security Threat Model — [Project Name]

## Executive Summary

- Application Type: [Mobile/Web/API/Hybrid]
- Standards Applied: [MASVS v2.1.0 / ASVS v5.0.0]
- Total Threats Identified: [Count by severity]
- Critical Issues: [Count]
- High Priority Issues: [Count]

## Findings

### [SEVERITY]: [Threat Title]

**Category:** [OWASP Category]
**Location:** `file_path:line_number`
**CWE:** [If applicable]

**Description:**
[What the vulnerability is]

**Attack Vector:**
[How it could be exploited]

**Impact:**
[What could happen if exploited]

**Remediation:**
[Specific steps to fix]

**Secure Code Example:**
```[language]
[Show correct implementation]
```

**Testing:**
[How to verify the fix]

**References:**
- OWASP [MASVS/ASVS] Control: [Control ID]
- [Additional references]

---

[Repeat for each finding]

## Recommendations Summary

### Immediate Actions (Critical/High)
1. [Action item with file references]
2. [Action item with file references]

### Medium-Term Improvements (Medium)
1. [Action item]
2. [Action item]

### Long-Term Enhancements (Low)
1. [Action item]
2. [Action item]

## Security Hardening Checklist

- [ ] All critical vulnerabilities addressed
- [ ] Authentication mechanisms reviewed
- [ ] Input validation implemented
- [ ] Sensitive data encrypted
- [ ] Security headers configured
- [ ] Dependencies updated
- [ ] Security tests added
- [ ] Deployment configuration secured
```

## Best Practices

### DO
- **Be Systematic** — Scan entire codebase, not just obvious files
- **Be Specific** — Provide exact file paths and line numbers
- **Be Actionable** — Give concrete fixes, not vague advice
- **Prioritize** — Focus on critical and high-severity issues first
- **Provide Context** — Explain why something is a threat
- **Show Examples** — Demonstrate secure implementations
- **Reference Standards** — Link to specific OWASP controls

### DON'T
- Skip thorough codebase exploration
- Make assumptions about security without examining code
- Provide generic security advice without specifics
- Ignore low-severity issues entirely (document them)
- Recommend fixes without understanding the codebase architecture
- Overwhelm with theoretical threats (focus on actual vulnerabilities)

## References

For detailed standard documentation, see:
- `references/MASVS.md` — Mobile Application Security Verification Standard
- `references/ASVS.md` — Application Security Verification Standard

## Notes

- **Authorization Context Required** — This skill is for authorized security testing, defensive security, educational contexts, or pre-deployment reviews only
- **Continuous Process** — Threat modeling should be repeated when adding features or changing architecture
- **False Positives** — Some findings may need manual review to confirm exploitability
- **Compliance** — These standards help meet security compliance requirements (SOC 2, ISO 27001, PCI-DSS, etc.)
