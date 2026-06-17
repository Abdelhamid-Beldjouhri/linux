# Managing File Permissions in Linux

## Project Description

As a security professional supporting the research team, I reviewed the file permissions in the `/home/researcher2/projects` directory to ensure they aligned with the organization's authorization policy. Using Linux commands such as `ls -la` and `chmod`, I identified permission settings that did not match the required access levels and corrected them to maintain proper security and confidentiality.

## Scenario

This document covers the file structure of the `/home/researcher2/projects` directory and the permissions of the files and subdirectory it contains. The goal was to audit existing permissions, identify any that didn't match the required authorization, and correct them using Linux commands.

---

## Step 1: Check File and Directory Details

**Command:**
```bash
ls -la /home/researcher2/projects
```

This command lists all files and directories — including hidden ones — inside `projects`, along with detailed information such as permissions, ownership, and group.

**Current Permissions Found:**

| File / Directory | Permission String | User | Group | Other |
|---|---|---|---|---|
| `project_k.txt` | `-rw-rw-rw-` | read, write | read, write | read, write |
| `project_m.txt` | `-rw-r-----` | read, write | read | none |
| `project_r.txt` | `-rw-rw-r--` | read, write | read, write | read |
| `project_t.txt` | `-rw-rw-r--` | read, write | read, write | read |
| `.project_x.txt` | `-rw--w----` | read, write | write | none |
| `drafts/` | `drwx--x---` | read, write, execute | execute | none |

---

## Step 2: Describe the Permissions String

Example breakdown using `project_r.txt`:

```
-rw-rw-r--
```

- **Field 1 (`-`):** Indicates this is a regular file, not a directory (a directory would show `d`).
- **Fields 2–4 (`rw-`):** **User** permissions (owner, `researcher2`) — can read and write, but not execute (not applicable to a text file).
- **Fields 5–7 (`rw-`):** **Group** permissions — same as user: read and write, no execute.
- **Fields 8–10 (`r--`):** **Other** permissions (any other user on the system) — can only read; no write or execute.

---

## Step 3: Change File Permissions

**Issue identified:** Organization policy prohibits write access for "Other" on any file. `project_k.txt` violated this policy with `Other = read, write`.

**Command:**
```bash
chmod o-w project_k.txt
```

**Explanation:** `o-w` removes (`-`) the write (`w`) permission from Other (`o`), without affecting User or Group permissions.

**Result:**
```
-rw-rw-r--
```

---

## Step 4: Change File Permissions on a Hidden File

**Requirement:** `.project_x.txt` should have no write permission for anyone, while User and Group retain read access.

**Command:**
```bash
chmod u=r,g=r,o= .project_x.txt
```

**Explanation:** This command sets permissions absolutely rather than adding/removing individual bits: `u=r` sets User to read-only, `g=r` sets Group to read-only, and `o=` removes all permissions for Other. The leading period in the filename indicates this is a hidden file.

**Result:**
```
-r--r-----
```

---

## Step 5: Change Directory Permissions

**Requirement:** Only `researcher2` (the owner) should be able to access the `drafts` directory and its contents.

**Command:**
```bash
chmod -R g-x,o-x drafts
```

**Explanation:** `g-x` removes execute permission from Group, and `o-x` removes execute (and effectively all meaningful access) from Other, restricting access to only the directory's owner. The `-R` flag applies the change recursively to the directory's contents as well.

**Result:**
```
drwx------
```

---

## Summary

Through this activity, I demonstrated the ability to examine file and directory permissions using `ls -la`, interpret the 10-character permission string, and modify permissions with `chmod` to enforce least-privilege access. I corrected a file with improper write access for others, secured a hidden file to remove unauthorized write permissions, and restricted a directory to a single authorized user. These skills are essential for maintaining authorization controls and protecting sensitive data within an organization's file system.

---

### Skills Demonstrated
- Linux command-line navigation and file inspection (`ls -la`)
- Interpreting Linux permission strings (user/group/other, read/write/execute)
- Modifying permissions with `chmod` (symbolic mode: relative `+`/`-` and absolute `=`)
- Applying the principle of least privilege
- Managing hidden files and recursive directory permissions
