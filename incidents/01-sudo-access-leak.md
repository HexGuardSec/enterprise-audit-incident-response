# 🔐 Scenario 01 – Unused Sudo Access Detected

## 📝 Context

During a routine internal audit, it was discovered that the user `employee1` still had elevated privileges through the `sudo` group, despite not requiring administrative access for the past six months. Logs also confirmed that `sudo` was never used by this user recently.

To follow the **principle of least privilege**, the access must be removed.

---

## 🎯 Objective

- Check if `employee1` has sudo privileges
- Confirm group membership
- Remove unnecessary sudo access
- Validate the removal

---

## 🧪 Steps

### 1️⃣ Check sudo privileges

```bash
sudo -l
```

Result: The user has full access via group membership (`(ALL : ALL) ALL`).

---

### 2️⃣ Confirm group membership

```bash
getent group sudo
```

Result: `employee1` is listed in the `sudo` group.

---

### 3️⃣ Remove sudo access

```bash
sudo deluser employee1 sudo
```

This command removes the user from the `sudo` group without deleting the account.

---

### 4️⃣ Verify changes

```bash
getent group sudo
```

Result: `employee1` no longer appears in the group.

---

## 🖼️ Screenshots

* `01-sudo-list.png` — Show sudo privileges
* `02-check-sudo-su.png` — Verify connect root
* `03-remove-user-sudo.png` — Remove user from group
* `04-confirm-user-removed.png` — Confirm user was removed

---

## ✅ Outcome

Access to administrative commands has been revoked for `employee1`, reducing privilege exposure and maintaining system security. Regular permission reviews like this help prevent insider risks and misconfigurations.