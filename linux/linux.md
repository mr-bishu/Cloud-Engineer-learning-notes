# Linux Basics — Cloud Engineering Journey

## About This Document

This document contains my Linux fundamentals practice as part of my journey toward becoming an AWS Cloud Engineer.

**Learning goals:**
- Understand the Linux file system.
- Learn essential Linux commands.
- Create and manage files and directories.
- Understand Linux file permissions.
- Practice basic Linux administration and security.

---

## Day 1: Linux Fundamentals

### 1. What Is Linux?

Linux is an open-source operating system widely used for servers, cloud computing, networking, cybersecurity, and DevOps.

**Why Linux is important for Cloud Engineers:**
- Many AWS EC2 instances run Linux.
- Linux is used to host websites and applications.
- Cloud engineers use Linux commands to manage servers.
- Linux supports automation using Bash scripts.
- Linux helps with troubleshooting, monitoring, and security.

### 2. Basic Linux Commands

| Command | Purpose | Example |
|---|---|---|
| `pwd` | Display the current directory | `pwd` |
| `ls` | List files and directories | `ls` |
| `ls -la` | List all files, including hidden files, with details | `ls -la` |
| `cd` | Change directory | `cd Documents` |
| `mkdir` | Create a directory | `mkdir projects` |
| `whoami` | Display the current username | `whoami` |
| `clear` | Clear the terminal screen | `clear` |
| `history` | Show previously executed commands | `history` |

### 3. Practical Exercise: Create a Directory

Run these commands in the terminal:

```bash
pwd
whoami
mkdir cloud-practice
cd cloud-practice
pwd
```

**Expected result:**
- Your current directory is displayed.
- Your username is displayed.
- A directory named `cloud-practice` is created.
- You enter the new directory.

### 4. Practice Questions

1. Which command displays the current directory?
2. Which command creates a directory?
3. Which command displays the current username?
4. What is the purpose of Linux in cloud computing?

---

## Day 2: Files, Directories, and Permissions

### 1. Linux File System Structure

Linux organizes files and directories in a hierarchical structure starting from the root directory `/`.

| Directory | Purpose |
|---|---|
| `/` | Root of the Linux file system |
| `/home` | Home directories for regular users |
| `/etc` | System configuration files |
| `/var/log` | System and application log files |
| `/tmp` | Temporary files |
| `/var/www` | Common location for website files |
| `/usr/bin` | Many standard user commands and executable programs |
| `/root` | Home directory of the root user |

### 2. File Management Commands

| Command | Purpose | Example |
|---|---|---|
| `touch` | Create an empty file if it does not exist | `touch notes.txt` |
| `cat` | Display file contents | `cat notes.txt` |
| `cp` | Copy a file | `cp notes.txt backup.txt` |
| `mv` | Move or rename a file | `mv notes.txt linux-notes.txt` |
| `rm` | Delete a file | `rm unwanted.txt` |
| `file` | Identify a file's type | `file notes.txt` |

**Important:** Be careful when using `rm`. Deleted files may not be recoverable easily.

### 3. Practical Exercise: Create and Manage a File

```bash
mkdir linux-practice
cd linux-practice
touch notes.txt
echo "I am learning Linux for AWS Cloud Engineering." > notes.txt
cat notes.txt
cp notes.txt backup.txt
mv backup.txt linux-backup.txt
ls -l
```

**Expected result:**
- A directory named `linux-practice` is created.
- A file named `notes.txt` is created.
- Text is written to and displayed from the file.
- A backup copy is created and renamed to `linux-backup.txt`.
- The final directory contains both files.

### 4. Understanding Linux Permissions

Linux permissions determine who can read, modify, or execute files.

There are three basic permission types:

| Permission | Symbol | Meaning |
|---|---|---|
| Read | `r` | Read a file or list directory contents |
| Write | `w` | Modify a file or change directory contents |
| Execute | `x` | Execute a program or access a directory |

Permissions are assigned to three categories:

- **Owner:** The user who owns the file.
- **Group:** Users belonging to the file's group.
- **Others:** All other users.

