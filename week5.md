# Week 5: Advanced Security and Monitoring

[← Back to Home](index.md)

## Introduction
This week I worked on more advanced security - AppArmor, fail2ban, and automatic updates. I also wrote the two scripts required for the assessment.

## 1. Access Control with AppArmor

AppArmor is a "Mandatory Access Control" system. It limits what each program can do, even if that program gets hacked.

Ubuntu Server 24.04 already has AppArmor enabled by default!

```bash
# Check AppArmor status
sudo aa-status
```

Output showed:
- 34 profiles loaded
- All in enforce mode
- MySQL and other services are being monitored

```bash
# Install AppArmor tools
sudo apt install apparmor-utils
```

## 2. Automatic Security Updates

I configured `unattended-upgrades` to automatically install security patches.

```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
```

Config file at `/etc/apt/apt.conf.d/50unattended-upgrades`:
```bash
Unattended-Upgrade::Allowed-Origins {
    "${distro_id}:${distro_codename}";
    "${distro_id}:${distro_codename}-security";
};
Unattended-Upgrade::Automatic-Reboot "false";
```

## 3. Intrusion Detection with Fail2ban

Fail2ban watches log files for failed login attempts and blocks suspicious IPs.

```bash
# Install
sudo apt install fail2ban -y

# Create config
sudo nano /etc/fail2ban/jail.local
```

Config:
```ini
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600
```

```bash
# Restart
sudo systemctl restart fail2ban
```

## 4. Automation Scripts

### Script 1: security-baseline.sh
This runs ON the server and checks if all security settings are correct:
- UFW is active
- SSH has root login and password auth disabled
- AppArmor is running
- Fail2ban is running
- Unattended upgrades is active

### Script 2: monitor-server.sh
This runs FROM my Mac and connects to the server via SSH to grab stats:
- Load average
- Memory usage
- Disk space
- Open network connections

## Reflection
Fail2ban is really clever - I tested it by deliberately failing SSH logins and watched my IP get banned! Had to wait an hour or manually unban myself.

---
[← Week 4](week4.md) | [Next: Week 6 →](week6.md)
