# Troubleshooting & System Experiments

This document contains practical troubleshooting scenarios and controlled experiments performed while learning Linux system administration.

The objective is to understand how the system behaves under different conditions before automating solutions.

---

# 🔍 1️⃣ Process Resource Limiting (prlimit)

## Objective

Understand how limiting process resources affects system behavior.

## Scenario

Run a process (e.g. emacs) and restrict its memory usage using `prlimit`.

## Steps

Find process PID:

```bash
pidof emacs
````

Apply memory limit (example):

```bash
prlimit --pid <PID> --rss=1000000
```

Check current limits:

```bash
prlimit --pid <PID>
```

## What to Observe

* Process responsiveness
* Memory consumption via:

  ```bash
  top
  free -h
  ```
* Whether process crashes or becomes unstable

## Notes

Useful for:

* Simulating constrained environments
* Understanding Linux process control
* Learning system resource management

---

# 🔍 2️⃣ High CPU Usage Investigation

## Objective

Identify which process is consuming excessive CPU.

## Steps

Check system load:

```bash
uptime
```

Monitor processes:

```bash
top
```

List top CPU consumers:

```bash
ps -eo pid,user,%cpu,cmd --sort=-%cpu | head
```

## What to Observe

* CPU usage above 80–90%
* Repeated spikes
* Background processes running unexpectedly

---

# 🔍 3️⃣ Disk Space Investigation

## Objective

Identify what is filling up disk space.

## Steps

Check filesystem usage:

```bash
df -h
```

Identify large directories:

```bash
du -sh /*
```

Interactive view:

```bash
sudo ncdu /
```

## What to Observe

* Root (/) partition near full
* /var growing due to logs
* Unexpected large directories

---

# 🔍 4️⃣ Service Not Starting

## Objective

Diagnose why a service fails to start.

## Steps

Check service status:

```bash
systemctl status <service>
```

Check logs:

```bash
journalctl -u <service> -n 100 --no-pager
```

Check port usage:

```bash
sudo ss -tulpn | grep <port>
```

## What to Observe

* Permission denied errors
* Port already in use
* Configuration file issues

---

# 🔍 5️⃣ Network Connectivity Issue

## Objective

Diagnose why server cannot reach external services.

## Steps

Check IP configuration:

```bash
ip a
```

Check routing:

```bash
ip r
```

Test connectivity:

```bash
ping 8.8.8.8
```

Test DNS resolution:

```bash
dig google.com
```

Trace route:

```bash
traceroute google.com
```

## What to Observe

* Missing default route
* DNS not resolving
* High latency or dropped hops

---

# 📌 Philosophy

Troubleshooting is not about memorizing commands.
It is about:

1. Observe the symptom
2. Identify the layer (process, disk, service, network)
3. Validate with commands
4. Document findings
5. Automate prevention later

---

Status: 🚧 Continuously updated during Linux & Bash phase.
