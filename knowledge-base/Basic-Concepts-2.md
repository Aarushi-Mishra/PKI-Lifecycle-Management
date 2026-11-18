# Understanding PKI: From Certificate Authorities to Website Security

## What is a Certificate Authority (CA)?

A Certificate Authority (CA) is a trusted organization or system that issues digital certificates.

Think of a CA as a passport-issuing authority for websites and devices. It verifies identity and issues a digitally signed certificate that others can validate.

Main functions:

- Validate the identity or domain control of certificate applicants.
- Issue and digitally sign certificates.
- Manage certificate validity and revocation.

### Public CA vs. Private CA

| Public CA                                        | Private CA                                                |
| ------------------------------------------------ | --------------------------------------------------------- |
| Issues publicly trusted certificates             | Issues certificates for internal systems                  |
| Trusted by common browsers and operating systems | Trusted only by devices configured to trust it            |
| Examples: DigiCert, Sectigo, Let's Encrypt       | Examples: Microsoft AD CS, internal enterprise CA         |
| Common use: Public HTTPS websites                | Common use: Internal applications, corporate VPN, devices |

### How do public CAs make money?

Commercial CAs sell certificate products and related services, such as enterprise certificate lifecycle management, automation, support, identity validation, and code signing.

Not all public CAs charge for certificates. For example, Let's Encrypt provides free TLS certificates.

_Question: If a CA issues a certificate, why should your browser trust that CA?_

## How does a browser or device trust a CA?

Your device contains a trusted root certificate store, which holds certificates belonging to trusted Root CAs.

Browsers and operating systems maintain trust programs with requirements for including and retaining these CAs.

When you visit a website, your browser verifies a certificate chain:

**Trusted Root CA (Already trusted by the browser or operating system)**        
        ⬇️
        
**Intermediate CA (Certificate signed by the Root CA)**         
        ⬇️
        
**Leaf Certificate (Website certificate signed by the Intermediate CA)**

This is called the **certificate trust path** or chain of trust.

The browser checks the certificate signatures, hostname, validity, and applicable security requirements, tracing the chain back to a trusted Root CA.

### What if a trusted Root CA is compromised?

It is extremely serious. An attacker who controls its signing key could potentially issue fraudulent certificates that appear trusted. However, this does not automatically compromise every website or encrypted session. Browser vendors and operating systems can remove trust in the affected CA.

_Question: If Root CAs are so powerful, why don't they issue every website certificate directly?_

## Why do we need Intermediate CAs?

Because the Root CA's private key is too sensitive to use for routine certificate issuance.

In a well-designed hierarchy:

- Root CA: Its private key is normally kept offline or protected with strict controls.
- Intermediate CA: Handles day-to-day certificate issuance and can be operated online under controlled security policies.
- Leaf certificate: Belongs to the website, application, or device.

This structure limits exposure of the Root CA private key and allows individual Intermediate CAs to be replaced or revoked if necessary.

_Question: How does an organization get its own website certificate from a CA?_

## How can organizations request leaf certificates?

A leaf certificate is the certificate issued to an end entity, such as a website or device.

Organizations generally use two approaches.

| Method                  | How it works                                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------ |
| CA website or portal    | Enter domain and organization information. The portal may generate a key and CSR, or ask you to provide one. |
| Generate a CSR yourself | Generate a private key and CSR locally, then submit the CSR to the CA.                                       |

Modern systems can also automate certificate enrollment through APIs and protocols such as ACME.

Important: The private key should remain under the control of the organization or its authorized key-management service.

_Question: What actually happens when a CSR is generated?_

## What are the steps to create a CSR and obtain a certificate?

1. Generate key pair
   - Create a private key and corresponding public key

2. Generate CSR
   - Include public key and requested identity, such as domain and organization

3. Submit request to CA
   - CA verifies domain control and other required information

4. CA issues certificate
   - CA digitally signs the approved certificate

5. Install on website server
   - Configure the certificate and private key, then verify HTTPS

The CSR contains the public key, requested identity, and proof of possession of the private key through a digital signature. The private key itself is not submitted.

_Question: What happens if the certificate becomes untrustworthy before it expires?_

## How can a certificate be revoked?

A certificate may need to be revoked before its expiration date if it can no longer be trusted.

Common reasons include:

- Private key compromise: Someone obtains the website's private key.
- CA compromise: An issuing CA's signing key is compromised.
- Incorrect issuance: A certificate was issued to an unauthorized party.
- Certificate no longer needed: An application is retired or replaced.

### How does the browser know a certificate was revoked?

One method is a Certificate Revocation List (CRL).

A CRL is a digitally signed list of revoked certificate serial numbers, published by the issuing CA.

CA revokes certificate

        ⬇️        
CA publishes updated CRL (Typically hosted at an HTTP distribution URL)

        ⬇️
