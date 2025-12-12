# Week 3: Application Selection for Performance Testing

[← Back to Home](index.md)

## Introduction
This week I had to pick which applications I'd install on my server for performance testing. The goal was to choose apps that stress different parts of the system.

## 1. Application Selection Matrix

I chose three applications that each put pressure on different system resources:

| Application | What It Stresses | Why I Chose It |
| :--- | :--- | :--- |
| **Apache Web Server** | Network and CPU | It's the most popular web server and good for testing concurrent connections |
| **MySQL Database** | Disk I/O and Memory | Databases are always reading and writing to disk |
| **Sysbench** | Pure CPU | Just hammers the CPU with calculations, gives me a baseline |

## 2. Installation Documentation

Here's exactly how I installed each application through SSH:

### Installing Apache Web Server
```bash
# First update the package list
sudo apt update

# Install Apache
sudo apt install apache2 -y

# Check if it's running
sudo systemctl status apache2

# Allow it through the firewall
sudo ufw allow 'Apache'
```

### Installing MySQL Server
```bash
# Install MySQL
sudo apt install mysql-server -y

# Run the security wizard
sudo mysql_secure_installation

# Check if it's running
sudo systemctl status mysql
```

### Installing Sysbench
```bash
# Install sysbench
sudo apt install sysbench -y

# Check the version
sysbench --version
```

## 3. Expected Resource Profiles

Based on what I know about these applications:

| Application | CPU | Memory | Disk | Network |
| :--- | :--- | :--- | :--- | :--- |
| Apache (idle) | Almost nothing | ~50MB | Nothing | Nothing |
| Apache (busy) | High | Medium | Low | High |
| MySQL (idle) | Almost nothing | ~300MB for caching | Nothing | Nothing |
| MySQL (busy) | Medium | High | Very high | Low-Medium |
| Sysbench CPU | 100% all cores | Low | Nothing | Nothing |

## 4. Monitoring Strategy

My plan for measuring performance:

**Step 1: Get baseline numbers**
```bash
vmstat 1 300   # 5 minutes of readings
```

**Step 2: Run stress tests**
```bash
# For Apache
ab -n 1000 -c 10 http://localhost/

# For Sysbench
sysbench cpu --cpu-max-prime=20000 run
```

**Step 3: Record everything**
I'll save all the output to files so I can make graphs later.

## Reflection
Choosing applications wasn't too hard, but understanding WHAT resources they use was useful. I learned that different workloads stress different parts of the system.

---
[← Week 2](week2.md) | [Next: Week 4 →](week4.md)
