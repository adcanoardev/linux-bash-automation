# Troubleshooting & System Experiments

This document contains practical troubleshooting scenarios and controlled experiments performed while learning Linux system administration.

The objective is to understand how the system behaves under different conditions before automating solutions.

---

# 🔍 Troubleshooting – Address Space Limiting, Swap Generation & Forensic Analysis

## Objective

Induce a controlled application failure by enforcing a **very low virtual address space limit (`RLIMIT_AS = 1000`)** on a running `emacs` process.
This constraint causes memory allocation failures, application instability, and **swap usage**, enabling the extraction and forensic analysis of the swap file using **Autopsy**.

---

## Environment

* **Target system:** Ubuntu Server
* **Application under test:** `emacs`
* **Resource control tool:** `prlimit`
* **Limit applied:** Address Space (`--as=1000`)
* **Swap type:** swapfile
* **Forensic analysis system:** Windows (Autopsy)

---

## Scenario Description

A restrictive **virtual memory address space limit** is applied to an active `emacs` process using `prlimit`.
Once the process exceeds the allowed address space, memory allocations fail, leading to:

* Severe application slowdown
* Allocation errors
* Increased swap activity
* Application crash or freeze

The resulting **swapfile** is preserved and extracted for **forensic analysis**, simulating a real-world troubleshooting and incident response scenario.

---

## Procedure

### 1️⃣ Launch Emacs

Start the application:

```bash
emacs &
```

Identify the process ID:

```bash
pidof emacs
```

---

### 2️⃣ Apply Address Space Limit (`RLIMIT_AS`)

Apply an extremely low virtual memory limit to force failure:

```bash
prlimit --pid 2527 --as=1000
```

Verify the applied limits:

```bash
prlimit --pid 2527
```

Expected output:

* `AS (address space)` set to `1000`

---

### 3️⃣ Trigger Application Failure

Continue interacting with `emacs` until memory allocation failures occur.

Monitor system behavior:

```bash
top
free -h
htop
```

Expected observations:

* Emacs becomes unresponsive or crashes
* Memory allocation errors
* Swap usage increases despite low RAM pressure
* System remains stable, process does not

---

### 4️⃣ Verify Swap Usage

Confirm active swap usage:

```bash
swapon --show
```

Typical swapfile location:

```text
/swapfile
```

---

### 5️⃣ Disable Swap Safely (Forensic Preservation)

⚠️ Swap must be disabled before extraction to prevent data corruption.

```bash
sudo swapoff /swapfile
```

---

### 6️⃣ Extract Swapfile

Copy the swapfile to the Desktop:

```bash
sudo cp /swapfile ~/Desktop/emacs_swapfile.raw
```

Fix ownership:

```bash
sudo chown $USER:$USER ~/Desktop/emacs_swapfile.raw
```

---

### 7️⃣ Transfer Swapfile to Windows Host

Example using SCP:

```bash
scp ~/Desktop/emacs_swapfile.raw usuario_windows@IP_WINDOWS:/ruta/destino
```

---

### 8️⃣ Forensic Analysis Using Autopsy

1. Open **Autopsy** on Windows
2. Create a new case
3. Add data source → **Disk Image / Raw Image**
4. Select `emacs_swapfile.raw`
5. Analyze:

   * Extracted strings
   * Memory remnants
   * Application artifacts
   * Crash-related data

---

## Technical Notes

* `--as` limits **virtual address space**, not RAM directly
* Extremely low `RLIMIT_AS` values cause immediate allocation failures
* Swap may still be populated with partially allocated or evicted pages
* Swapfiles are valuable sources of **volatile forensic artifacts**

---

## Key Takeaways

* `prlimit --as` is effective for simulating memory exhaustion scenarios
* Swap analysis provides insight into application state at failure time
* Controlled crashes are useful for **forensic training and troubleshooting**
* Proper swap handling is critical for data integrity

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
