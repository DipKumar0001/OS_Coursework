# Week 2: Security Planning and Testing Methodology

[← Back to Home](index.md)

## Introduction
This week I focused on planning out my security setup and figuring out how I'll test the server's performance. Before jumping into configurations, I wanted to have a clear plan.

## 1. Performance Testing Plan

### My Approach
To test how well my server performs, I'm going to use a mix of different tools. Some will generate fake load on the system, and others will measure how the system responds.

### Tools I'm Planning to Use

**For collecting metrics:**
- `mpstat` and `top` - to check CPU usage
- `free` and `vmstat` - to check memory
- `iostat` - to check disk read/write activity
- `iftop` - to check network traffic

**For generating load:**
- `sysbench` - a benchmarking tool that can stress the CPU
- `ab` (Apache Bench) - to simulate lots of web requests

### How I'll Collect Data
1. First I'll take baseline readings when the server is doing nothing (idle for about 5 minutes)
2. Then I'll run stress tests and take readings during the load (about 10 minutes)
3. Compare the two sets of data to see how the system behaves under pressure

## 2. Security Configuration Checklist

Here's what I'm planning to configure:

| Area | What to Do | How to Do It | Done? |
| :--- | :--- | :--- | :--- |
| SSH | Stop root from logging in directly | Change `PermitRootLogin no` in sshd_config | ☐ |
| SSH | Only allow key-based login | Set `PasswordAuthentication no` | ☐ |
| Firewall | Block everything except what I need | Use `ufw` to allow only SSH and web ports | ☐ |
| Users | Create a normal admin account | Make a user and add to sudo group | ☐ |
| Access Control | Limit what apps can do | Enable AppArmor profiles | ☐ |
| Updates | Keep server patched automatically | Set up unattended-upgrades | ☐ |
| Protection | Block attackers | Install fail2ban | ☐ |

## 3. Threat Model

I tried to think like an attacker - what could go wrong with my server?

### STRIDE Methodology
I used STRIDE which stands for Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, and Elevation of Privilege.

### Threats I Identified

**Threat 1: Someone pretending to be me (Spoofing)**
- **What could happen:** An attacker could try to log in as admin
- **How I'll prevent it:** Use SSH keys instead of passwords - much harder to crack. Also restrict which IPs can even connect.

**Threat 2: Brute force attacks (Denial of Service)**
- **What could happen:** Someone tries thousands of password combinations
- **How I'll prevent it:** Install fail2ban to automatically block IPs that fail too many times

**Threat 3: Compromised service taking over (Privilege Escalation)**
- **What could happen:** If Apache or MySQL gets hacked, the attacker could try to get root access
- **How I'll prevent it:** Use AppArmor to lock down what each service can access

## Reflection
Writing out the security plan before doing anything felt a bit tedious at first, but it actually helped me understand WHY each security measure is important, not just HOW to do it.

---
[← Week 1](week1.md) | [Next: Week 3 →](week3.md)
