# TLS

Before understanding how HTTPS works, it is very important to understand the terminology and the concept behind it. 

#### Facts:
1. Any message encrypted with Ramesh's public key can only be decrypted with Ramesh private key.
2. Anyone access to Ramesh's public key can verify that a message could only have been created by someone with access to Rmaeh's private key.
3. Private key can always derive the public key. Public key can never derive the private key — mathematically impossible

| Term | Full form | What it does | Secure? | Status |
|------|-----------|--------------|---------|--------|
| HTTP | HyperText Transfer Protocol | Transfers data between browser and server | No — plain text | Still used |
| HTTPS | HyperText Transfer Protocol Secure | HTTP running inside an encrypted tunnel | Yes | Current standard |
| SSL | Secure Sockets Layer | Original encryption protocol for that tunnel | Weak — broken | Deprecated |
| TLS | Transport Layer Security | Modern replacement for SSL | Yes | Current standard |

> **One line to remember:** HTTPS is the goal. TLS is how it works. SSL was the original attempt, now retired.
TLS is the encryption standard. HTTP uses TLS to become HTTPS. Similarly TLS can work with many other protocols to encrypt the data in communication between devices.

Let's see the objects and stakeholders in this overall concept.

## Stakeholders

| | Stakeholder | Description |
|---|---|---|
| 👤 | **Website owner** | The company or developer who generates keys, obtains the certificate and deploys it on the server |
| 🧑‍💻 | **End user** | The visitor — their browser silently validates the certificate before any data flows |
| 🛡️ | **CA vendor** | Verifies domain ownership and issues the signed certificate — e.g. Let's Encrypt, ZeroSSL, DigiCert |
| 🌐 | **Domain registrar** | Where the domain name is registered — used during domain ownership validation e.g. GoDaddy, Namecheap |

## Key objects

## Key Objects

| | Object | Description | Note |
|---|---|---|---|
| 🔑 | **Private key** | A large random number — the root of everything. Used to sign the CSR and decrypt TLS sessions | 🔴 Never shared |
| 🔓 | **Public key** | Mathematically derived from the private key. Embedded inside the certificate | 🟢 Safe to share |
| 📄 | **CSR** | Certificate Signing Request — contains your public key + identity info + signature. Sent to the CA | 📤 Sent to CA |
| 📜 | **Certificate (.crt)** | Issued by CA after domain validation. Contains public key, domain, validity dates and CA's signature | 🟢 CA-signed |

## Certificate Chain

| | Level | Real world analogy | Description |
|---|---|---|---|
| 🏭 | **Root certificate** | Manufacturer | Pre-installed in your OS and browser — the ultimate trust anchor |
| 🏢 | **Intermediate certificate** | Regional distributor | Signed by root CA — bridges the root to your certificate |
| 🏪 | **Leaf certificate** | Local seller | Your certificate — signed by intermediate, presented to the browser |
| 🧑‍💻 | **Browser (end user)** | Customer | Walks the chain upward — root found in trust store → shows padlock |

> **Trust flows top-down. Verification walks bottom-up.**

## Infrastructure

| | Component | Description |
|---|---|---|
| 🖥️ | **Web server** | Holds the private key and certificate — presents them during the TLS handshake e.g. Nginx, Apache, Caddy |
| 🔐 | **OS trust store** | Built-in list of ~150 trusted root CA certificates in every OS and browser — foundation of public trust |
| 📡 | **DNS** | Domain name system — used during certificate issuance to prove domain ownership via TXT records or HTTP files |
| 🌍 | **Browser** | Validates the full certificate chain against the OS trust store — shows padlock when all checks pass |

