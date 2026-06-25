# 🔑 How SSH Authentication Works

A short, accurate model of what happens when you run `ssh you@server` — so the
setup steps in the other guides make sense. SSH does **two independent jobs** on
every connection:

1. **The server proves itself** (host verification) and a secure channel is built.
2. **You prove yourself** (user authentication) with your key.

> ℹ️ This is different from encrypting a message to someone (the GPG model in
> *Understanding Public/Private-Key Cryptography*). SSH login **signs a
> challenge**; it does not encrypt a letter to the server.

---

## 1. The server proves itself first — host keys & `known_hosts`

Every server has its own **host key pair**. The first time you connect, SSH shows
the server's fingerprint and asks you to trust it (trust on first use):

```text
The authenticity of host 'bigpurple.edu' can't be established.
ED25519 key fingerprint is SHA256:abcd...wxyz.
Are you sure you want to continue connecting (yes/no)?
```

- Type `yes` **only after** confirming the fingerprint out-of-band (your HPC docs
  or, for GitHub, its published key fingerprints). This step is what protects you
  from a **man-in-the-middle** server impersonation.
- The accepted host key is saved to `~/.ssh/known_hosts`. On later connections SSH
  checks it matches. If it ever changes you'll see:

```text
@@@ WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! @@@
```

That means the server was rebuilt **or** someone is intercepting you. Verify
before clearing the old entry with `ssh-keygen -R bigpurple.edu`.

---

## 2. You prove yourself — challenge–response, not encryption

When you authenticate with a key:

1. Your client offers your **public key**; the server checks it is listed in your
   `~/.ssh/authorized_keys` on that server.
2. The server sends a **one-time challenge** bound to this session.
3. Your client **signs** it with your **private key** (unlocked by your
   passphrase, locally).
4. The server **verifies** the signature with your public key.

Your private key **never leaves your machine**, and your passphrase **never
crosses the network**. The channel's confidentiality comes from a *separate*
ephemeral key exchange (Diffie–Hellman), independent of your user key.

```text
client                                   server
  | ---- offer public key -------------->|  is it in authorized_keys? ✓
  | <--- random challenge ---------------|
  | ---- sign challenge (private key) -->|  verify signature with public key ✓
  | <=== authenticated, shell opens ====>|
```

---

## 3. `authorized_keys` — the server's allow-list

Your **public** key is appended (one per line) to `~/.ssh/authorized_keys` on the
server. **Anyone holding the matching private key can log in.** To revoke access,
delete that line. To rotate a key, add the new line **and remove the old one**.

---

## 4. File permissions (SSH refuses loose permissions)

SSH ignores keys/configs that are too open. Set these once:

| Path | Mode | |
| --- | --- | --- |
| `~/.ssh` | `700` | `chmod 700 ~/.ssh` |
| private key (`id_ed25519…`) | `600` | `chmod 600 ~/.ssh/id_ed25519*` |
| public key (`*.pub`) | `644` | |
| `~/.ssh/authorized_keys` | `600` | on the server |
| `~/.ssh/config` | `600` | |
| `~/.ssh/known_hosts` | `644` | |

Wrong permissions cause `UNPROTECTED PRIVATE KEY FILE` or silently ignored
`authorized_keys` (→ `Permission denied (publickey)`).

---

*Next: see [SSH Troubleshooting and Cheat Sheet](./SSH%20Troubleshooting%20and%20Cheat%20Sheet.md) when something doesn't connect.*
