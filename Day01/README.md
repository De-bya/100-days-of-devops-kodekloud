# Day 01 – Create Linux User with Non-Interactive Shell

## Task
To accommodate the backup agent tool's specifications, the system admin team at **xFusionCorp Industries** requires the creation of a user with a non-interactive shell.

Create a user named `yousuf` with a non-interactive shell on **App Server 2**.

This type of user is commonly used for services and automation where login access is not required.

---

## Lab Environment
- **Jump Host:** `thor@jumphost`
- **Target Server:** App Server 2 (`stapp02.stratos.xfusioncorp.com`)
- **Server User:** `steve`

---

## Understanding Non-Interactive Shells

A **non-interactive shell** (`/sbin/nologin`) prevents a user from logging into the system interactively. This is a security best practice for:
- Service accounts that run background processes
- Backup agents and automated tools
- Application users that don't need terminal access
- Automated systems that only need file ownership or process execution

---

## Steps

### 1. Connect to the Server from Jump Host

```bash
ssh steve@stapp02.stratos.xfusioncorp.com
```

**Note:** 
- You'll see an SSH key fingerprint warning on first connection - type `yes` to continue
- You'll be prompted for the password. Enter the correct password for user `steve`
- The key fingerprint will be permanently added to known hosts

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
- `1002` - User ID (UID) - may vary depending on existing users
- `1002` - Group ID (GID) - typically same as UID for new users
- `` - GECOS field (user info, empty here)
- `/home/yousuf` - Home directory
- `/sbin/nologin` - Login shell (non-interactive)

### 4. Exit from App Server 2

```bash
exit
```

This returns you to the jump host (`thor@jumphost`).

---


### Check user's shell
```bash
grep yousuf /etc/passwd
```

