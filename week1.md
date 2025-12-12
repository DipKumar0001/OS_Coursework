# Week 1: System Planning and Distribution Selection

[← Back to Home](index.md)

## Introduction
This week I set up the basic foundation for my Linux server project. The main goal was to plan out my system and decide which Linux distribution to use.

## 1. System Architecture Diagram

Here's how my setup looks - I have two systems talking to each other:

```
┌─────────────────────────────────────────────────────────────────┐
│                        WORKSTATION                              │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  My Mac Host Machine                                    │   │
│  │  - SSH Client (Terminal)                                │   │
│  │  - Browser for testing web server                       │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ SSH (Port 22)
                              │ HTTP (Port 80)
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      SERVER (VirtualBox VM)                     │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  Ubuntu Server 24.04 LTS (Headless)                     │   │
│  │  - SSH Server                                           │   │
│  │  - Apache Web Server                                    │   │
│  │  - MySQL Database                                       │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

## 2. Distribution Selection Justification

**My Choice:** Ubuntu Server 24.04 LTS

| Feature | Ubuntu Server 24.04 LTS | CentOS Stream | Debian Stable |
| :--- | :--- | :--- | :--- |
| **Package Manager** | APT - I'm already familiar with this | DNF - More common in enterprise | APT - Same as Ubuntu but older packages |
| **Support** | 5 years of updates | Rolling release | Long support but older packages |
| **Documentation** | Loads of tutorials online | Good but enterprise focused | Great but sometimes hard to follow |

**Why I picked Ubuntu:**
- I've used Ubuntu Desktop before so the commands are familiar
- There's so much help available online when I get stuck
- The 5 year LTS means I won't have to worry about updates breaking things

## 3. Workstation Configuration

**I went with Option C - Hybrid Approach**

I'm using my Mac as the main workstation for administering the server:
- **SSH Client:** The built-in Terminal app on macOS
- **File Transfer:** Using `scp` to copy files

## 4. Network Configuration

| Setting | What I Set | Why |
| :--- | :--- | :--- |
| **Adapter 1** | NAT | So the server can get online |
| **Adapter 2** | Host-Only | Creates a private network for SSH |
| **Server IP** | 192.168.56.10 (static) | Fixed IP |

## 5. System Specifications

```bash
$ uname -a
Linux server-01 6.8.0-31-generic #31-Ubuntu SMP x86_64 GNU/Linux

$ free -h
               total        used        free
Mem:           3.8Gi       512Mi       2.1Gi

$ lsb_release -a
Distributor ID: Ubuntu
Description:    Ubuntu 24.04 LTS
```

## Reflection
Setting up the VM took a bit of trial and error. Adding the second network adapter fixed internet access issues.

---
[Next: Week 2 →](week2.md)
