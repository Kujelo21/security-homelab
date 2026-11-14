# Security Homelab

> A local, isolated lab for learning web security and threat detection (SOC) on my own machine.

![status](https://img.shields.io/badge/status-in%20progress-yellow)

**Project type:** Educational

## 🎯 Goal

Build a safe, reproducible environment where I can practise attacking and defending
deliberately vulnerable applications and later learn log analysis and detection (SIEM).

## ❓ Problem

Security skills can't be learned by testing real websites without permission. That is illegal.
I need my own lab: easy to start, easy to reset and isolated from the network.

## 🧰 Technologies

| Technology | Purpose |
|---|---|
| Windows 11 + WSL2 (Ubuntu) | Linux environment on my laptop |
| Docker Desktop | Running lab applications in containers |
| Docker Compose | Starting the whole lab with one command |
| OWASP Juice Shop | Deliberately vulnerable web shop (OWASP Top 10) |

## 🏗️ Architecture

```
Windows 11 (host)
 └── Browser ──► http://localhost:3000
                     │  (127.0.0.1 only, not exposed to LAN)
 └── WSL2: Ubuntu
      └── Docker
           └── juice-shop container (port 3000)
```

## 🔐 Security decisions

| Decision | Why |
|---|---|
| Ports bound to `127.0.0.1` | Vulnerable apps must never be reachable from the Wi-Fi network |
| Containers instead of installing apps directly | Easy reset, nothing left on the system |
| Lab stopped when not in use | Smaller attack surface and less RAM usage |
| Note: `docker` group membership | It is effectively root access, acceptable on a personal lab machine but a known privilege-escalation vector on servers |

## 🚀 Getting started

Requirements: Docker (Docker Desktop with WSL2 integration on Windows).

```bash
git clone git@github.com:Kujelo21/security-homelab.git
cd security-homelab
docker compose up -d
```

Open **http://localhost:3000** in your browser.

Stop the lab:

```bash
docker compose down
```

## 📝 Writeups

| # | Challenge | Topic |
|---|---|---|
| 01 | [Score Board](writeups/juice-shop/01-score-board.md) | Security through obscurity, Broken Access Control |

## 🗺️ Roadmap

- [x] Docker + OWASP Juice Shop
- [ ] More Juice Shop writeups (OWASP Top 10)
- [ ] DVWA
- [ ] Kali Linux VM
- [ ] Wazuh SIEM + log analysis

## 📚 What I learned

- Setting up WSL2 and Docker Desktop on Windows
- Difference between binding a port to `127.0.0.1` and `0.0.0.0`
- Basic Docker commands (`run`, `ps`, `stop`, `compose`)

## ⚖️ Disclaimer

Educational project. All testing is performed only in this local lab.