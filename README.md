# 🐧 Linux & Bash Automation

Step 02 of my DevOps Roadmap.

This repository contains practical Linux administration documentation and Bash automation scripts built while strengthening core system fundamentals.
The objective is to move from manual server management to structured, repeatable and automated operations.

---

## 🎯 Focus

- Linux server baseline configuration
- System hardening basics
- Operational troubleshooting
- Bash scripting for automation
- Scheduled tasks with systemd timers
- Basic auditing and backup workflows

This phase builds the foundation required before moving into Docker, Kubernetes and Cloud Infrastructure.

---

## 📂 Repository Structure

### 📁 docs/

Documentation-first approach to system administration.

- `SERVER_BASELINE.md`
- `SERVER_SECURITY_CHECKLIST.md`
- `TROUBLESHOOTING.md`
- `COMMANDS_CHEATSHEET.md`

---

### 📁 scripts/

Operational automation scripts written in Bash.

- `healthcheck.sh`
- `backup.sh`
- `restore.sh`
- `deploy_pull_restart.sh`
- `user_audit.sh`
- `port_audit.sh`
- `log_snapshot.sh`

---

### 📁 systemd/

Service and timer definitions for scheduled automation.

- `backup.service`
- `backup.timer`
- `healthcheck.service` *(optional)*

---

### 📁 logs/

Sample log outputs for reference.  
(No secrets or sensitive data included.)

---

## 🧠 Philosophy

- Documentation before automation
- Automation before orchestration
- Strong Linux fundamentals before Cloud complexity

---

## 📈 Status

🚧 In progress — Building solid Linux and Bash foundations before advancing to containerization and cloud-native tooling.
