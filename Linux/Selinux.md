# SELinux

A guide on how to enable, disable, and manage SELinux (temporarily and permanently), along with file contexts and troubleshooting.

---

## SELinux States

SELinux has three states:

1. **Enforcing** – SELinux policy is enforced.
2. **Permissive** – SELinux prints warnings instead of enforcing.
3. **Disabled** – SELinux is turned off completely.

---

## Check SELinux Status

```bash
getenforce
```

---

## Temporarily Enable / Disable SELinux

> These changes are temporary and will revert after reboot.

**Enable (Enforcing):**

```bash
setenforce 1
```

**Disable (Permissive):**

```bash
setenforce 0
```

**Example:**

```bash
[root@ip-172-31-39-3 ~]# setenforce 0
[root@ip-172-31-39-3 ~]# getenforce
Permissive
```

---

## Permanently Enable / Disable SELinux

To make the change permanent, edit the SELinux configuration file and reboot the server.

**Config file path:**

```bash
vim /etc/selinux/config
```

**Set one of the following values:**

```ini
SELINUX=enforcing
SELINUX=permissive
SELINUX=disabled
```

> After saving the file, **reboot** the server to apply the changes.

To completely disable SELinux, set `SELINUX=disabled` and reboot.

---

## SELinux File Context

Think of it like this:

- **SELinux = Lock**
- **File Context = Key**

For Apache (`httpd`), the expected file context is:

```
httpd_sys_content_t
```

### Check the File Context

```bash
ls -lZ
```

or

```bash
ls -Z
```

---

## Change File Context (Permanent)

Use `semanage` to add a new file context rule, then `restorecon` to apply it.

**Change context to `default_t`:**

```bash
semanage fcontext -a -t default_t "/var/www/html/image(/.*)?"
restorecon -Rv /var/www/html/image
```

**Example output:**

```bash
[root@ip-172-31-39-3 image]# semanage fcontext -a -t default_t "/var/www/html/image(/.*)?"
[root@ip-172-31-39-3 image]# restorecon -Rv /var/www/html/image
Relabeled /var/www/html/image from system_u:object_r:httpd_sys_content_t:s0 to system_u:object_r:default_t:s0
Relabeled /var/www/html/image/index.html from system_u:object_r:httpd_sys_content_t:s0 to system_u:object_r:default_t:s0

[root@ip-172-31-39-3 image]# ls -Z
system_u:object_r:default_t:s0 index.html
```

**Revert the context back:**

```bash
semanage fcontext -d "/var/www/html/image(/.*)?"
restorecon -Rv /var/www/html/image
```

**Example output:**

```bash
[root@ip-172-31-39-3 image]# semanage fcontext -d "/var/www/html/image(/.*)?"
[root@ip-172-31-39-3 image]# restorecon -Rv /var/www/html/image
Relabeled /var/www/html/image from system_u:object_r:default_t:s0 to system_u:object_r:httpd_sys_content_t:s0
Relabeled /var/www/html/image/index.html from system_u:object_r:default_t:s0 to system_u:object_r:httpd_sys_content_t:s0

[root@ip-172-31-39-3 image]# ls -Z
system_u:object_r:httpd_sys_content_t:s0 index.html
```

---

## Check SELinux Audit Logs

```bash
tail -n 500 /var/log/audit/audit.log | grep AVC
```

**Example output:**

```log
type=AVC msg=audit(1778845693.880:305): avc:  denied  { getattr } for  pid=1514 comm="httpd" path="/var/www/html/image/index.html" dev="nvme0n1p1" ino=9654345 scontext=system_u:system_r:httpd_t:s0 tcontext=system_u:object_r:default_t:s0 tclass=file permissive=0
type=AVC msg=audit(1778846065.991:328): avc:  denied  { read } for  pid=1514 comm="httpd" name="index.html" dev="nvme0n1p1" ino=9654345 scontext=system_u:system_r:httpd_t:s0 tcontext=system_u:object_r:default_t:s0 tclass=file permissive=1
type=AVC msg=audit(1778846065.991:329): avc:  denied  { open } for  pid=1514 comm="httpd" path="/var/www/html/image/index.html" dev="nvme0n1p1" ino=9654345 scontext=system_u:system_r:httpd_t:s0 tcontext=system_u:object_r:default_t:s0 tclass=file permissive=1
```

### What is AVC?

**AVC = Access Vector Cache**

It is the kernel component inside SELinux that makes the actual **"allow or deny"** decision for every security-relevant operation.

---

## Check the SELinux Context of a Process

```bash
ps -efZ | grep http
```

---

## Quick Reference

| Task | Command |
|------|---------|
| Check current SELinux mode | `getenforce` |
| Set SELinux to enforcing (temp) | `setenforce 1` |
| Set SELinux to permissive (temp) | `setenforce 0` |
| Permanent config file | `/etc/selinux/config` |
| View file context | `ls -Z` or `ls -lZ` |
| Add file context rule | `semanage fcontext -a -t <type> "<path>"` |
| Delete file context rule | `semanage fcontext -d "<path>"` |
| Apply file context | `restorecon -Rv <path>` |
| View AVC denials | `grep AVC /var/log/audit/audit.log` |
| View process context | `ps -efZ` |

