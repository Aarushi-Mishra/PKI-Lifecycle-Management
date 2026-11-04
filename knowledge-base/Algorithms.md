
# Important Algorithms That Are Highly Relevant


| Algorithm         | Full Form                                                       | Category                         | Remember This                                                     |
| ----------------- | --------------------------------------------------------------- | -------------------------------- | ----------------------------------------------------------------- |
| ⭐️ AES-256-GCM       | Advanced Encryption Standard (256-bit), Galois/Counter Mode     | Symmetric encryption (AEAD)      | Encrypts data and protects its integrity                          |
| ChaCha20-Poly1305 | ChaCha20 stream cipher + Poly1305 authenticator                 | Symmetric encryption (AEAD)      | Alternative to AES-GCM, commonly used in TLS                      |
| ⭐️ RSA-2048/4096     | Rivest–Shamir–Adleman (2048/4096-bit)                           | Asymmetric cryptography          | Public/private keys for certificates and digital signatures       |
| ⭐️ ECDSA P-256       | Elliptic Curve Digital Signature Algorithm (256-bit NIST curve) | Asymmetric — Digital signature   | Authenticates servers and signs certificates                      |
| ⭐️ X25519            | Curve25519-based Diffie–Hellman key agreement                   | Asymmetric — Key agreement       | Establishes shared secrets during TLS key exchange                |
| ⭐️ SHA-256           | Secure Hash Algorithm (256-bit)                                 | Cryptographic hashing            | Generates digests for integrity and digital signatures            |
| HMAC-SHA256       | Hash-based Message Authentication Code using SHA-256            | Keyed hashing / MAC              | Verifies message integrity and authenticity using a shared secret |
| ⭐️ ML-KEM            | Module-Lattice-Based Key-Encapsulation Mechanism                | Post-quantum — Key establishment | Establishes shared secrets resistant to known quantum attacks     |
| ⭐️ ML-DSA            | Module-Lattice-Based Digital Signature Algorithm                | Post-quantum — Digital signature | Provides quantum-resistant digital signatures                     |





- Symmetric: AES-GCM, ChaCha20-Poly1305 → Encrypt data.
- Asymmetric: RSA, ECDSA → Digital signatures and authentication.
- Key agreement: X25519 → Establish a shared secret.
- Hashing: SHA-256 → Generate a digest.
- Keyed hashing: HMAC-SHA256 → Authenticate messages.
- Post-quantum: ML-KEM, ML-DSA → Key establishment and signatures designed to resist quantum attacks.