Client checks CRL during certificate validation (If revocation checking is enabled)

The certificate may contain a CRL Distribution Point (CDP) telling clients where to obtain the CRL.

**Disadvantage:** CRLs can become large, and clients may rely on cached copies until they're refreshed. A recently revoked certificate might not immediately be recognized.

**Advantage:** Clients can reuse a downloaded CRL, reducing the number of direct requests to CA infrastructure.

_Question: Instead of downloading a list, couldn't the browser simply ask whether one particular certificate is valid?_

## What is OCSP, and why was it introduced?

OCSP = Online Certificate Status Protocol.

Instead of downloading a list of revoked certificates, the client queries an OCSP responder for a particular certificate's status.

Browser receives certificate

            ⬇️
Browser queries OCSP responder

            ⬇️
Responder returns signed status (Good, Revoked, Unknown)

**Advantages:** Smaller responses and potentially fresher certificate status information.

**Disadvantages:** Direct queries can increase responder traffic, introduce latency, and reveal which sites a client visits. Responses can also be cached, so OCSP is not necessarily perfectly real-time.

_ Question: Can we obtain the benefits of OCSP without making every browser contact the responder separately?_

## CRL or OCSP? What about OCSP Stapling?

OCSP Stapling lets the web server obtain a signed OCSP response and include it in the TLS handshake.

The server caches a signed response and can reuse it for multiple client connections until it needs refreshing.

| Method        | Main advantage                                 | Main drawback                                   |
| ------------- | ---------------------------------------------- | ----------------------------------------------- |
| CRL           | Download once, check multiple certificates     | Lists can be large or stale                     |
| OCSP          | Checks status of an individual certificate     | Adds queries, latency and privacy concerns      |
| OCSP Stapling | Server delivers signed status with certificate | Requires server support and refreshed responses |

### Which is best? 
There isn't one universal answer. OCSP Stapling improves efficiency where supported, but modern browsers also use other revocation mechanisms, including browser-distributed revocation data and short-lived certificates.

_Question: Does a browser repeat all these checks every time we visit a website?_

## Is the entire process repeated every time a client visits a website?

No. Certificate issuance happens beforehand, and a full TLS handshake isn't required for every page request.

Here's the difference:

| Activity                     | When it happens                                     |
| ---------------------------- | --------------------------------------------------- |
| Key generation and CSR       | When requesting a certificate                       |
| CA validation and issuance   | Before the certificate is installed                 |
| Certificate presentation     | During a full certificate-based TLS handshake       |
| Certificate chain validation | When authenticating the server                      |
| CRL/OCSP checking            | Depending on client policy and caching              |
| New page request             | Often reuses an existing TLS connection             |
| TLS session resumption       | Can avoid repeating full certificate authentication |

### What is Certificate Pinning?

Certificate pinning adds an extra trust restriction.

Normally, a browser accepts a valid certificate chain ending at any CA it trusts.

With pinning, an application only accepts a particular certificate or public key (or a configured set of them).

For example, a banking mobile application might be configured to trust only specific public keys for its backend API.

Even if an attacker obtains a fraudulent certificate from another trusted CA, the application can reject it because the public key doesn't match its pin.

### Why isn't pinning used everywhere?
Pinning can cause outages when certificates or keys rotate unexpectedly. Browser-based HTTP Public Key Pinning (HPKP) has been removed from Chrome, although carefully managed pinning remains useful in some mobile applications and controlled environments.

**Key distinction:** Certificate pinning does not replace CA validation or revocation checking. It adds another restriction on which certificates or keys are accepted.

## Does every page of every website need a separate certificate?

No. One certificate can protect multiple pages, subdomains, or even different domains, depending on the names listed in its Subject Alternative Name (SAN).

| Certificate arrangement | Example                                             | What it covers                                                           |
| ----------------------- | --------------------------------------------------- | ------------------------------------------------------------------------ |
| Single-domain           | `www.example.com`                                   | That specific hostname                                                   |
| Wildcard                | `*.example.com`                                     | First-level subdomains such as `shop.example.com` and `mail.example.com` |
| Multi-domain (SAN)      | `www.example.com`, `api.example.com`, `example.org` | Multiple explicitly listed hostnames                                     |

For example, these URLs can use the same certificate because they share the hostname `www.example.com`:

```
https://www.example.com/home
https://www.example.com/products
https://www.example.com/contact
```

Certificates identify hostnames, not individual web pages or URL paths.

A large organization may still use separate certificates for different applications to improve security, simplify management, and limit the impact of a compromised private key.

ds a restriction on acceptable certificates or public keys

The central idea: PKI lets a client establish trust in a server's identity using certificates, cryptographic signatures, and a trusted CA hierarchy. Revocation mechanisms and additional controls help maintain that trust throughout the certificate's lifetime.
