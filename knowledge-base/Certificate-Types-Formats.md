# PKI Certificates & Formats — Interview Cheat Sheet

## 1. Types of certificates

| Certificate type | Purpose                                   | Common scenario                   |
| ---------------- | ----------------------------------------- | --------------------------------- |
| Root CA          | Establishes the highest level of trust    | Trusted root in browser/OS        |
| Intermediate CA  | Issues certificates on behalf of Root CA  | Enterprise certificate issuance   |
| TLS Server       | Authenticates a server                    | HTTPS/TLS handshake               |
| TLS Client       | Authenticates a client                    | mTLS, VPN, corporate Wi-Fi        |
| Code Signing     | Verifies software publisher and integrity | Application/software distribution |
| S/MIME           | Signs or encrypts emails                  | Secure corporate email            |

## 2. All important files used in the PKI project

| File                     | Type                        | Purpose                                     |
| ------------------------ | --------------------------- | ------------------------------------------- |
| `ca.key`                 | Root private key            | Signs Intermediate CA certificate           |
| `ca.crt`                 | Root certificate            | Establishes trust                           |
| `int.key`                | Intermediate private key    | Signs server certificates and CRLs          |
| `int.csr`                | Intermediate CSR            | Requests certificate from Root CA           |
| `int.crt`                | Intermediate certificate    | Authorizes certificate issuance             |
| `app-server.key`         | Server private key          | Proves server identity during TLS           |
| `app-server.csr`         | Server CSR                  | Requests TLS server certificate             |
| `app-server.crt`         | TLS server certificate      | Presented during TLS handshake              |
| `app-server-renewed.key` | Replacement private key     | Used with renewed certificate               |
| `app-server-renewed.csr` | Replacement CSR             | Requests renewed certificate                |
| `app-server-renewed.crt` | Replacement certificate     | Replaces original server certificate        |
| `crl.pem`                | Certificate Revocation List | Lists revoked certificates                  |
| `intermediate.ext`       | Configuration file          | Defines Intermediate CA permissions         |
| `intermediate.cnf`       | Configuration file          | Defines CA operations and issuance policies |
| `index.txt`              | CA database                 | Tracks issued and revoked certificates      |
| `serial`                 | Serial counter              | Tracks next certificate serial number       |
| `crlnumber`              | CRL counter                 | Tracks next CRL number                      |


## 3. Certificate formats and file extensions

| Format  | Common extensions              | What it contains                                          | Where used                         |
| ------- | ------------------------------ | --------------------------------------------------------- | ---------------------------------- |
| PEM     | `.pem`, `.crt`, `.cer`, `.key` | Base64-encoded certificates or keys                       | OpenSSL, Linux, Nginx              |
| DER     | `.der`, `.cer`                 | Binary-encoded certificates or keys                       | Windows, Java                      |
| PKCS#12 | `.p12`, `.pfx`                 | Certificate + private key + optional chain                | Windows IIS, certificate migration |
| PKCS#7  | `.p7b`, `.p7c`                 | Certificates and chains, no private keys                  | Certificate chain distribution     |
| PKCS#10 | `.csr`, `.req`                 | Certificate Signing Request                               | Certificate issuance               |
| PKCS#8  | `.key`, `.pem`                 | Private key, optionally encrypted                         | Key storage                        |
| JKS     | `.jks`                         | Java keystore containing certificates and/or private keys | Java applications                  |

### Remember

- TLS handshake: Server presents `.crt` certificate and usually its Intermediate CA chain; private key stays on the server.
- Certificate request: Application Team submits `.csr`, never the private key.
- Windows certificate import/export: Often `.pfx` / `.p12`.
- Linux/OpenSSL: Usually PEM-encoded `.crt`, `.key`, and `.pem`.
- Certificate revocation: CRLs may be stored as `.pem` or `.crl`.
- File extension ≠ file format: A `.crt` can contain PEM or DER data.

**Priority: Focus on Root CA, Intermediate CA, TLS Server/Client certificates, and the PEM, DER, PKCS#12, and PKCS#7 formats.**


## 4. What is X.509?

X.509 is an international standard (ITU-T) that defines the structure and information contained in digital certificates.

It is the standard used for TLS server certificates, client certificates, and CA certificates in the PKI project.

| Term              | Meaning                                         | Why it matters                                                      |
| ----------------- | ----------------------------------------------- | ------------------------------------------------------------------- |
| X.509             | Standard defining digital certificate structure | Enables systems to interpret and validate certificates consistently |
| Subject           | Identity the certificate represents             | Identifies the certificate holder                                   |
| Issuer            | CA that signed the certificate                  | Establishes who issued it                                           |
| Public Key        | Public cryptographic key                        | Used for signature verification or other permitted operations       |
| Serial Number     | Unique number assigned by issuing CA            | Identifies a certificate for tracking and revocation                |
| Validity          | Start and expiry dates                          | Determines when the certificate is valid                            |
| SAN               | Subject Alternative Name                        | Specifies DNS names or IP addresses covered                         |
| Digital Signature | CA's cryptographic signature                    | Allows verification of certificate authenticity                     |

### X.509 vs. PEM vs. CRT

These terms are often confused in interviews.

| Term            | What it describes                  |
| --------------- | ---------------------------------- |
| X.509           | Certificate standard and structure |
| PEM / DER       | How certificate data is encoded    |
| `.crt` / `.cer` | Filename extensions                |



**Takeaway: X.509 defines the structure of digital certificates used in PKI and TLS. It enables systems to verify identities, public keys, certificate issuers, validity periods, and digital signatures.**