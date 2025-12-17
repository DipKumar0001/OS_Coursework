# Week 4: Initial System Configuration & Security

[← Back to Home](index.md)

## Introduction
This was the week where I actually started implementing the security measures I planned in Week 2. Everything was done remotely through SSH.

## 1. SSH Configuration

### Step 1: Generate SSH Keys on my Mac
I started by generating an Ed25519 SSH key pair on my local machine.
![Generate SSH Keys](images/Screenshot%202025-12-17%20at%2018.15.27.png)

```bash
ssh-keygen -t ed25519 -f ~/.ssh/os_course_key
```
This creates a private key (stays on my Mac) and a public key (goes on the server).

### Step 2: Copy the Public Key to the Server
Next, I transferred the public key to the remote server.
![Copy Public Key](images/Screenshot%202025-12-17%20at%2018.15.42.png)

```bash
ssh-copy-id -i ~/.ssh/os_course_key.pub monkey@10.41.17.2
```

### Step 3: Harden the SSH Config on the Server
I edited `/etc/ssh/sshd_config` to disable root login and password authentication for better security.
![Harden SSH Config](images/Screenshot%202025-12-17%20at%2019.27.45.png)

```bash
PermitRootLogin no          # Can't log in as root anymore
PasswordAuthentication no   # Must use key, no passwords
PubkeyAuthentication yes    # Enable key-based auth
```

### Step 4: Restart SSH
After saving the changes, I restarted the SSH service to apply the new configuration.
![Restart SSH](images/Screenshot%202025-12-17%20at%2019.31.02.png)

```bash
sudo systemctl reload ssh
```

### Step 5: Testing it worked
Finally, I verified the connection works without a password.
![Test Connection](images/Screenshot%202025-12-17%20at%2019.31.47.png)

```bash
ssh -i ~/.ssh/os_course_key monkey@10.41.17.2
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
