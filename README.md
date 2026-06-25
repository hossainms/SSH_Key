# 🔐 SSH & Public-Key Cryptography — A Hands-On Learning Bundle

A beginner-friendly set of guides on SSH keys and public-key cryptography, plus an
interactive quiz (built with `ipywidgets` + `Voilà`, deployable via Binder/Docker).

## 📚 Suggested reading order

1. **[Understand SSH Public and Private Key.md](./Understand%20SSH%20Public%20and%20Private%20Key.md)** — public/private-key cryptography concepts (GPG example).
2. **[How SSH Authentication Works.md](./How%20SSH%20Authentication%20Works.md)** — host keys, the login handshake, `authorized_keys`, file permissions.
3. **[Setting Up SSH Keys.md](./Setting%20Up%20SSH%20Keys.md)** — generate a key, add it to GitHub & HPC, `ssh-agent`, config.
4. **[SSH_Key_Renewal_and_Agent_Setup.md](./SSH_Key_Renewal_and_Agent_Setup.md)** — rotate/replace a key and revoke the old one.
5. **[ssh_crisis_handling_guide.md](./ssh_crisis_handling_guide.md)** — lost key, forgotten passphrase, access from another machine.
6. **[SSH Troubleshooting and Cheat Sheet.md](./SSH%20Troubleshooting%20and%20Cheat%20Sheet.md)** — fix common errors; quick command reference.
7. **Test your understanding** — take the interactive quiz (below).

---

## 🚀 Quick Launch via Binder

[![Launch on Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/<your-username>/<your-repo>/main?urlpath=voila/render/voila/render/Test-Your_Understanding-1.ipynb)

> 🔗 Replace `<your-username>` and `<your-repo>` with your GitHub details if you fork.

## 📦 Project Structure

```
🗂️ Repo Structure:
.
├── Test-Your_Understanding-1.ipynb
├── Binder/
│   └── Dockerfile
├── requirements.txt
└── README.md
```

## 🛠️ Local Development

### 1. Run with Docker
```bash
docker build -t quiz-app .
docker run -p 8888:8888 quiz-app
```
Visit: [http://localhost:8888](http://localhost:8888)

### 2. Run Locally (Without Docker)
```bash
pip install voila ipywidgets
voila voila/render/Test-Your_Understanding-1.ipynb
```
🔍 URL Explanation:

- `urlpath=voila/render/` tells Binder to launch the notebook using **Voila**.
- Since the notebook is at the repo root, the path is just the notebook filename:
  `Test-Your_Understanding-1.ipynb`

📌 Summary:

- The correct Binder launch URL is:
  
https://mybinder.org/v2/gh/hossainms/SSH_Key/main?urlpath=voila/render/Test-Your_Understanding-1.ipynb

## 📚 Requirements

```
voila
ipywidgets
```

## 📖 About

This project serves as a minimal, reproducible example for deploying educational content using interactive widgets and sharing it via Binder or Docker.
