# Server Security Checklist

Baseline security checklist for a freshly provisioned Linux server.  
Focused on minimal production-ready hardening.

---

## 🔐 SSH Hardening

- [ ] Root login disabled (`PermitRootLogin no`)
- [ ] Password authentication disabled (`PasswordAuthentication no`)
- [ ] SSH key authentication configured
- [ ] Strong SSH key (ed25519 or rsa 4096)
- [ ] Non-standard SSH port (optional)
- [ ] SSH service restarted after changes

Verify:
```bash
sudo systemctl status ssh
sudo ss -tulpn | grep ssh
````

---

## 🧱 Firewall (UFW or similar)

* [ ] Firewall installed
* [ ] Default policy set to deny incoming
* [ ] Only required ports allowed (e.g. 22, 80, 443)
* [ ] Firewall enabled

Example:

```bash
sudo ufw default deny incoming
sudo ufw allow OpenSSH
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
sudo ufw status
```

---

## 🚫 Fail2ban (Brute-force protection)

* [ ] Fail2ban installed
* [ ] SSH jail enabled
* [ ] Ban time configured
* [ ] Max retry attempts configured

Verify:

```bash
sudo systemctl status fail2ban
sudo fail2ban-client status
```

---

## 👤 User & Privilege Management

* [ ] Non-root administrative user created
* [ ] User added to sudo group
* [ ] Root account locked (optional)
* [ ] Unused users removed
* [ ] Strong password policy applied

Verify:

```bash
cat /etc/passwd
sudo getent group sudo
```

---

## 📂 File Permissions

* [ ] Sensitive files restricted (600/640 where appropriate)
* [ ] Correct ownership for services
* [ ] No world-writable critical files

Check:

```bash
ls -lah /etc/ssh/
find / -type f -perm -o+w 2>/dev/null
```

---

## 📜 Logging & Monitoring

* [ ] Log rotation configured (logrotate)
* [ ] SSH login attempts monitored
* [ ] System logs reviewed periodically

Check logs:

```bash
sudo tail -n 100 /var/log/auth.log
sudo journalctl -n 100 --no-pager
```

---

## 🧠 System Updates

* [ ] System fully updated
* [ ] Security updates enabled
* [ ] Automatic updates configured (optional)

Update:

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🌐 Network Exposure

* [ ] Only required ports open
* [ ] No unnecessary services running
* [ ] Listening ports reviewed

Check:

```bash
sudo ss -tulpn
```

---

## 📊 Resource Limits (Optional Hardening)

* [ ] Proper ulimit values configured
* [ ] Critical services monitored for resource usage
* [ ] Swap usage reviewed

Check:

```bash
ulimit -a
free -h
```

---

## ✅ Final Review

* [ ] Server accessible via SSH key only
* [ ] Firewall active
* [ ] No default credentials
* [ ] Logs clean (no repeated errors)
* [ ] System reboot tested

Reboot test:

```bash
sudo reboot
```

After reboot:

```bash
systemctl --failed
```

---

Status: 🚧 In progress — evolving alongside Linux & Bash automation phase.

