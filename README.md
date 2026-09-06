# Vault Exchange

Secure file sharing in the browser, using RSA for key exchange and AES for the actual file data.

## Overview

Vault Exchange lets one person encrypt a file for a specific recipient and lets that recipient decrypt it, without ever sending the file or its encryption key in a form that's readable in transit.

It uses **hybrid encryption**, the same pattern behind HTTPS and PGP:

- **AES-256-GCM** encrypts the file itself. Fast, and works on files of any size.
- **RSA-OAEP (2048-bit)** encrypts only the small AES key, using the recipient's public key. Only their matching private key can unlock it.

Everything — key generation, encryption, decryption — runs client-side in the browser via the Web Crypto API. No server, no backend, no data ever leaves the page.

## Features

- Generate an RSA public/private key pair locally
- Encrypt any file for a recipient using their public key
- Download the encrypted file plus the wrapped AES key
- Decrypt a received file using your private key and the wrapped key
- No installation, no dependencies, no backend

## Project structure

```
.
├── index.html   # entire application (HTML, CSS, JS)
└── README.md
```

## Getting started

### Run locally

Just open `index.html` in any modern browser (Chrome, Firefox, Edge, Safari). No build step, no server required.

### Deploy with GitHub Pages

```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPO.git
cd YOUR-REPO
cp /path/to/index.html .
git add index.html
git commit -m "Add secure file sharing demo"
git push
```

Then in the repo: **Settings → Pages → Source: Deploy from branch → main / (root)**.

The site will be live at:

```
https://YOUR-USERNAME.github.io/YOUR-REPO/
```

If your repo already serves a different `index.html` from `main`, rename this file (e.g. `vault-exchange.html`) or place it in its own subfolder and link to it.

## How it works

1. **Generate keys** — the sender or recipient generates an RSA key pair. The private key stays in the browser; the public key can be shared freely.
2. **Encrypt** — the sender picks a file and the recipient's public key. A random AES-256 key is generated, used to encrypt the file, and then that AES key is itself encrypted ("wrapped") with the recipient's RSA public key.
3. **Share** — the sender passes along the encrypted file and the wrapped key (email, chat, USB drive — the channel doesn't need to be trusted).
4. **Decrypt** — the recipient uses their RSA private key to unwrap the AES key, then uses that AES key to decrypt the file.

## Security notes

- This is a functional demonstration of correct cryptographic patterns using standard, audited browser APIs (`SubtleCrypto`) — it is not a hardened, production key-management system.
- Private keys are never transmitted, but they are also not persisted anywhere; closing the tab discards them unless you've saved a copy.
- Treat the private key textbox like a password field: don't paste your private key into an untrusted page.

## License

MIT
