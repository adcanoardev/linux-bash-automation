# Linux Commands Cheatsheet

Practical Linux commands for daily operations, troubleshooting and system analysis.  
Includes examples and what to check in real scenarios.

---

# 🔧 PROCESSES

## ps – list processes (snapshot)

```bash
ps aux
ps aux | grep nginx
ps -eo pid,ppid,user,%cpu,%mem,etime,cmd --sort=-%cpu | head
````

What to look at:

* High %CPU or %MEM
* Long running time (etime)
* Parent process (PPID)
* Full command (cmd)

---

## top / htop – live monitoring

```bash
top
htop
```

What to look at:

* Load average (top line)
* Processes consuming most CPU/RAM
* Overall responsiveness

---

## pidof – get PID by process name

```bash
pidof sshd
pidof nginx
```

What to look at:

* Multiple PIDs → service running workers

---

## kill – stop a process

```bash
kill -TERM <PID>
kill -KILL <PID>
```

What to look at:

* Use TERM first (graceful shutdown)
* Use KILL only if process is stuck

---

## nice / renice – process priority

```bash
nice -n 10 command
renice -n 10 -p <PID>
```

What to look at:

* Increase nice value to lower priority
* Prevent non-critical tasks from impacting system

---

# 💾 DISK

## df -h – filesystem usage

```bash
df -h
df -h /var
```

What to look at:

* Filesystems above 85–90%
* Root (/) or /var filling up

---

## du -sh – directory size

```bash
du -sh /var/log/*
du -sh ./*
```

What to look at:

* Large directories
* Rapidly growing log folders

---

## lsblk – disk layout

```bash
lsblk
lsblk -f
```

What to look at:

* Mounted disks
* Filesystem types
* Available devices

---

## ncdu – interactive disk usage

```bash
sudo ncdu /
```

What to look at:

* Quickly identify large folders

---

# 🧠 MEMORY / CPU

## free -h – memory usage

```bash
free -h
```

What to look at:

* Available memory
* Swap usage

---

## vmstat – system performance

```bash
vmstat 1 5
```

What to look at:

* si/so → swap activity
* wa → IO wait
* r → run queue

---

## uptime – load average

```bash
uptime
```

What to look at:

* Load compared to number of CPU cores

---

## lscpu – CPU information

```bash
lscpu
```

What to look at:

* Number of CPUs
* Architecture
* Virtualization support

---

# ⚙️ SERVICES (systemd)

## systemctl – manage services

```bash
systemctl status nginx
sudo systemctl restart nginx
sudo systemctl enable nginx
sudo systemctl disable nginx
```

What to look at:

* active (running) vs failed
* Recent log output
* Service enabled at boot

---

## journalctl -u – service logs

```bash
journalctl -u nginx -n 100 --no-pager
journalctl -u nginx -f
journalctl -u nginx --since "today"
```

What to look at:

* Repeated errors
* Permission issues
* Bind failures

---

# 🌐 NETWORK

## ip a – network interfaces

```bash
ip a
```

What to look at:

* Correct IP assigned
* Interface state (UP)

---

## ip r – routing table

```bash
ip r
```

What to look at:

* Default route present
* Correct gateway

---

## ss -tulpn – listening ports

```bash
sudo ss -tulpn
sudo ss -tulpn | grep :80
```

What to look at:

* Which service is listening
* Port conflicts

---

## curl – HTTP requests / health checks

```bash
curl -I https://example.com
curl -s http://localhost:8080/health
curl -v http://localhost:8080
```

What to look at:

* HTTP status codes (200, 301, 404, 500)
* Connection errors
* Response time

---

## dig – DNS lookup

```bash
dig example.com
dig +short example.com
```

What to look at:

* Correct IP resolution
* DNS failures

---

## traceroute – network path

```bash
traceroute example.com
```

What to look at:

* Where connection drops
* Network latency between hops

---

# 📜 LOGS

## /var/log/* – classic logs

```bash
ls -lah /var/log
sudo tail -n 200 /var/log/syslog
sudo tail -n 200 /var/log/auth.log
```

What to look at:

* Authentication attempts (auth.log)
* System errors (syslog)

---

## journalctl – systemd logs

```bash
journalctl -n 200 --no-pager
journalctl -f
journalctl --since "1 hour ago"
```

What to look at:

* Recent errors
* Kernel messages
* Service-related issues

---

## tail -f – follow logs live

```bash
tail -f /var/log/syslog
tail -f /var/log/nginx/error.log
```

What to look at:

* Real-time errors while reproducing issues

---

## grep – search inside logs

```bash
grep -R "error" /var/log/nginx/
grep -n "failed" /var/log/syslog
grep -E "timeout|refused|denied" /var/log/syslog
```

What to look at:

* Repeated error patterns
* Failure messages

---

## awk – basic log analysis

Example: extract IPs from failed SSH logins

```bash
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | head
```

What to look at:

* Repeated attacking IPs

Example: count HTTP status codes (nginx)

```bash
awk '{print $9}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head
```

What to look at:

* Many 404 → bad routes or bots
* 500 → backend issues
* 301/302 → redirects
