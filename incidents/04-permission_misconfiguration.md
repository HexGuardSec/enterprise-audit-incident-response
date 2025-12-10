# 🛡️ Incident 04 — Permission Misconfiguration

## 📘 Summary

A sensitive file under `/opt/finance` was discovered with overly permissive access rights (`777`). This misconfiguration allowed any user on the system to read or modify potentially confidential financial information.

---

## 🎯 Objectives

- Identify files or directories with improper permissions.
- Correct access rights using `chmod`, `chown`, and appropriate user/group settings.
- Validate that only authorized users can access the sensitive file.

---

## 🛠️ Technical Details

### 🔍 Initial Misconfiguration

The file `/opt/finance/secrets.txt` contained confidential salary data and was assigned full access rights to all users:

```bash
ls -l /opt/finance
````

Expected output:

```
-rwxrwxrwx 1 root root 30 Dec 10 10:12 secrets.txt
```

📸 Screenshot: `screenshots/04-permission_misconfiguration/permission_before.png`

---

### 🔧 Remediation Steps

1. **Assign proper ownership**:

```bash
sudo chown root:security /opt/finance/secrets.txt
```

2. **Restrict file permissions**:

```bash
sudo chmod 640 /opt/finance/secrets.txt
```

3. **Verify updated permissions**:

```bash
ls -l /opt/finance
```

Expected output:

```
-rw-r----- 1 root security 30 Dec 10 10:15 secrets.txt
```

📸 Screenshot: `screenshots/04-permission_misconfiguration/permission_after.png`

---

## 👤 Access Validation

A non-privileged user was created to verify the access restriction:

```bash
sudo adduser testuser
su - testuser
cat /opt/finance/secrets.txt
```

Expected result: `Permission denied`

📸 Screenshot: `screenshots/04-permission_misconfiguration/user_access_denied.png`

---

## ✅ Outcome

The file is now only accessible by the root user and members of the `security` group. This mitigates the risk of unauthorized data access and aligns with least privilege best practices.

---

## 📚 Lessons Learned

* Regular audits of permissions on sensitive files are essential.
* Avoid defaulting to `chmod 777`, especially in production environments.
* File access should always follow the principle of least privilege.