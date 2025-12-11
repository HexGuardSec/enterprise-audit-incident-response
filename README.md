# **Enterprise Audit Incident Response Lab**

![Status](https://img.shields.io/badge/Status-In%20Progress-orange)
![Category](https://img.shields.io/badge/Category-Incident%20Response-blue)
![Level](https://img.shields.io/badge/Level-SOC%20Analyst%20\(Junior\)-yellow)
![OS](https://img.shields.io/badge/OS-Ubuntu%2024.04-orange)
![Skills](https://img.shields.io/badge/Skills-Log%20Analysis%20%7C%20SSH%20%7C%20Permissions%20%7C%20Threat%20Hunting-lightgrey)
![MadeFor](https://img.shields.io/badge/Made%20for-Internal%20IT%20%2F%20SOC%20Training-blueviolet)

---

## 🧩 **Overview**

This lab simulates **real enterprise security incidents** faced by Internal IT Engineers and SOC Level 1 Analysts.
Each scenario focuses on **misconfigurations, suspicious activity, privilege issues, unauthorized services**, and improper system hygiene.

The objective of this lab is to demonstrate:

* Real investigation workflows
* Linux system auditing
* Root cause analysis
* Remediation of security issues
* Professional reporting

---

# 📁 **Completed Incident Scenarios**

Here are the incidents you **already completed**, rewritten in a clean SOC style.

---

## 🔹 **1. Sudo Access Leak (Privilege Misuse)**

A user retained **sudo privileges** after a temporary role change, even though the elevated rights were **unused for 6+ months**.

### ✔ Investigation

* Identified all sudo-capable users
* Cross-checked `/var/log/auth.log` for sudo activity
* Confirmed user inactivity

### ✔ Remediation

* Removed unnecessary sudo rights
* Documented the privilege cleanup
* Validated no privilege escalation vectors remained

---

## 🔹 **2. Unknown SSH Service (Suspicious Service on System)**

A secondary SSH service binary was found running with unknown origin.

### ✔ Investigation

* Enumerated running services using `ps`, `systemctl`, and `ss -tulpn`
* Located unexpected SSH binary
* Checked persistence mechanisms

### ✔ Remediation

* Disabled and removed unauthorized service
* Cleaned leftover files
* Validated system integrity

---

## 🔹 **3. Crontab Abuse (Unauthorized Scheduled Task)**

A cron task existed without documentation, ownership justification, or approval.

### ✔ Investigation

* Inspected `/etc/crontab`, `/etc/cron.*`, and user crontabs
* Traced file modification history
* Identified orphaned or suspicious script

### ✔ Remediation

* Removed unauthorized cron entry
* Checked for associated malicious scripts
* Ensured no persistence mechanisms remained

---

## 🔹 **4. Permissions Misconfiguration**

A directory or file had excessively permissive rights (e.g. `777`, `775`, or world-writable logs), posing a confidentiality or integrity risk.

### ✔ Investigation

* Identified abnormal permissions
* Checked access history and responsible users
* Mapped potential exploitation paths

### ✔ Remediation

* Set appropriate ownership and permissions
* Restricted access following least-privilege model
* Confirmed correct behavior after changes

---

## 🔹 **5. Unknown Port Listener (Suspicious Network Activity)**

A process was listening on a non-standard port with no ticket or documented service.

### ✔ Investigation

* Used `ss`, `netstat`, and `lsof` to map processes
* Verified binary paths and parent processes
* Checked logs for suspicious connections

### ✔ Remediation

* Disabled or removed rogue listener
* Applied hardening to prevent recreation
* Documented findings and actions

---

# 🛠️ **Tools & Techniques Used**

* `journalctl` – log analysis
* `ss`, `netstat`, `lsof` – network & process enumeration
* `ps aux`, `systemctl` – service investigation
* `find`, `grep`, `awk` – file search & filtering
* `visudo`, `/etc/sudoers.d` – privilege audits
* `crontab`, `/etc/cron.*` – scheduled tasks review
* System hardening and remediation

---

# 📸 **Screenshots Folder**

All evidence (logs, commands, outputs, investigation notes) is stored inside:

```
/screenshots/
```

Screenshots are named per incident for clarity.

---

# 📘 **Documentation Method**

Each incident includes:

* Description
* Investigation steps
* Findings
* Remediation
* Validation
* Final SOC-style summary

---

# 🚀 **Planned Additions**

Future scenarios to include:

* SSH brute-force detection
* Unauthorized key installation
* SUID exploitation detection
* Suspicious log pattern analysis
* Network anomaly detection
* Rudimentary SIEM-style triage

---

# 🎯 **Purpose of This Lab**

This project demonstrates your growing ability as a:

* Internal IT Engineer
* SOC Level 1 Analyst
* Linux Security Technician

It showcases **real technical skills**, not theory — exactly what Japanese companies want to see from a junior engineer.