### 5. Understanding `ls -l`

Example output:

```text
-rwxr-xr--
```

The first character indicates the file type. The remaining nine characters represent permissions for the owner, group, and others.

- Owner: `rwx` — read, write, execute.
- Group: `r-x` — read and execute.
- Others: `r--` — read only.

### 6. Understanding `chmod`

The `chmod` command changes file permissions.

| Command | Meaning |
|---|---|
| `chmod 600 notes.txt` | Owner can read and write; group and others have no permissions |
| `chmod 644 notes.txt` | Owner can read and write; group and others can read |
| `chmod 755 script.sh` | Owner has all permissions; group and others can read and execute |
| `chmod u+x script.sh` | Add execute permission for the owner |

### 7. How Numeric Permissions Work

Linux represents permissions using numbers:

| Permission | Number |
|---|---:|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |

Add the numbers to calculate a permission value.

Examples:

- `7 = 4 + 2 + 1` → read, write, execute.
- `6 = 4 + 2` → read and write.
- `5 = 4 + 1` → read and execute.
- `4 = 4` → read only.

In `chmod 755 script.sh`:
- Owner: `7` → `rwx`.
- Group: `5` → `r-x`.
- Others: `5` → `r-x`.

### 8. Practical Exercise: File Permissions

```bash
touch secret.txt
chmod 600 secret.txt
ls -l secret.txt
```

This sets the file's permissions so that the owner can read and write it, while group and others receive no permissions through the normal Unix permission bits.

**Security note:** Permissions help protect files, but access can also depend on the user account, directory permissions, ACLs, and other security controls.

### 9. Why Protect SSH Private Keys?

SSH private keys are used to authenticate securely to remote systems, including cloud servers.

Protect private keys because:
- Someone with access to a usable private key may be able to authenticate as its owner.
- Private keys should not be shared or committed to public GitHub repositories.
- Restrictive permissions help prevent unauthorized local users from reading them.

On Linux, a private key file commonly uses:

```bash
chmod 600 ~/.ssh/id_ed25519
```

Use the actual path and filename of your private key.

---

## Day 1 and Day 2 Revision

### Important Commands

```bash
pwd
ls -la
whoami
mkdir projects
cd projects
touch notes.txt
cat notes.txt
cp notes.txt backup.txt
mv backup.txt final-notes.txt
ls -l
chmod 600 notes.txt
chmod 755 script.sh
```

### Interview Questions

**1. What is Linux?**

Linux is an open-source operating system commonly used for servers, cloud computing, networking, and automation.

**2. Why do cloud engineers use Linux?**

Linux is widely used to host applications and services, manage cloud servers, automate tasks, and troubleshoot infrastructure.

**3. What does `pwd` do?**

It displays the current working directory.

**4. What is the difference between `cp` and `mv`?**

`cp` copies files, while `mv` moves or renames files.

**5. What does `chmod 600` mean?**

The owner can read and write the file; group and others have no permissions.

**6. What does `chmod 755` mean?**

The owner can read, write, and execute; group and others can read and execute.

**7. What is the purpose of `/etc`?**

It stores system-wide configuration files.

**8. Why should private SSH keys be protected?**

They can be used to authenticate to remote systems, so unauthorized access could compromise those systems.

---

## Practice Checklist

- [ ] I can use `pwd`, `ls`, and `cd`.
- [ ] I can create directories using `mkdir`.
- [ ] I can create files using `touch`.
- [ ] I can read file contents using `cat`.
- [ ] I understand `cp`, `mv`, and `rm`.
- [ ] I understand the Linux directory structure.
- [ ] I understand read, write, and execute permissions.
- [ ] I can explain `chmod 600` and `chmod 755`.
- [ ] I understand why SSH private keys must be protected.

---

## Learning Progress

- **Day 1:** Linux fundamentals and basic navigation.
- **Day 2:** File management and permissions.
- **Next:** Day 3 — Linux users, groups, ownership, and `sudo`.

**Goal:** Build practical Linux skills for AWS Cloud Engineering, with hands-on exercises and interview preparation.

