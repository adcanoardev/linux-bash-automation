# Server Baseline Configuration

Initial configuration steps after provisioning a fresh Linux server (Debian/Ubuntu based).

This document defines the minimum setup required before deploying applications or automation scripts.

---

## 🎯 Objective

Establish a clean, secure and stable base system before applying advanced configurations.

---

# 1️⃣ System Update

Update package index and upgrade installed packages.

```bash
sudo apt update && sudo apt upgrade -y
````

Optional: remove unused packages

```bash
sudo apt autoremove -y
```

Verify:

```bash
uname -a
lsb_release -a
```

---

# 2️⃣ Create Administrative User

Avoid working directly as root.

```bash
adduser adrian
usermod -aG sudo adrian
```

Verify:

```bash
groups adrian
```

Switch user:

```bash
su - adrian
```

---

# 3️⃣ Configure SSH Access

Edit SSH configuration:

```bash
sudo nano /etc/ssh/sshd_config
```

Recommended settings:

```
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Restart SSH:

```bash
sudo systemctl restart ssh
```

Verify:

```bash
sudo systemctl status ssh
```

---

# 4️⃣ Configure Firewall (UFW)

Install if needed:

```bash
sudo apt install ufw -y
```

Basic configuration:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw enable
```

Check status:

```bash
sudo ufw status verbose
```

---

# 5️⃣ Timezone & Time Sync

Set correct timezone:

```bash
sudo timedatectl set-timezone Europe/Madrid
```

Verify:

```bash
timedatectl
```

Ensure time sync is active.

---

# 6️⃣ Install Essential Tools

Basic operational tools:

```bash
sudo apt install -y \
  curl \
  wget \
  vim \
  git \
  htop \
  net-tools \
  ncdu \
  unzip
```

Optional troubleshooting tools:

```bash
sudo apt install -y traceroute dnsutils
```

---

# 7️⃣ System Information Snapshot

Capture baseline system info.

```bash
hostnamectl
ip a
lsblk
free -h
df -h
```

Document:

* Hostname
* IP address
* Disk layout
* Memory size
* CPU cores

---

# 8️⃣ Verify Running Services

Check active services:

```bash
systemctl list-units --type=service --state=running
```

Check failed services:

```bash
systemctl --failed
```

---

# 9️⃣ Reboot Test

Validate system boots correctly after configuration.

```bash
sudo reboot
```

After reconnecting:

```bash
uptime
systemctl --failed
```

---

# 📌 Baseline Complete When:

* System fully updated
* Non-root admin user created
* SSH hardened
* Firewall active
* Essential tools installed
* No failed services
* Reboot successful

---

Status: 🚧 In progress — evolving with Linux & Bash Automation phase.
