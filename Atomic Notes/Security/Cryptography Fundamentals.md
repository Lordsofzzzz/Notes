---
status: seedling
tags: [security, cryptography, encoding, hashing]
created: 2026-04-19
updated: 2026-04-19
---

# Cryptography Fundamentals

## Summary
The study of techniques for secure communication, covering encoding, hashing, and encryption.

## Details
**Core Logical Content Mandate**:
Cryptography ensures confidentiality, integrity, and authenticity.

### Encoding vs. Hashing vs. Encryption
- **Encoding**: Data transformation for transmission (not for security). Example: **Base64**.
- **Hashing**: One-way transformation for integrity. Same input always yields same output. Example: **SHA256**.
- **Encryption**: Two-way transformation for confidentiality, requiring a key.

### Hashing
Used for password storage and file integrity.
- **Tools**: `sha256sum`, `md5sum`, `certutil -hashfile`.
- **Logic**: Use a **Salt** (random data) with a hash to prevent rainbow table attacks.

### Symmetric vs. Asymmetric Encryption
- **Symmetric**: Same key for encryption and decryption (e.g., **AES**, **Blowfish**).
- **Asymmetric**: Key pair (Public/Private). Public encrypts, Private decrypts (e.g., **RSA**, **ECC**). Used in **SSH keys** and **TLS**.

### Key Tools
- `openssl`: General purpose crypto tool for file encryption and certificate generation.
- `gpg`: For asymmetric file encryption and signing.
- `ssh-keygen`: Generating RSA/Ed25519 pairs for remote access.

## Associative Trails
A grasp of cryptography is essential for understanding how to protect your own communications (OpSec) and how to attack weak implementations (Password cracking).

## Connections
- [[Password Attacks]]
- [[VPN and Proxy Setup (Red Team)]]

## Sources
- [[PEN-100-notes.md]]
