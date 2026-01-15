# OWASP Mobile Application Security Verification Standard (MASVS)

**Version:** 2.1.0 (January 2025)

**Official Sources:**
- Primary Documentation: https://mas.owasp.org/MASVS/
- GitHub Repository: https://github.com/OWASP/masvs
- Latest Release: https://github.com/OWASP/masvs/releases/tag/v2.1.0

## Overview

The OWASP MASVS is the industry standard for mobile app security. It establishes baseline security and privacy requirements for mobile applications across iOS, Android, and cross-platform frameworks.

## Key Changes in v2.x

- **No More Verification Levels**: Traditional L1, L2, R levels moved to MAS Testing Profiles in MASTG
- **Focused Requirements**: Simplified, more actionable security controls
- **Privacy Category**: New MASVS-PRIVACY added in v2.1.0
- **CycloneDX Support**: SBOM format for DevOps integration

## Security Control Categories

### MASVS-STORAGE: Secure Data Storage

**Objective:** Ensure sensitive data stored on the device is protected

**Key Requirements:**
- Sensitive data must be encrypted at rest
- Use platform-provided secure storage (Keychain/KeyStore)
- No hardcoded secrets, API keys, or credentials in code
- Prevent sensitive data in logs, backups, or temp files
- Secure deletion of sensitive data when no longer needed

**Common Vulnerabilities:**
- API keys in strings or configuration files
- Passwords stored in SharedPreferences/UserDefaults
- PII in plain text SQLite databases
- Sensitive data in application logs
- Unencrypted local file storage

**Secure Implementations:**
- iOS: Keychain Services with kSecAttrAccessibleWhenUnlocked
- Android: EncryptedSharedPreferences, KeyStore, Room with SQLCipher
- React Native: react-native-keychain, expo-secure-store
- Flutter: flutter_secure_storage

---

### MASVS-CRYPTO: Cryptography

**Objective:** Ensure cryptographic operations are implemented securely

**Key Requirements:**
- Use industry-standard algorithms (AES-256, RSA-2048+, SHA-256+)
- Proper key management (no hardcoded keys)
- Secure random number generation (SecRandomCopyBytes, SecureRandom)
- Avoid deprecated algorithms (MD5, SHA1, DES, RC4, ECB mode)
- Use platform-provided crypto libraries (CommonCrypto, Jetpack Security)

**Common Vulnerabilities:**
- Hardcoded encryption keys in source code
- Using Math.random() for security purposes
- MD5/SHA1 for password hashing
- AES with ECB mode (deterministic encryption)
- Custom/homebrew cryptography

**Secure Implementations:**
- Password hashing: bcrypt, scrypt, Argon2
- Encryption: AES-256-GCM with random IVs
- Key derivation: PBKDF2, HKDF
- iOS: CryptoKit (iOS 13+) or CommonCrypto
- Android: Jetpack Security, Tink library

---

### MASVS-AUTH: Authentication & Authorization

**Objective:** Verify user identity and enforce access controls

**Key Requirements:**
- Implement authentication on remote endpoints, not just client
- Enforce strong password policies
- Support multi-factor authentication for sensitive apps
- Secure session management (timeout, revocation)
- Biometric authentication used appropriately
- OAuth/OIDC implemented correctly
- Authorization checks enforced server-side

**Common Vulnerabilities:**
- Client-side only authentication checks
- Session tokens stored insecurely
- No session timeout or revocation
- Biometric bypass vulnerabilities
- JWT tokens without signature verification
- Authorization decisions made on client

**Secure Implementations:**
- Token storage: Secure storage (Keychain/KeyStore)
- Session timeout: 15-30 minutes for sensitive apps
- Biometrics: LAContext (iOS), BiometricPrompt (Android)
- OAuth: Use AppAuth library, validate state parameter
- JWT: Validate signature, expiration, issuer, audience

---

### MASVS-NETWORK: Network Communication

**Objective:** Ensure secure data transmission over networks

**Key Requirements:**
- TLS 1.2+ for all network connections
- Certificate validation enabled and enforced
- Certificate pinning for sensitive applications
- No sensitive data in URLs or HTTP headers
- Reject cleartext HTTP traffic
- Secure WebSocket connections (WSS)

**Common Vulnerabilities:**
- Certificate validation disabled for "testing"
- HTTP used instead of HTTPS
- Sensitive tokens in URL parameters
- Accepting self-signed certificates in production
- Missing certificate pinning for high-risk apps
- Insecure TLS configurations

**Secure Implementations:**
- iOS: App Transport Security (ATS) enabled
- Android: Network Security Configuration with cleartextTrafficPermitted="false"
- Certificate pinning: TrustKit (iOS/Android), OkHttp CertificatePinner
- API keys: Use Authorization header, not URL params
- React Native: Use 'https://' URLs, enforce certificate validation

---

### MASVS-PLATFORM: Platform Interaction

**Objective:** Secure interaction with OS and other apps

**Key Requirements:**
- Minimize requested permissions (principle of least privilege)
- Validate data from external sources (deep links, intents, URL schemes)
- Secure IPC mechanisms
- WebView security hardening
- Secure keyboard handling (disable autocorrect for passwords)
- Prevent screenshot/screen recording for sensitive screens

**Common Vulnerabilities:**
- Excessive permissions requested
- Deep link/URL scheme injection
- Insecure intent handling (Android)
- WebView with JavaScript enabled unnecessarily
- Exported components without permission checks (Android)
- Sensitive data visible in app switcher

