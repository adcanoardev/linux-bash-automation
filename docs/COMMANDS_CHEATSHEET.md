# Linux Commands Cheatsheet

Minimal reference for daily Linux operations and troubleshooting.

---

## 🔧 PROCESSES

### ps – list running processes
```bash
ps aux
ps -eo pid,user,%cpu,%mem,cmd --sort=-%cpu | head
```

### top / htop – live process monitoring
```bash
top
htop
```

### pidof – get PID by process name
```bash
pidof nginx
```

### kill – send signal to process
```bash
kill -TERM <PID>
kill -KILL <PID>
```

### nice / renice – adjust process priority
```bash
nice -n 10 command
renice -n 10 -p <PID>
```

---

## 🧩 SHELL – Wildcards (pattern matching)

```bash
ls *.txt
ls file?.txt
ls file[123].txt
ls file[a-z].txt
ls file[[:digit:]].txt
echo *.txt
```
- `*` → any number of characters  
- `?` → exactly one character  
- `[abc]` → specific characters  
- `[a-z]` → range  
- `[[:digit:]]` → any numeric digit (0–9, POSIX class)  

---

## 💾 DISK

### df – filesystem usage
```bash
df -h
```

### du – directory size
```bash
du -sh /*
du -sh /var/log/*
```

### lsblk – block devices layout
```bash
lsblk
```

### ncdu – interactive disk usage analyzer
```bash
sudo ncdu /
```

---

## 🧠 MEMORY / CPU

### free – memory usage
```bash
free -h
```

### vmstat – system performance statistics
```bash
vmstat 1 5
```

### uptime – load average & uptime
```bash
uptime
```

### lscpu – CPU information
```bash
lscpu
```

---

## ⚙️ SERVICES

### systemctl – manage systemd services
```bash
systemctl status nginx
systemctl restart nginx
systemctl enable nginx
```

### journalctl – view systemd logs
```bash
journalctl -u nginx -n 100
journalctl -f
```

---

## 🌐 NETWORK

### ip – network interfaces and routes
```bash
ip a
ip r
```

### ss – show listening sockets
```bash
ss -tulpn
```

### curl – HTTP request / API test
```bash
curl -I https://example.com
```

### dig – DNS query tool
```bash
dig example.com
```

### traceroute – trace network path
```bash
traceroute example.com
```

---

## 📜 LOGS

### tail – view log in real time
```bash
tail -f /var/log/syslog
```

### grep – search text in files
```bash
grep "error" /var/log/syslog
```

### awk – text processing & field extraction
```bash
awk '{print $9}' access.log | sort | uniq -c | sort -nr
```

---

## 🔀 Redirections

### stdout (1) – normal output
```bash
command > file.txt
```

### stderr (2) – error output
```bash
command 2> errors.txt
```

### Discard errors
```bash
command 2> /dev/null
```

### Redirect both stdout and stderr
```bash
command > output.txt 2>&1
```

### Send everything to null
```bash
command > /dev/null 2>&1
```
---

## 🔄 Shell Expansions

Shell expands expressions before executing the command.

### Brace Expansion – generate combinations
```bash
echo file{1..3}.txt
echo {a,b,c}.log
```

### Tilde Expansion – home directory
```bash
cd ~
cd ~/Documents
```

### Parameter Expansion – variables
```bash
name="adrian"
echo $name
echo ${name}
```

### Command Substitution – use command output
```bash
echo $(date)
files=$(ls)
```

### Arithmetic Expansion – basic math
```bash
echo $((2 + 3))
```

### Pathname Expansion – globbing
```bash
ls *.txt
ls file[0-9].txt
```

### Order (simplified)
1. Brace  
2. Tilde  
3. Parameter  
4. Command substitution  
5. Arithmetic  
6. Globbing
