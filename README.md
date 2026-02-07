# 🔐 CryptVault

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Offline First](https://img.shields.io/badge/Offline-First-success)](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)
[![Crypto: AES-256-GCM](https://img.shields.io/badge/Crypto-AES--256--GCM-blueviolet)](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto/encrypt)
[![SSS: 3-of-5](https://img.shields.io/badge/Shamir-3--of--5-orange)](https://en.wikipedia.org/wiki/Shamir%27s_Secret_Sharing)

> **Eine vollständig lokale, serverlose PWA für sicheres Passwort-Management mit Shamir's Secret Sharing.**

CryptVault ist eine Progressive Web App (PWA), die Passwortlisten verschlüsselt und den Verschlüsselungsschlüssel mittels [Shamir's Secret Sharing (SSS)](https://en.wikipedia.org/wiki/Shamir%27s_Secret_Sharing) in 5 unabhängige Teile aufteilt. Zur Wiederherstellung sind mindestens 3 dieser 5 Schlüssel erforderlich – mathematisch beweisbar sicher, 100% offline, zero-knowledge.

---

## ✨ Features

### 🔒 Sicherheit
- **AES-256-GCM** Verschlüsselung via WebCrypto API
- **Shamir's Secret Sharing (3-of-5)** – informationstheoretisch sicher
- **Zero-Knowledge**: Kein Server, keine Cloud, keine Datenübertragung
- **Memory-Only**: Sensitive Daten verlassen nie den Arbeitsspeicher
- **Perfect Secrecy**: Mit <3 Shares ist der Schlüssel mathematisch unmöglich zu berechnen

### 🌐 Offline-First PWA
- Funktioniert komplett ohne Internetverbindung
- Service Worker für Asset-Caching
- Installierbar auf Desktop & Mobile (Add to Home Screen)
- Keine Backend-Infrastruktur nötig

### 📱 Benutzerfreundlich
- **Bulma CSS** Framework für responsive UI
- **Alpine.js** für reaktive Komponenten ohne Build-Step
- QR-Code-Generierung für einfache Share-Übertragung
- Drag-&-Drop Datei-Import (JSON, CSV, TXT)
- Dark Mode Optimierung

---

## 🚀 Schnellstart

### Option 1: Direkt nutzen (Empfohlen)
```bash
# Repository klonen
git clone https://github.com/dein-username/cryptvault.git

# In das Verzeichnis wechseln
cd cryptvault

# Lokaler Server starten (z.B. via Python)
python3 -m http.server 8000

# Im Browser öffnen: http://localhost:8000
