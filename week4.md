# Week 4: Initial System Configuration & Security

[← Back to Home](index.md)

## Introduction
This was the week where I actually started implementing the security measures I planned in Week 2. Everything was done remotely through SSH.

## 1. SSH Configuration

### Step 1: Generate SSH Keys on my Mac
```bash
ssh-keygen -t ed25519 -f ~/.ssh/os_course_key
```
This creates a private key (stays on my Mac) and a public key (goes on the server).

### Step 2: Copy the Public Key to the Server
```bash
ssh-copy-id -i ~/.ssh/os_course_key.pub dipesh@192.168.56.10
```

### Step 3: Harden the SSH Config on the Server
I edited `/etc/ssh/sshd_config`:
```bash
PermitRootLogin no          # Can't log in as root anymore
PasswordAuthentication no   # Must use key, no passwords
PubkeyAuthentication yes    # Enable key-based auth
```

### Step 4: Restart SSH
```bash
sudo systemctl reload ssh
```

**Testing it worked:**
```bash
ssh -i ~/.ssh/os_course_key dipesh@192.168.56.10
# No password prompt - it just connects!
```

## 2. Firewall Configuration

The idea is "deny everything by default, then allow only what you need".

```bash
# Block all incoming traffic by default
sudo ufw default deny incoming

# Allow all outgoing
sudo ufw default allow outgoing

# Now punch holes for specific services
sudo ufw allow ssh          # Port 22
sudo ufw allow http         # Port 80 for Apache
sudo ufw allow 3306/tcp     # MySQL port

# Turn it on
sudo ufw enable
```

### Checking the Rules
```bash
$ sudo ufw status verbose
Status: active
Default: deny (incoming), allow (outgoing)

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
3306/tcp                   ALLOW IN    Anywhere
```

## 3. User Management

I created a separate admin user instead of using root.

```bash
# Create the user
sudo adduser admin_user

# Give sudo access
sudo usermod -aG sudo admin_user

# Lock the root account
sudo passwd -l root
```

## Reflection
Setting up key-based SSH was a bit confusing at first. I accidentally locked myself out once when I disabled password auth before copying my key over! Had to use the VirtualBox console to fix it. Lesson learned - always test the key works before disabling passwords.

---
[← Week 3](week3.md) | [Next: Week 5 →](week5.md)
