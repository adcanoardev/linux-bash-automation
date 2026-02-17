# Troubleshooting & System Experiments

Practical troubleshooting notes and experiments while learning Linux administration.

Objective:
Understand system behavior before automating solutions.

---

# 1️⃣ Process Resource Limiting (prlimit)

## Objective
Limit memory of a running process and observe system behavior.

## Steps
Get PID:

```bash
pidof emacs
````

Limit memory (example):

```bash
prlimit --pid <PID> --rss=1000000
```

Check limits:

```bash
prlimit --pid <PID>
```

## What to observe
* Process responsiveness
* Memory usage:

```bash
top
free -h
```

* Stability (does it freeze or crash?)

---

# 2️⃣ High CPU Usage Investigation

## Objective
Identify processes consuming excessive CPU.

## Steps
Check load:

```bash
uptime
```

Monitor live:

```bash
top
```

Top CPU consumers:

```bash
ps -eo pid,user,%cpu,cmd --sort=-%cpu | head
```

## What to observe
* CPU consistently above 80–90%
* Unexpected background processes
* Repeated spikes

---

# 3️⃣ Disk Space Investigation

## Objective
Find what is filling disk space.

## Steps
Check usage:

```bash
df -h
```

Find large directories:

```bash
du -sh /*
```

Interactive view:

```bash
sudo ncdu /
```

## What to observe
* Root (/) near 100%
* /var growing due to logs
* Unexpected large folders

---

# 4️⃣ Service Not Starting

## Objective
Diagnose why a service fails.

## Steps
Check status:

```bash
systemctl status <service>
```

Check logs:

```bash
journalctl -u <service> -n 100 --no-pager
```

Check port:

```bash
sudo ss -tulpn | grep <port>
```

## What to observe
* Permission errors
* Port already in use
* Configuration errors

---

# 5️⃣ Network Connectivity Issue

## Objective
Diagnose connectivity problems.

## Steps
Check IP:

```bash
ip a
```

Check routing:

```bash
ip r
```

Ping external IP:

```bash
ping 8.8.8.8
```

Test DNS:

```bash
dig google.com
```

Trace route:

```bash
traceroute google.com
```

## What to observe
* Missing default route
* DNS not resolving
* Packet loss or latency

---

# 6️⃣ Inode Inspection & File Metadata

## Objective
Understand what an inode is and how Linux stores file metadata.

Important:
An inode does NOT store the filename.
It stores file metadata.

---

## Get inode number
```bash
ls -i test.txt
```

Example output:

```
524373 test.txt
```

Meaning:

* 524373 → inode number
* test.txt → filename pointing to that inode

Multiple filenames (hard links) can share the same inode.

---

## Check inode usage in filesystem
```bash
df -i
```

Example:

```
Filesystem      Inodes  IUsed   IFree IUse% Mounted on
/dev/sda3      1277952 206287 1071665   17% /
```

Important:

A filesystem can run out of inodes even if disk space is available.

This happens when:

* Too many small files exist
* Logs accumulate

---

## Inspect inode metadata (low-level)
```bash
sudo debugfs -R "stat <524373>" /dev/sda3
```

Shows:

* Owner (User / Group)
* File size
* Link count
* Timestamps
* Block usage

Example fields:

* Size → file size in bytes
* Links → number of hard links
* ctime → metadata change time
* mtime → content modification time
* atime → last access time
* crtime → creation time (if supported)

---

## Key concept
File structure in Linux:

Directory entry (filename) → inode → data blocks

* Filename stored in directory
* Inode stores metadata
* Data blocks store content

---

## Why this matters
Helps with:

* Understanding hard links
* Diagnosing inode exhaustion
* Troubleshooting deleted files still using space
* Low-level filesystem debugging
