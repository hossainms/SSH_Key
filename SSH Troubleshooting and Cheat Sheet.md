# 🧰 SSH Troubleshooting & Cheat Sheet

A quick reference for confirming SSH works and fixing the common failures.

---

## ✅ Confirm it works

```bash
# GitHub — expect: "Hi <username>! You've successfully authenticated..."
ssh -T git@github.com

# HPC / server (using a Host alias from ~/.ssh/config)
ssh bigpurple
```

## 🔍 Debug a failing connection

```bash
ssh -vvv you@bigpurple.edu
```

Read the verbose log for `Offering public key:` (which key was tried) and
`Authentications that can continue:` (what the server accepts).

---

## ⚠️ Common errors and fixes

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| `Permission denied (publickey)` | Your public key isn't in the server's `authorized_keys` / GitHub, or the wrong key was offered | Re-add the `.pub` key; set `IdentityFile` in `~/.ssh/config`; run `ssh -vvv` to see which key is used |
| `UNPROTECTED PRIVATE KEY FILE` / `bad permissions` | Private key is world/group readable | `chmod 600 ~/.ssh/id_ed25519*` and `chmod 700 ~/.ssh` |
| `REMOTE HOST IDENTIFICATION HAS CHANGED` / `Host key verification failed` | Host key changed (server rebuilt) or possible MITM | Verify it's legitimate, then `ssh-keygen -R bigpurple.edu` and reconnect |
| `Too many authentication failures` | The agent is offering many keys before the right one | Add `IdentitiesOnly yes` + the correct `IdentityFile` in `~/.ssh/config` |
| `Could not open a connection to your authentication agent` | `ssh-agent` not running | `eval "$(ssh-agent -s)"`, then `ssh-add` |
| `The agent has no identities` | Key not loaded into the agent | `ssh-add ~/.ssh/id_ed25519_saf_bigpurple` |
| Passphrase prompt every time | Key not persisted in the agent | Add `AddKeysToAgent yes` (and `UseKeychain yes` on macOS) to `~/.ssh/config` |

---

## 📋 Command cheat sheet

```bash
# Generate a modern key (one per device/service)
ssh-keygen -t ed25519 -C "you@purpose" -f ~/.ssh/id_ed25519_purpose

# Install your public key on a server
ssh-copy-id -i ~/.ssh/id_ed25519_purpose.pub your_username@bigpurple.edu

# Load key into the agent (persist for the session)
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_purpose          # macOS: ssh-add --apple-use-keychain ...

ssh-add -l                                  # list loaded keys
ssh -T git@github.com                       # test GitHub auth
ssh-keygen -R bigpurple.edu                 # remove a stale host key
ssh-keygen -lf ~/.ssh/id_ed25519_purpose.pub  # show a key's fingerprint
```

---

## 📖 Glossary

- **Private key** — secret half; stays on your machine; signs the login challenge.
- **Public key** — shareable half; goes in `authorized_keys` / GitHub.
- **Passphrase** — encrypts the private key on disk; entered locally, never sent.
- **`authorized_keys`** — server's allow-list of public keys.
- **`known_hosts`** — your record of trusted server host keys.
- **Fingerprint** — short hash of a key used to verify it by eye.
- **`ssh-agent`** — holds your unlocked key in memory so you type the passphrase once.
