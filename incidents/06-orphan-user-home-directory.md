# 🧹 Orphaned Home Directory Cleanup  

<p>
  <img src="https://img.shields.io/badge/OS-Ubuntu%20Server-E95420?style=flat-square&logo=ubuntu" />
  <img src="https://img.shields.io/badge/Topic-Linux%20Security-blueviolet?style=flat-square" />
  <img src="https://img.shields.io/badge/Focus-System%20Cleanup-success?style=flat-square" />
  <img src="https://img.shields.io/badge/Scope-User%20Lifecycle%20%26%20Home%20Dirs-informational?style=flat-square" />
  <img src="https://img.shields.io/badge/Impact-Low%20Risk%20%2F%20High%20Hygiene-important?style=flat-square" />
</p>

---

## 🎯 Objective  

A previously removed user account (`olduser`) still had a leftover home directory on the system.  
This can lead to **inconsistent system state, wasted storage, and potential data exposure**.

Goal:  
✔️ Detect the orphan directory  
✔️ Confirm the user no longer exists  
✔️ Clean it safely  
✔️ Validate the system is consistent afterwards  

---

## 1️⃣ Detection of an Orphan Home Directory

A simple list of `/home` revealed the directory:

```bash
ls /home
````

![presence-home](/screenshots/06-orphan-user-home-directory/presence-home.png)

---

## 2️⃣ Confirming User Deletion

Before deleting any directory manually, always ensure the user truly doesn't exist:

```bash
cat /etc/passwd | grep -E "olduser"
```

![no-user](/screenshots/06-orphan-user-home-directory/no-user.png)

This confirms the directory is orphaned.

---

## 3️⃣ Removing the Leftover Home Directory

The directory was safely removed using:

```bash
sudo rm -rf /home/olduser
```

![del-home](/screenshots/06-orphan-user-home-directory/del-home.png)

---

## 4️⃣ Validation After Cleanup

Verify the directory no longer exists:

```bash
ls /home
```

![no-presence-home](/screenshots/06-orphan-user-home-directory/no-presence-home.png)

---

## ✔️ Final Status

| Check                             | Result     |
| --------------------------------- | ---------- |
| User exists in `/etc/passwd`      | ❌ No       |
| Home directory present in `/home` | ❌ No       |
| System / user mapping consistency | ✅ Restored |
| Cleanup completed                 | 🟢 Success |