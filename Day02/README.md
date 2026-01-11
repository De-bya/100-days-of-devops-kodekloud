# Day 02 – Create Linux Temporary User with Expiry Date

## Task
As part of the temporary assignment to the **Nautilus** project, a developer named `mariyam` requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed.

Create a user named `mariyam` on **App Server 2** in Stratos Datacenter. Set the expiry date to `2024-01-28`, ensuring the user is created in lowercase as per standard protocol.

---

## Lab Environment
- **Jump Host:** `thor@jumphost`
- **Target Server:** App Server 2 (`stapp02`)
- **Server User:** `steve`

---

## Understanding User Account Expiry

**Account expiry** is different from password expiry:
- **Account expiry**: The account becomes completely inaccessible after the specified date
- **Use cases**: Temporary contractors, project-based access, trial periods
- **Security benefit**: Automatically revokes access without manual intervention

When an account expires, the user cannot log in even with the correct password.

---

## Steps

### 1. Connect to App Server 2 from Jump Host

```bash
ssh steve@stapp02
```

### 2. Create the User with Expiry Date

```bash
sudo useradd -e 2024-01-28 mariyam
```

**Explanation of flags:**
- `useradd` : Command to create a new user account
- `-e 2024-01-28` : Sets the account expiration date (YYYY-MM-DD format)
- `mariyam` : Username (in lowercase as per protocol)

---

### 3. Verify User Account Expiry

Use the `chage` command to check account aging information:

```bash
sudo chage -l mariyam
```

**Expected output:**
```
Last password change                    : Dec 27, 2025
Password expires                        : never
Password inactive                       : never
Account expires                         : Jan 28, 2024
Minimum number of days between password change : 0
Maximum number of days between password change : 99999
Number of days of warning before password expires : 7
```

**Key fields to verify:**
- **Account expires**: Should show `Jan 28, 2024`
- **Password expires**: Shows `never` (only account expiry is set)
- **Last password change**: Shows current date when account was created

---

### 4. (Optional) Verify User in Password File

```bash
getent passwd mariyam
```

**Expected output:**
```
mariyam:x:1001:1001::/home/mariyam:/bin/bash
```

This confirms the user exists in the system.

---

### 5. Exit from App Server 2

```bash
exit
```

This returns you to the jump host (`thor@jumphost`).

---

## Complete Command Sequence (As Shown in Lab)

From jump host to completion:

```bash
# Connect to App Server 2
ssh steve@stapp02

# Create user with expiry date
sudo useradd -e 2024-01-28 mariyam

# Verify account expiry settings
sudo chage -l mariyam

# Exit back to jump host
exit
```

---

## Additional Commands Reference

### Modify existing user's expiry date
```bash
sudo usermod -e 2024-01-28 mariyam
```

### Remove expiry date (set to never expire)
```bash
sudo usermod -e "" mariyam
# or
sudo chage -E -1 mariyam
```

### Check specific expiry date only
```bash
sudo chage -l mariyam | grep "Account expires"
```

### Set password for the user 
```bash
sudo passwd mariyam
```

---

## Understanding the `chage` Command

The `chage` command (change age) is used to manage password and account expiry:

| Option | Description |
|--------|-------------|
| `-l` | List account aging information |
| `-E` | Set account expiration date |
| `-M` | Set maximum password age |
| `-m` | Set minimum password age |
| `-W` | Set password expiration warning days |

---

## What Happens After Expiry?

After January 28, 2024:
- User `mariyam` cannot log in to the system
- Any running processes owned by the user continue to run
- Files and home directory remain intact
- Administrator must manually extend expiry or remove the account

To extend access after creation:
```bash
sudo usermod -e 2024-03-31 mariyam
```

---

## Key Takeaways

1. **Temporary users** should always have expiry dates for security
2. **`useradd -e`** sets expiry during user creation
3. **`chage -l`** verifies expiry and password aging settings
4. **Date format** must be YYYY-MM-DD
5. **Account expiry** is different from password expiry
6. Expired accounts cannot login but files remain intact
