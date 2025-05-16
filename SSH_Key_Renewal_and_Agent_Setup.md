
# 🔐 Renew Your Old SSH Key: A Practical Guide

## 🔁 Regenerating and Replacing an SSH Key: A Realistic Scenario

### 👥 Scenario

Safir discovers he has an old SSH key in `~/.ssh/`:

- `id_ed25519_bigpurple_saf`
- `id_ed25519_bigpurple_saf.pub`

He wants to replace them with a new, clearly named key:

- `id_ed25519_saf_bigpurple`
- `id_ed25519_saf_bigpurple.pub`

Safir also wants to stop typing the passphrase every time he connects.

---

### ✅ Step-by-Step Instructions

#### 🔥 Step 1: Delete the Old Key Pair

```bash
rm ~/.ssh/id_ed25519_bigpurple_saf ~/.ssh/id_ed25519_bigpurple_saf.pub
```

> ⚠️ Tip: Always check with `ls ~/.ssh/` before deleting.

---

#### 🔐 Step 2: Generate a New Key Pair

```bash
ssh-keygen -t ed25519 -C "Safir@bigpurple" -f ~/.ssh/id_ed25519_saf_bigpurple
```

- Choose a strong passphrase when prompted.
- Creates:
  - `~/.ssh/id_ed25519_saf_bigpurple`
  - `~/.ssh/id_ed25519_saf_bigpurple.pub`

---

#### 🧠 Step 3: Avoid Repeating Passphrase- Load the Key into `ssh-agent`

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_saf_bigpurple
```

> 💡 Optional: Use `keychain` or desktop agents for persistence across reboots.

---

#### 📤 Step 4: Upload the Public Key to BigPurple

**Option 1 (Preferred):**

```bash
ssh-copy-id -i ~/.ssh/id_ed25519_saf_bigpurple.pub safir16@bigpurple.nyumc.org
```

**Option 2 (Manual):**

```bash
scp ~/.ssh/id_ed25519_saf_bigpurple.pub safir16@bigpurple.nyumc.org:~
# Then on BigPurple:
cat ~/id_ed25519_saf_bigpurple.pub >> ~/.ssh/authorized_keys
```

---

#### ⚙️ Step 5: Configure SSH for Simpler Login

Edit your `~/.ssh/config`:

```ssh
Host bigpurple
    HostName safir_bigpurple
    User safir16
    IdentityFile ~/.ssh/id_ed25519_saf_bigpurple
    Port 22
    ForwardAgent yes
    Compression yes
    TCPKeepAlive yes
    ServerAliveInterval 60
    ServerAliveCountMax 120
    ControlMaster auto
    ControlPath ~/.ssh/cm-%r@%h:%p
    ControlPersist 10m
```

Then just connect with:

```bash
ssh bigpurple
```

No retyping passphrase, no clutter, no stress.
