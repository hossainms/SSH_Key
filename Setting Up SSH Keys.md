# Setting Up SSH Keys for GitHub and HPC Access: A Realistic Dialogue

*A practical, fun walkthrough in conversation format.*

---

## 🎭 Characters

* **Safir** – New to HPC and Git, wants secure and easy SSH access.
* **Taysir** – Lab senior, seasoned in secure shell sorcery.

---

### 🔐 Generating the SSH Key

**Safir:**
Hey Taysir, I’m tired of typing my GitHub and BigPurple passwords every time I push or ssh. Any solution?

**Taysir:**
Yes. Time to grow up and use SSH keys. They’re safer and smarter.

**Safir:**
Okay, how do I create one?

**Taysir:**
Use ed25519 — modern, fast, and secure:

```bash
ssh-keygen -t ed25519 -C "GitHub and BigPurple access" -f ~/.ssh/id_bigpurple
```

**Safir:**
What’s the passphrase part?

**Taysir:**
It encrypts your private key. Use one. If your laptop gets stolen, your keys stay locked.

---

### 📤 Uploading the Public Key to GitHub

**Safir:**
Got it. Now for GitHub?

**Taysir:**
Copy the contents of your public key:

```bash
cat ~/.ssh/id_bigpurple.pub
```

Paste it into GitHub under **Settings → SSH and GPG Keys → New SSH Key**.

---

### 🏰 Uploading Public Key to BigPurple (HPC)

**Safir:**
And BigPurple?

**Taysir:**
Try this first:

```bash
ssh-copy-id -i ~/.ssh/id_bigpurple.pub your_username@bigpurple.edu
```

If that fails (some HPCs block password SSH), use:

```bash
scp ~/.ssh/id_bigpurple.pub your_username@bigpurple.edu:~
ssh your_username@bigpurple.edu
mkdir -p ~/.ssh && chmod 700 ~/.ssh
cat ~/id_bigpurple.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
rm ~/id_bigpurple.pub
```

---

### ⚙️ SSH Config for Simplicity

**Safir:**
Can I avoid typing long hostnames?

**Taysir:**
Yes, create or edit `~/.ssh/config`:

```ini
Host bigpurple
  HostName bigpurple.edu
  User your_username
  IdentityFile ~/.ssh/id_bigpurple
  IdentitiesOnly yes
```

Then you can simply:

```bash
ssh bigpurple
```

---

### 🧠 Avoid Typing Passphrase Every Time

**Safir:**
But I still have to type the passphrase?

**Taysir:**
Only once per session. Use `ssh-agent`:

**A.	Start the agent:**
```bash
eval "$(ssh-agent -s)"
```
**B.	Add your private key:**
```
ssh-add ~/.ssh/id_bigpurple
```

**Taysir:**
On macOS:

```bash
ssh-add --apple-use-keychain ~/.ssh/id_bigpurple
```

On Windows:

```powershell
Start-Service ssh-agent
ssh-add C:\Users\you\.ssh\id_bigpurple
```

---

### 🚀 Done

**Safir:**
So now: `ssh bigpurple` — no password. And I can `git push` without credential prompts?

**Taysir:**
Exactly. You've joined the Keymasters. Now go automate something.

---

*✅ Save this `.md` file in your GitHub repo as part of your onboarding docs for team members or workshop participants.*
