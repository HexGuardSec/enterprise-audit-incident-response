# 🛠️ Scenario 02 – Suspicious SSH Port and Rogue Service

---

## 📄 Context

During a routine audit, an unusual SSH service was discovered listening on port `49122`. This was **not defined** in the official `/etc/ssh/sshd_config`, suggesting a rogue or misconfigured process.

---

## 🔍 Phase 1 – Port & Service Detection

We used the `ss` tool to list all active listening ports:

```bash
sudo ss -tulnp
````

📸 `01-port-listen-unknow.png`

We spotted this line:

```
tcp LISTEN 0 128 0.0.0.0:49122 0.0.0.0:* users:(("sshd",pid=1483,fd=3))
```

An unexpected process is listening on port `49122`, associated with `sshd`.

---

## 🔎 Phase 2 – Investigate the Process

We traced the process using `ps`:

```bash
ps -p 1483 -o pid,cmd
```

📸 `02-ps-port-owner.png`

This revealed:

```
PID CMD
1483 /usr/sbin/sshd
```

However, when checking the SSH config file:

```bash
cat /etc/ssh/sshd_config | grep Port
```

📸 `05-sshd-config-check.png`

Only port `22` was listed. There was **no trace** of port `49122` in the official config.

---

## 📁 Phase 3 – Discover the Hidden Service

We suspected a custom systemd service and searched `/etc/systemd/system`:

```bash
ls -l /etc/systemd/system/
cat /etc/systemd/system/sshd-backup.service
```

📸 `03-systemctl-service.png`

The file contained:

```ini
[Unit]
Description=Rogue SSH Service

[Service]
ExecStart=/usr/sbin/sshd -D -p 49122

[Install]
WantedBy=multi-user.target
```

We confirmed it was running:

```bash
sudo systemctl status sshd-backup
```

📸 `04-systemctl-status-service.png`

---

## ✅ Phase 4 – Resolution

### 1. Stop and Disable the Fake Service

```bash
sudo systemctl stop sshd-backup
sudo systemctl disable sshd-backup
```

📸 `06-stop-disabled-service.png`

### 2. Remove the Unit File + Reload systemd

```bash
sudo rm /etc/systemd/system/sshd-backup.service
sudo systemctl daemon-reload
```

📸 `07-rm-service-daemon-reload.png`

### 3. Confirm Cleanup

```bash
sudo ss -tulnp | grep 49122
sudo systemctl status sshd-backup
```

📸 `08-verification-no-port.png`
📸 `09-status-failed.png`

---

## 🎯 Takeaways

* A rogue systemd unit can silently launch hidden SSH services.
* Always correlate open ports with systemd, crontab, and `ps` output.
* Limit write access to `/etc/systemd/system/` to trusted admins only.
* Use `ss`, `ps`, `systemctl`, and `journalctl` together during audits.