**Secure Implementations:**
- Deep links: Validate and sanitize all input
- Android: Explicit intents, validate intent data, use android:exported="false"
- iOS: Validate URL scheme parameters, use Universal Links
- WebView: Disable JavaScript if not needed, validate URLs before loading
- Sensitive screens: FLAG_SECURE (Android), ignoresSafeArea (iOS)

---

### MASVS-CODE: Code Quality

**Objective:** Maintain secure coding practices

**Key Requirements:**
- Input validation for all untrusted data
- Prevent injection vulnerabilities (SQL, command, XSS)
- Secure deserialization practices
- Memory safety (prevent buffer overflows, use-after-free)
- Keep dependencies up to date
- Remove debug code and logging from production builds
- Third-party SDK security review

**Common Vulnerabilities:**
- SQL injection via string concatenation
- Unsafe deserialization of untrusted data
- XSS in WebViews displaying user content
- Outdated dependencies with known CVEs
- Debug logging in production
- Memory corruption vulnerabilities

**Secure Implementations:**
- SQL: Use parameterized queries, prepared statements, ORM
- Validation: Whitelist approach, type checking, length limits
- Dependencies: Regular updates, automated vulnerability scanning
- Build configurations: Separate debug/release, ProGuard/R8 (Android)
- Native code: Use safe string functions, ASLR, stack canaries

---

### MASVS-RESILIENCE: Anti-Tampering & Anti-Reversing

**Objective:** Make reverse engineering and tampering difficult

**Note:** These controls are optional for most apps but required for high-security applications (banking, DRM, etc.)

**Key Requirements:**
- Code obfuscation to impede analysis
- Anti-debugging techniques
- Integrity checks (app signing verification)
- Root/jailbreak detection for sensitive apps
- Tamper detection mechanisms
- Runtime application self-protection (RASP)

**Common Weaknesses:**
- No obfuscation (readable code after decompilation)
- Debug symbols in production builds
- No integrity verification
- Sensitive apps running on rooted/jailbroken devices
- Easy-to-bypass protection mechanisms

**Implementations (Use Judiciously):**
- Obfuscation: ProGuard/R8 (Android), Swift obfuscation
- Root detection: RootBeer (Android), DTTJailbreakDetection (iOS)
- Integrity: Verify app signature at runtime
- Anti-debug: Detect debugger attachment, timing checks
- **Warning:** These provide defense in depth but can be bypassed by determined attackers

---

### MASVS-PRIVACY: Privacy Controls

**Objective:** Protect user privacy and comply with regulations (GDPR, CCPA)

**Introduced:** v2.1.0 (January 2025)

**Key Requirements:**
1. **Minimize access to sensitive data and resources**
   - Request minimum necessary permissions
   - Access location/camera/contacts only when needed
   - Temporal permission usage (access only during feature use)

2. **Prevent user identification**
   - Avoid persistent device identifiers without consent
   - No fingerprinting techniques for tracking
   - Anonymize analytics data

3. **Transparency about data collection**
   - Clear privacy policy
   - In-app disclosures before data collection
   - Explain purpose of each permission

4. **User control over their data**
   - Opt-in for non-essential data collection
   - Data export/download capabilities
   - Account and data deletion
   - Revocable consent mechanisms

**Common Privacy Violations:**
- Collecting IMEI, IMSI, or advertising IDs without consent
- Tracking location continuously in background
- Sharing user data with third parties without disclosure
- No mechanism to delete account/data
- Analytics SDKs with invasive defaults

**Privacy-Preserving Implementations:**
- Use ephemeral IDs instead of persistent identifiers
- On-device processing instead of cloud (when possible)
- Differential privacy for analytics
- Clear consent UI with granular options
- Regular data retention policy enforcement

## Testing Guidance

The MASVS works with the **MASTG** (Mobile Application Security Testing Guide) which provides:
- Atomic test cases for each control
- Platform-specific testing procedures
- Tools and techniques for verification

**Testing Profiles:**
- **MAS-L1**: Standard security for all apps
- **MAS-L2**: Sensitive data apps (healthcare, finance)
- **MAS-R**: Anti-reversing for high-security apps

## Tools for MASVS Testing

- **Static Analysis**: MobSF, Semgrep, Checkmarx
- **Dynamic Analysis**: Frida, objection, Burp Suite Mobile Assistant
- **Network Analysis**: Charles Proxy, mitmproxy, Burp Suite
- **iOS**: idb, Clutch, class-dump
- **Android**: jadx, apktool, Drozer

## Compliance Mapping

MASVS helps achieve compliance with:
- **GDPR/CCPA**: MASVS-PRIVACY controls
- **PCI-DSS**: MASVS-STORAGE, MASVS-CRYPTO, MASVS-AUTH
- **HIPAA**: MASVS-STORAGE, MASVS-NETWORK, MASVS-CRYPTO
- **SOC 2**: Multiple MASVS categories

## Additional Resources

- **MASTG** (Testing Guide): https://mas.owasp.org/MASTG/
- **MASWE** (Weakness Enumeration): Mobile security weaknesses database
- **MAS Checklist**: https://mas.owasp.org/checklists/
- **CycloneDX SBOM**: For CI/CD integration

---

*Last Updated: January 2025 (MASVS v2.1.0)*
