# Phase 3 — Certificate Request and Issuance

## Objective

Simulate application certificate request intake, CSR validation, issuance, and verification using ServiceNow and OpenSSL.

## Business Scenario

The Payment Application Team requests a TLS certificate for:

`payments.securebank.example`

## Step 1 — Create ServiceNow Request

Create a ServiceNow request using an available workflow.

**Ticket details:**

| Field | Value |
|---|---|
| Short Description | New TLS Certificate Request |
| Application | Payment Platform |
| Domain | payments.securebank.example |
| Priority | Medium |
| State | In Progress |
| Assignment Group | Crypto Operations (if configured) |

An Incident record may be used as a simplified lab ticket if a request workflow is unavailable.

## Step 2 — Generate Server Private Key

```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out pki-lab/server/app-server.key
```

### Command Breakdown

| Term | Meaning |
|---|---|
| `openssl` | Cryptographic tool. |
| `genpkey` | Generates a private key. |
| `-algorithm RSA` | Selects RSA encryption/signing algorithm. |
| `-pkeyopt` | Supplies key-generation options. |
| `rsa_keygen_bits:2048` | Generates a 2048-bit RSA key. |
| `-out` | Defines output location. |
| `app-server.key` | Server private-key filename. |

**Expected Result:** Server private key generated.

## Step 3 — Generate CSR

```bash
openssl req -new \
-key pki-lab/server/app-server.key \
-out pki-lab/server/app-server.csr \
-subj "/CN=payments.securebank.example/O=SecureBank Lab/C=US"
```

### Command Breakdown

| Term | Meaning |
|---|---|
| `req` | Handles certificate requests. |
| `-new` | Creates a new CSR. |
| `-key` | Specifies the private key. |
| `app-server.key` | Server private key. |
| `-out` | Specifies output file. |
| `app-server.csr` | Certificate Signing Request. |
| `-subj` | Defines identity details. |
| `CN` | Common Name (domain). |
| `O` | Organization. |
| `C` | Country. |

**Expected Result:** CSR file generated.

## Step 4 — Validate CSR

```bash
openssl req -in pki-lab/server/app-server.csr -noout -text -verify
```

### Command Breakdown

| Term | Meaning |
|---|---|
| `req` | Processes certificate requests. |
| `-in` | Identifies the input file. |
| `app-server.csr` | CSR being inspected. |
| `-noout` | Suppresses encoded request output. |
| `-text` | Displays readable request details. |
| `-verify` | Verifies the CSR signature. |

**Expected Result:** CSR signature verification succeeds.

**Important:** CSR validity does not establish requester authorization. Authorization must be checked through the request workflow.

## Step 5 — Initialize CA Database

Run these commands once for a new Intermediate CA, before the first certificate issuance.

```bash
touch pki-lab/intermediate-ca/index.txt
```

| Term | Meaning |
|---|---|
| `touch` | Creates an empty file if one doesn't exist. |
| `index.txt` | CA database tracking issued and revoked certificates. |

```bash
echo 1000 > pki-lab/intermediate-ca/serial
```

| Term | Meaning |
|---|---|
| `echo` | Prints text or a value. |
| `1000` | Starting certificate serial number (hexadecimal in the CA database). |
| `>` | Writes output to a file, replacing existing contents. |
| `serial` | File storing the next serial number. |

```bash
echo 1000 > pki-lab/intermediate-ca/crlnumber
```

| Term | Meaning |
|---|---|
| `echo` | Prints a value. |
| `1000` | Initial CRL number. |
| `>` | Writes the value to a file. |
| `crlnumber` | Stores the next Certificate Revocation List number. |

**Expected Result:** The CA tracking files are initialized.

**Warning:** Do not repeat the two `echo` commands after issuing certificates. They overwrite existing counters.

Before proceeding, create `config/intermediate.cnf` using the configuration supplied in the project instructions.

## Step 6 — Issue Server Certificate

```bash
openssl ca -batch \
-config config/intermediate.cnf \
-extensions server_cert \
-in pki-lab/server/app-server.csr \
-out pki-lab/server/app-server.crt \
-notext
```

### Command Breakdown

| Term | Meaning |
|---|---|
| `ca` | Performs Certificate Authority operations. |
| `-batch` | Runs without interactive approval prompts. |
| `-config` | Specifies OpenSSL configuration. |
| `intermediate.cnf` | Intermediate CA configuration file. |
| `-extensions` | Selects certificate extension settings. |
| `server_cert` | Configuration section for TLS server certificates. |
| `-in` | Identifies the input CSR. |
| `app-server.csr` | Application server CSR. |
| `-out` | Specifies output location. |
| `app-server.crt` | Signed server certificate. |
| `-notext` | Excludes extra readable text from the certificate output. |

**Expected Result:** Certificate issued and recorded in the CA database.

## Step 7 — Verify Certificate Chain

```bash
openssl verify \
-CAfile pki-lab/root-ca/ca.crt \
-untrusted pki-lab/intermediate-ca/int.crt \
pki-lab/server/app-server.crt
```

### Command Breakdown

| Term | Meaning |
|---|---|
| `verify` | Validates the certificate chain. |
| `-CAfile` | Defines the trusted Root CA file. |
| `ca.crt` | Root CA trust anchor. |
| `-untrusted` | Provides additional chain certificates. |
| `int.crt` | Intermediate CA certificate. |
| `app-server.crt` | Certificate being verified. |

**Expected Result:** `OK`

## Step 8 — Inspect Certificate Details

```bash
openssl x509 -in pki-lab/server/app-server.crt -noout -dates -serial -ext subjectAltName
```

### Command Breakdown

| Term | Meaning |
|---|---|
| `x509` | Processes X.509 certificates. |
| `-in` | Specifies the certificate file. |
| `app-server.crt` | Issued TLS certificate. |
| `-noout` | Suppresses encoded certificate output. |
| `-dates` | Displays validity start and expiry dates. |
| `-serial` | Displays the certificate serial number. |
| `-ext` | Displays a specific certificate extension. |
| `subjectAltName` | Lists approved DNS names or IP addresses for the certificate. |

**Expected Result:** Certificate information is displayed, including the intended DNS name.

## Step 9 — Update ServiceNow

Document:

- CSR signature verification
- Request approval, if actually completed
- Certificate issuance
- Certificate serial number
- Expiration date
- Successful chain verification

Mark the request Completed or Resolved as appropriate.

## Screenshots

**Screenshot 1 — ServiceNow request**

`![Certificate Request](screenshots/03-request-ticket.png)`

**Screenshot 2 — Certificate verification**

`![Certificate Verification](screenshots/03-certificate-verification.png)`

**Screenshot 3 — Fulfilled ticket**

`![Resolved Ticket](screenshots/03-resolved-ticket.png)`

## Outcome

Demonstrated certificate request handling, CSR validation, issuance, verification, and operational documentation.