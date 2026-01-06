# Day 01 – Create Linux User with Non-Interactive Shell

## Task
Create a user named `yousuf` with a non-interactive shell on **App Server 2**.
This type of user is commonly used for services and automation where login access is not required.

---

## Understanding Non-Interactive Shells

A **non-interactive shell** (`/sbin/nologin`) prevents a user from logging into the system interactively. This is a security best practice for:
- Service accounts that run background processes
- Application users that don't need terminal access
- Automated systems that only need file ownership or process execution

---

## Steps

### 1. Connect to the Server

```bash
ssh steve@stapp02.stratos.xfusioncorp.com
```

**Note:** You'll be prompted for the password. Enter the correct password for user `steve`.

---

### 2. Create the User with Non-Interactive Shell

```bash
sudo useradd -s /sbin/nologin yousuf
```

**Explanation of flags:**
- `-s /sbin/nologin` : Sets the user's shell to `/sbin/nologin`, which denies interactive login
- Alternative shells for non-interactive users: `/bin/false` or `/usr/sbin/nologin`

**What happens:**
- Creates user `yousuf`
- Assigns a unique UID (User ID)
- Creates a home directory at `/home/yousuf` (by default)
- Sets the shell to `/sbin/nologin`

---

### 3. Verify the User Creation

```bash
getent passwd yousuf
```

**Expected output:**
```
yousuf:x:1002:1002::/home/yousuf:/sbin/nologin
```

**Understanding the output:**
- `yousuf` - Username
- `x` - Password is stored in `/etc/shadow` (encrypted)
- `1002` - User ID (UID)
- `1002` - Group ID (GID)
- `` - GECOS field (user info, empty here)
- `/home/yousuf` - Home directory
- `/sbin/nologin` - Login shell (non-interactive)

---

### 4. (Optional) Test Non-Interactive Behavior

Try to switch to the user to confirm login is disabled:

```bash
su - yousuf
```

**Expected result:**
```
This account is currently not available.
```

This confirms the non-interactive shell is working correctly.

---

## Additional Commands (Optional)

### Set a password (if needed for sudo operations)
```bash
sudo passwd yousuf
```

### View user details
```bash
id yousuf
```

### Check user's shell
```bash
grep yousuf /etc/passwd
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Permission denied when running `useradd` | Use `sudo` prefix |
| User already exists | Use `sudo userdel yousuf` to remove, then recreate |
| Cannot verify user | Ensure you're on the correct server (stapp02) |

---

## Summary

✅ Successfully created user `yousuf` with non-interactive shell  
✅ Verified user configuration  
✅ Ensured security by preventing interactive login
