# **Enterprise Audit Incident Response Lab**

![Status](https://img.shields.io/badge/Status-In%20Progress-orange)
![Category](https://img.shields.io/badge/Category-Incident%20Response-blue)
![Level](https://img.shields.io/badge/Level-SOC%20Analyst%20\(Junior\)-yellow)
![OS](https://img.shields.io/badge/OS-Ubuntu%2024.04-orange)
![Skills](https://img.shields.io/badge/Skills-Log%20Analysis%20%7C%20SSH%20%7C%20Permissions%20%7C%20Threat%20Hunting-lightgrey)
![MadeFor](https://img.shields.io/badge/Made%20for-Internal%20IT%20%2F%20SOC%20Training-blueviolet)

---

## 🧩 **Overview**

This lab simulates **real enterprise incidents** faced by Internal IT Engineers and SOC Level 1 Analysts.
Each scenario focuses on:

* Misconfigurations
* Unauthorized services
* Privilege issues
* Suspicious network activity
* Poor system hygiene
* Log-based investigations

The goal is to demonstrate **true hands-on technical ability**, not theory — exactly what companies expect during junior IT / SOC technical interviews.

---

# 📁 **Completed Incident Scenarios**

Below are all scenarios completed so far, rewritten in a **clean SOC-style format**.

---

## 🔹 **1. Sudo Access Leak (Privilege Misuse)**

A user retained **sudo privileges** after a temporary role upgrade, even though the elevated rights were unused for months.

### ✔ Investigation

* Enumerated sudo-capable users
* Analyzed `/var/log/auth.log` for sudo usage
* Confirmed complete inactivity

### ✔ Remediation

* Removed unnecessary sudo rights
* Cleaned rules in `/etc/sudoers.d`
* Validated system behavior post-removal

---

## 🔹 **2. Unknown SSH Service (Suspicious Service Running)**

An unexpected SSH-like service was running with its own binary and configuration.

### ✔ Investigation

* Reviewed running processes and services (`ps`, `systemctl`, `ss -tulpn`)
* Located the unknown binary
* Searched for persistence or malicious scripts

### ✔ Remediation

* Disabled the unauthorized service
* Removed its binary and config
* Ensured no persistence mechanism remained

---

## 🔹 **3. Crontab Abuse (Unauthorized Scheduled Task)**

A cron task existed without documentation or ownership, suggesting abandoned automation or potential malicious activity.

### ✔ Investigation

* Checked `/etc/crontab`, `/etc/cron.*`, and user crontabs
* Inspected script content and timestamps
* Verified user activity history

### ✔ Remediation

* Removed the suspicious cron entry
* Deleted or archived leftover scripts
* Performed full validation of scheduled task integrity

---

## 🔹 **4. Permissions Misconfiguration**

A directory had **dangerously permissive rights**, allowing unauthorized access or modification.

### ✔ Investigation

* Identified dangerous permission patterns (`777`, world-writable, etc.)
* Mapped which users had access and why
* Evaluated potential exploitation vectors

### ✔ Remediation

* Reapplied correct permissions and ownership
* Ensured least-privilege model
* Tested impacted applications after the fix

---

## 🔹 **5. Unknown Port Listener (Suspicious Network Activity)**

A process was listening on a non-standard port without documentation.

### ✔ Investigation

* Mapped port-to-process using `ss`, `netstat`, `lsof`
* Inspected binary origin and parent processes
* Reviewed logs for unusual remote connections

### ✔ Remediation

* Disabled and removed the rogue service
* Audited system for related artefacts
* Added monitoring rules to detect recurrence

---

## 🔹 **6. Orphaned Home Directory (Residual Artefact After User Deletion)**

A directory `/home/olduser` remained on the system even though the account **no longer existed**, representing poor system hygiene and potential data exposure.

### ✔ Investigation

* Listed home directories → detected `/home/olduser`
* Checked `/etc/passwd` → no `olduser` entry found
* Examined file contents and modification dates

### ✔ Findings

* The user account was deleted
* The home directory **was not removed during deletion**
* Sensitive data or scripts could have remained

### ✔ Remediation

* Removed the orphaned directory:

  ```bash
  sudo rm -rf /home/olduser
  ```
* Verified cleanup in `/home`
* Confirmed no references in sudoers, cron, groups, services

### ✔ SOC Summary

Residual artefacts after account deletion can expose internal data.
Cleanup completed and system hygiene restored.

---

# 🛠️ **Tools & Techniques Used**

* `journalctl` — log analysis
* `ps aux`, `systemctl` — process & service audit
* `ss`, `lsof` — network port investigation
* `find`, `grep`, `awk` — targeted search & filtering
* `visudo`, `/etc/sudoers.d` — privilege auditing
* `crontab`, `/etc/cron.*` — scheduled task inspection
* Linux hardening & security validation

---

# 📸 **Screenshots**

Evidence for each incident is stored in:

```
/screenshots/
```

Screenshots include commands, findings, logs, and validation steps.

---

# 📘 **Documentation Structure**

Each incident follows this structure:

* Description
* Investigation steps
* Findings
* Remediation
* Validation
* SOC-style summary

---

# 🚀 **Planned Additions**

Future simulated incidents include:

* SSH brute-force detection
* Unauthorized SSH key installation
* SUID escalation attempts
* Suspicious log pattern analysis
* Firewall misconfigurations
* Network anomaly detection