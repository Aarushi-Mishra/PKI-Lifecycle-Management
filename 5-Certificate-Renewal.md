# Phase 5 — Certificate Renewal

## Objective
Simulate a planned TLS certificate renewal to maintain certificate validity and reduce the risk of service disruptions caused by expiration.

## Process:

Expiration Approaching / Planned Renewal

↓

IT Security Analyst Identifies Renewal Need

↓

Application Team Submits Request

↓

Application Team Generates New Key + CSR

↓

IT Security Analyst Validates Request

↓

Intermediate CA Issues Replacement

↓

IT Security Analyst Verifies Certificate

↓

Application Team Deploys Replacement

↓

Inventory Updated and Ticket Closed

## Business Scenario
The Payment Application Team requests renewal of the TLS certificate for `payments.securebank.example` as part of routine certificate lifecycle maintenance.

## Renewal Process

| Step | Activity | Description |
|---|---|---|
| 1 | Review Existing Certificate | Checked the current certificate's expiration date and serial number. |
| 2 | Create ServiceNow Ticket | Documented the renewal request for the Payment Platform and marked it In Progress. |
| 3 | Generate Private Key | Created a new 2048-bit RSA private key for the replacement certificate. |
| 4 | Generate CSR | Created a Certificate Signing Request (CSR) using the new private key and application identity. |
| 5 | Issue Certificate | Used the Intermediate CA to sign the CSR and issue a replacement TLS certificate with a new serial number. |
| 6 | Verify Certificate | Validated the replacement certificate against the Root CA and Intermediate CA certificate chain. |
| 7 | Review Certificate Details | Checked the replacement certificate's validity dates and serial number. |
| 8 | Update ServiceNow | Recorded the new certificate details, verification result, and previous certificate information. |

## Validation

- **Trust Chain:** Replacement certificate verified against the Root CA and Intermediate CA.
- **Certificate Identity:** Associated with `payments.securebank.example`.
- **Serial Number:** New certificate issued with a distinct serial number.
- **Validity:** New certificate validity dates reviewed.
- **Deployment Status:** Issued / Pending Deployment. Actual deployment was outside the lab scope.


## Outcome

Simulated the certificate renewal lifecycle, including private key generation, CSR creation, certificate issuance, trust-chain verification, and ServiceNow inventory updates.

**Key Learning:** Certificate renewal involves more than replacing an expiring certificate. It requires proper key management, CA validation, accurate inventory tracking, and deployment coordination to maintain service continuity.
