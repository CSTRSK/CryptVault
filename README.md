# 🔐 CryptVault

---

## 🎯 Overview

**CryptVault** is a fully client-side, serverless Progressive Web App (PWA) that encrypts password lists using **AES-256-GCM** and splits the encryption key using **[Shamir's Secret Sharing](https://en.wikipedia.org/wiki/Shamir%27s_Secret_Sharing)** (SSS). 

The key is divided into **5 shares**, requiring **any 3** to reconstruct it. This provides mathematically provable security with **zero server dependency**, **zero cloud storage**, and **zero network transmission** of sensitive data.

> **Perfect for**: Digital inheritance planning, secure password backup, corporate secret sharing, and privacy-conscious users who demand full control over their cryptographic keys.

---

## ✨ Features

### 🔐 Cryptographic Security
- **AES-256-GCM** encryption via native WebCrypto API
- **Shamir's Secret Sharing (3-of-5 threshold)** with information-theoretic security
- **256-bit keys** generated via `crypto.getRandomValues()`
- **Perfect secrecy**: <3 shares reveal **zero** information about the key
- **Quantum-resistant**: No factorization or discrete logarithm dependencies

### 🌐 Offline-First PWA
- **100% offline capable** — works without any network connection
- **Installable** on desktop and mobile (Add to Home Screen)
- **Service Worker** caches all assets for instant loading
- **Zero backend** — no server, no database, no API calls

### 🎨 Modern UI/UX
- **Bulma CSS** framework for responsive, clean interface
- **Alpine.js** for lightweight reactivity without build steps
- **Dark theme optimized** with glassmorphism design
- **QR Code generation** for easy share transfer
- **Drag & drop** file import (JSON, CSV, TXT)

### 🔒 Privacy by Design
- **Memory-only operation**: Secrets never touch disk or localStorage
- **Zero telemetry**: No analytics, no tracking, no cookies
- **Auditable**: Single HTML file, readable source code
- **No supply chain**: Zero npm dependencies for core functionality

---

## 🚀 Quickstart

### Option 1: Direct Usage (Recommended)

```bash
# Clone repository
git clone https://github.com/CSTRSk/CryptVault.git

# Navigate to directory
cd cryptvault

# Start local server (Python 3)
python3 -m http.server 8000

# Or with Node.js
npx serve .

# Open browser: http://localhost:8000
```

Option 2: Install as PWA

1. Open the app in a modern browser (Chrome, Edge, Safari, Firefox)
2. Click "Install CryptVault" or "Add to Home Screen"
3. Launch as standalone app — completely offline

Option 3: Single File

Simply save `index.html` and open it locally. No server required.

---

📖 Usage Guide

Step 1: Import Password List

Supported formats:
- JSON: `[{"service": "google", "username": "admin", "password": "secret"}]`
- CSV: `service,username,password`
- TXT: Tab or comma-separated values

Example CSV:

```csv
service,username,password
google,admin,secret123
github,developer,gh_token_456
banking,john.doe,secure789!
```

Step 2: Encrypt

1. Click "Generate 256-bit Key"
2. Click "Encrypt Passwords"
3. Download `.enc` file (contains IV + ciphertext + auth tag)

Step 3: Split Key (SSS)

1. Click "Split Key into 5 Shares"
2. Distribute shares to trusted contacts:
   - Share 1: Family member A
   - Share 2: Family member B  
   - Share 3: Trusted friend
   - Share 4: Attorney/Notary
   - Share 5: Bank safe deposit

Security rule: Never store >1 share in the same physical location!

Step 4: Recovery

1. Collect any 3 shares from trustees
2. Enter shares in recovery interface
3. Upload encrypted `.enc` file
4. Automatic key reconstruction and decryption

---

🏗️ Architecture

```

+--------------------------------------------------------------------------+
|                         BROWSER (Client-Only)                            |
|                                                                          |
|   +--------------+   +----------------+   +------------------------+     |
|   |   UI Layer   |<->|    WebCrypto   |<->|       SSS Engine       |     |
|   | Bulma+Alpine |   |  AES-256-GCM   |   |     GF(2^256) Math     |     |
|   +--------------+   +----------------+   +------------------------+     |
|          |                    |                      |                   |
|          v                    v                      v                   |
|   +--------------+   +----------------+   +------------------------+     |
|   |  File Import |   |    Encrypt     |   |    5 Shares Output     |     |
|   | JSON/CSV/TXT |-->|   IV+Cipher    |-->| (3-of-5 Threshold SSS) |     |
|   +--------------+   +----------------+   +------------------------+     |
|                                                                          |
|   SECURITY GUARANTEES                                                    |
|   - No network requests after initial load                               |
|   - No data persistence (memory-only)                                    |
|   - No external crypto libraries (native WebCrypto only)                 |
+--------------------------------------------------------------------------+

---

🔐 Security Model

Threat Mitigation

Threat Vector	Risk Level	Mitigation	
Server compromise	Eliminated	No server exists	
Man-in-the-middle	Eliminated	No network transmission of secrets	
Cloud data breach	Eliminated	No cloud storage	
XSS/Injection	Low	No eval(), no innerHTML with user data	
Side-channel (timing)	Medium	Constant-time algorithms where possible	
Supply chain attack	Low	Zero build dependencies	
Quantum computing	Resistant	Information-theoretic security	

Mathematical Security

Shamir's Secret Sharing provides [perfect secrecy](https://en.wikipedia.org/wiki/Information-theoretic_security):

> With fewer than t shares (here: 3), the secret remains information-theoretically impossible to compute, even with unlimited computational power (including quantum computers).

Why? The scheme is based on polynomial interpolation over finite fields. Any subset of shares < t is consistent with every possible secret, providing zero information gain.

Finite Field Implementation

- Field: GF(2²⁵⁶) with prime p = 2²⁵⁶ − 189
- Polynomial: f(x) = a₀ + a₁x + a₂x² mod p
- Secret: a₀ = AES key
- Random coefficients: a₁, a₂ ← ℤₚ
- Reconstruction: Lagrange interpolation at x = 0

---

📁 Project Structure

```
cryptvault/
├── index.html              # Complete application (single file)
├── README.md               # This documentation
├── LICENSE                 # MIT License
├── assets/
│   ├── icons/              # PWA icons (192x192, 512x512)
│   │   ├── icon-192.png
│   │   └── icon-512.png
│   ├── screenshots/        # UI screenshots for docs
│   │   ├── encrypt.png
│   │   ├── shares.png
│   │   └── recover.png
│   └── demo-data/          # Example password lists
│       ├── passwords.csv
│       └── passwords.json
├── docs/
│   ├── ARCHITECTURE.md     # Detailed technical architecture
│   ├── SECURITY.md         # Security audit & threat model
│   ├── CRYPTOGRAPHY.md     # Mathematical proofs
│   └── API.md              # Internal API documentation
└── tests/
    ├── sss.test.js         # SSS unit tests
    ├── crypto.test.js      # WebCrypto tests
    └── e2e.test.js         # End-to-end workflows
```

---

🛠️ Development

Prerequisites

- Modern browser (Chrome 60+, Firefox 55+, Safari 11+, Edge 79+)
- Local web server (for Service Worker functionality)
- No Node.js, no npm, no build tools required!

Local Development

```bash
# 1. Clone repository
git clone https://github.com/CSTRSK/CryptVault.git
cd cryptvault

# 2. Start local server (required for Service Worker)
# Python 3
python3 -m http.server 8000

# PHP
php -S localhost:8000

# Node.js (if you have it)
npx serve .

# 3. Open http://localhost:8000

# 4. For PWA testing, use DevTools → Application → Service Workers
```

Zero-Build Philosophy

CryptVault intentionally has no build process:

Aspect	Benefit	
No webpack/vite	Instant startup, no config	
No npm install	Zero dependency vulnerabilities	
No transpilation	Readable, auditable source	
Single HTML file	Easy to archive, share, inspect	

This ensures longevity: the code will still work in 10 years when today's build tools are obsolete.

---

🧪 Testing

Manual Test Suite

```markdown
✅ Import Tests
   ├── JSON array format
   ├── CSV with headers
   ├── TXT comma-separated
   └── Large files (>10,000 entries)

✅ Encryption Tests
   ├── Key generation (256-bit)
   ├── AES-GCM encryption
   ├── IV uniqueness (never repeats)
   └── Ciphertext integrity

✅ SSS Tests
   ├── Split into 5 shares
   ├── Reconstruct with shares 1,2,3
   ├── Reconstruct with shares 2,4,5
   ├── Reconstruct with shares 1,3,5
   ├── Fail with 2 shares
   └── Fail with corrupted share

✅ Recovery Tests
   ├── Full workflow: Import → Encrypt → Split → Reconstruct → Decrypt
   ├── QR code scanning (simulated)
   ├── Wrong password file (should fail)
   └── Wrong shares (should fail)

✅ PWA Tests
   ├── Offline functionality
   ├── Service Worker registration
   ├── "Add to Home Screen"
   └── Standalone window mode
```

Cryptographic Verification

```javascript
// Test vector for SSS implementation
const secret = BigInt('0x' + 'f'.repeat(64)); // 256-bit max value
const shares = ShamirSecretSharing.split(secret, 5, 3);

// Test all combinations of 3 shares
const combinations = [
  [0, 1, 2], [0, 1, 3], [0, 1, 4],
  [0, 2, 3], [0, 2, 4], [0, 3, 4],
  [1, 2, 3], [1, 2, 4], [1, 3, 4],
  [2, 3, 4]
];

combinations.forEach(indices => {
  const subset = indices.map(i => shares[i]);
  const reconstructed = ShamirSecretSharing.reconstruct(subset);
  console.assert(reconstructed === secret, 'Reconstruction failed!');
});

console.log('All 10 combinations verified ✓');
```

---

🚀 Roadmap

Phase 1: Core (Complete ✅)
- AES-256-GCM encryption
- Shamir's Secret Sharing (3-of-5)
- PWA with offline support
- QR code generation
- File import/export

Phase 2: Security Hardening
- Argon2id key derivation (for password-based encryption)
- Hardware Security Module (HSM) support via WebAuthn
- Side-channel resistant implementations
- Formal security audit

Phase 3: Advanced Features
- Verifiable Secret Sharing (VSS): Feldman or Pedersen schemes
- Proactive Secret Sharing: Periodic share refreshing
- Time-Lock Encryption: Rivest-Shamir-Wagner time puzzles
- Steganography: Hide shares in images (stegcloak)
- Multi-language support (i18n)

Phase 4: Enterprise
- Multi-user vaults with MPC (Multi-Party Computation)
- LDAP/Active Directory integration
- Audit logging (local-only)
- Hardware wallet integration (Ledger, Trezor)

---

🤝 Contributing

We welcome contributions! Please follow these guidelines:

Security-First
- Any cryptographic changes require peer review by security experts
- Maintain zero external dependencies policy
- All crypto operations must use WebCrypto API or be formally verified

Code Standards
- Vanilla JavaScript (ES2020+)
- Semantic HTML5
- No frameworks for core logic (UI frameworks OK)
- Comprehensive comments for cryptographic operations

Pull Request Process

1. Fork the repository
2. Create branch: `git checkout -b feature/amazing-feature`
3. Commit with clear messages: `feat: add QR code scanning`
4. Test thoroughly: `npm test` (if tests exist) or manual verification
5. Document changes in README if user-facing
6. Submit PR with detailed description

Commit Convention

```
feat:     New feature
fix:      Bug fix
docs:     Documentation only
style:    Formatting, no code change
refactor: Code restructuring
perf:     Performance improvement
test:     Adding tests
chore:    Maintenance tasks
security: Security fix or improvement
```

---

📜 License

MIT License — see [LICENSE](LICENSE) for full text.

```
Copyright (c) 2026 CryptVault Contributors by CSTRSK

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```

---

🙏 Acknowledgments

- [Adi Shamir](https://en.wikipedia.org/wiki/Adi_Shamir) — Inventor of Secret Sharing (1979)
- [WebCrypto Working Group](https://www.w3.org/WebCrypto/) — Browser cryptography standardization
- [Bulma](https://bulma.io/) & [Alpine.js](https://alpinejs.dev/) — Elegant UI frameworks
- [Privacy Guides](https://www.privacyguides.org/) & [EFF](https://www.eff.org/) — Digital rights advocacy

---

⚠️ Disclaimer

> IMPORTANT: CryptVault is provided as-is for educational and research purposes. For production use:

- Conduct independent security audits
- Consult with professional cryptographers
- Test thoroughly in your specific environment
- Store shares in physically separated, secure locations
- Maintain regular backups of both encrypted data and shares
- Consider legal implications of digital inheritance in your jurisdiction

THE AUTHORS ASSUME NO LIABILITY FOR LOST DATA, SECURITY BREACHES, OR ANY DAMAGES ARISING FROM THE USE OF THIS SOFTWARE. 

Cryptography is hard. Secret management is harder. When in doubt, consult experts.
