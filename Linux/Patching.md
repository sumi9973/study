# Check security update only
yum clean all
yum check-update --security

# Do security update only
yum update --security -y

# Do security update only but exclude java packages
yum update --security --exclude=java* -y

# Update all packages including security updates
yum update -y

# What problem can happen if we do major update ?
If application is not compatible with new version of package then application can break. So we need to check the release notes of the package before doing major update.

> After applying the patch need to reboot the server


# How to update only specific package ?
yum update <package_name> -y

# How to update kernel only ?
yum update kernel -y

# How to switch to a specific kernel version ?
1. Check the available kernel versions using the command:
   ```bash
   rpm -q kernel
   ```
2. Set the default kernel version using the command:
   ```bash
   grub2-set-default <kernel_version>
   ```
3. Reboot the server to apply the changes.
   ```bash
    init 6
   ```
4. Verify the current kernel version using the command:
   ```bash
   uname -r
   ```
   
# How to remove old kernel versions ?
1. List all installed kernel versions using the command:
    ```bash
    rpm -q kernel
    ```
2. Remove the old kernel versions using the command:
    ```bash
    yum remove kernel-<version> -y
    ```
