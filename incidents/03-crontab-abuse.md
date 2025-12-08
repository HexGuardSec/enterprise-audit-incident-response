# 🛠️ Scenario 03 – Crontab Abuse by Non-Privileged User

## 🎯 Objective

Simulate a real-world scenario where a low-privileged user (`employee1`) abuses their own crontab to run a hidden script periodically. As an admin, investigate the activity, analyze the malicious cron job, and remove it properly.

---

## 🧩 Context

An alert was triggered by abnormal file creation under `/home/employee1/`.  
Upon investigation, it appears that the user created a cron job running every minute, executing a local script silently and logging its output.

This is a classic abuse where a regular user misuses cron to run persistent, unauthorized background scripts.

---

## 🧪 Phase 1 – User Abuse Simulation

As `employee1`:

### 🔹 Create a hidden cron script

```bash
mkdir -p /home/employee1/cron-abuse
cd /home/employee1/cron-abuse

echo -e '#!/bin/bash\necho "[+] User: $(whoami), Date: $(date)" >> log.txt' > script.sh
chmod +x script.sh
````

### 🔹 Register the script in crontab

```bash
crontab -e
# Add this line at the bottom:
* * * * * /home/employee1/cron-abuse/script.sh
```

> The script writes a new line every minute into `log.txt`.

---

## 🧪 Phase 2 – Investigation by Admin (`itadmin`)

### 🔍 Check for user crontab jobs

```bash
ls /var/spool/cron/crontabs/
cat /var/spool/cron/crontabs/employee1
```

> Shows the unauthorized cron task.

### 🔍 Inspect the malicious script

```bash
cat /home/employee1/cron-abuse/script.sh
cat /home/employee1/cron-abuse/log.txt
```

> You can observe periodic entries confirming execution.

---

## 🔧 Phase 3 – Resolution

### ✅ Remove the cron job

```bash
crontab -r -u employee1
```

> Removes all scheduled tasks for that user.

### ✅ Remove the script directory

```bash
rm -r /home/employee1/cron-abuse
```

---

## 📸 Screenshots (for GitHub documentation)

| Screenshot Name          | Description                             |
| ------------------------ | --------------------------------------- |
| 01-create-script         | Crontab abuse script creation           |
| 02-add-cronjob           | Crontab entry with malicious task       |
| 03-check-cron-user       | `ls /var/spool/cron/crontabs/` output   |
| 04-crontab-user-file     | Contents of `employee1` crontab         |
| 05-check-script-contents | `cat script.sh`                         |
| 06-check-log-output      | Execution logs from `log.txt`           |
| 07-delete-crontab        | Verification that crontab was deleted   |
| 08-delete-script-folder  | Verification that script folder is gone |

---

## ✅ Lessons Learned

* Users can abuse their own crontabs without elevated privileges.
* Periodic jobs may hide malicious or resource-draining scripts.
* Monitoring `/var/spool/cron/crontabs` helps detect unauthorized persistence.