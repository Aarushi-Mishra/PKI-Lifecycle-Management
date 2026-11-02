# Phase 7 — Lessons Learned

## Key Concepts

| Term | Simple explanation |
|---|---|
| PKI | System for managing digital certificates and cryptographic keys. |
| CA | Certificate Authority that issues and signs certificates. |
| Root CA | Top-level trusted certificate authority. |
| Intermediate CA | CA authorized by a higher-level CA to issue certificates. |
| RSA | Asymmetric cryptographic algorithm. |
| Private Key | Secret key used for cryptographic operations. |
| Public Key | Key that can be shared publicly. |
| CSR | Certificate Signing Request containing identity and public-key details. |
| X.509 | Standard format for digital certificates. |
| TLS | Protocol for securing network communications. |
| SHA-256 | Cryptographic hash algorithm. |
| SAN | Subject Alternative Name identifying covered DNS names or IP addresses. |
| Serial Number | Unique certificate identifier assigned by the issuing CA. |
| CRL | Certificate Revocation List identifying revoked certificates. |
| Revocation | Invalidating a certificate before expiration. |
| Renewal | Issuing a replacement certificate for continued use. |
| CMDB | Configuration Management Database used to track IT assets and relationships. |
| HSM | Hardware Security Module used to protect cryptographic keys. |

## Key Lessons

### 1. Why use an Intermediate CA?

An Intermediate CA handles regular certificate issuance while reducing direct use of the Root CA private key.

### 2. Why validate a CSR?

To verify the request signature and inspect the requested identity and public key. Authorization must be checked separately.

### 3. Why is inventory management important?

An accurate inventory helps identify certificate ownership, track expirations, and plan renewals.

### 4. Why rotate keys during renewal?

Generating a new key reduces continued reliance on an older key and supports good key-management practices.

### 5. Why revoke certificates?

A certificate may need to be revoked when a private key is compromised or the certificate is no longer trustworthy.

### 6. Why verify certificate chains?

To confirm that certificates form a valid chain to a trusted CA.

## Operational Risks and Mitigations

| Risk | Mitigation |
|---|---|
| Expired certificate | Expiration tracking and timely renewal |
| Compromised private key | Revoke certificate and rotate keys |
| Incorrect certificate | Verify identity, SAN, and request authorization |
| Missing ownership | Maintain an accurate certificate inventory |
| Incomplete fulfillment | Document requests and verify results |
| Outdated inventory | Update records after every lifecycle change |

## Future Improvements

- Automate certificate expiration checks.
- Generate certificate expiration alerts.
- Integrate ServiceNow using APIs.
- Test TLS certificate deployment on a web server.
- Explore automated Certificate Lifecycle Management (CLM).

## Final Reflection

This project demonstrates how technical certificate operations connect with request fulfillment, inventory accuracy, documentation, security controls, and incident response.

It provides practical exposure to foundational Crypto Operations processes in a controlled lab environment.