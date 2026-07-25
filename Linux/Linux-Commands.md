# Linux System Administration - Learning Plan
**1. User Management**
   Commands: useradd, usermod, userdel, passwd, chage
   Configuration files: /etc/passwd, /etc/shadow, /etc/group, /etc/sudoers
   Manage user permissions, default shell, and password aging
   
   ## Common User Management Tasks and Commands
   - Change User password
     - `passwd <username>`
   - Lock/Unlock User account
     - `passwd -l <username>`   (Lock)
     - `passwd -u <username>`   (Unlock)
     - `usermod -L <username>` (Lock)
     - `usermod -U <username>` (Unlock)
   - Add user to a group
     - `usermod -aG <groupname> <username>` (secondary group)
     - `usermod -g <groupname> <username>`  (primary group)
   - Create a user 
     - `useradd <username>`
     - `useradd -u <UID> <username>` (Specify UID)
     - `useradd -r -s /sbin/nologin serviceuser` (Create system user with no login shell)
     - root user UID is always 0
   - Delete a user and remove home directory
     -  userdel -r <username>
   - View user information
     - `cat /etc/passwd | grep <username>`
     - `chage -l <username>`   (View password information)
   - Set password expiration policies
     - `chage -M <days> <username>`   (Maximum days)
     - `chage -E <date> <username> `  (Account expiration)
   - Force password change on next login
     - `chage -d 0 <username>`
   - View last login details
     - `lastlog | grep <username>`
   - Switch user
     - `su - <username>`
   - View currently logged-in users
     - `who`
   - Change Shell for a user
     - `chsh -s /bin/<shellname> <username>` or ` usermod -s /bin/<shellname> <username>`
   - Create a group
     - `groupadd <groupname>`
   - Delete a group
     - `groupdel <groupname>`
   - Check last password change info
     - `chage -l <username>`
   - Default files for password policies
     - /etc/login.defs
   - What is /etc/bashrc ~/.bashrc and /etc/profile ~/profile
     - ~/.bashrc: User-specific bash configuration file that sets environment variables and startup programs for individual users.
     - /etc/bashrc: System-wide bash configuration file that sets environment variables and startup programs for all users.
     - ~/.bash_profile: User-specific profile configuration file that sets environment variables and startup programs for individual users.
     - ~/.bash_logout: User-specific file that contains commands that run when the user logs out.
   - How to refresh the shell 
     - `source ~/.bashrc` or `exec bash`

Q. What is the difference between primary and secondary groups?”
A. Primary group is the default group for files, secondary groups provide additional access.

Q. How do you give a user sudo access without root password?
A. run `visudo`
   Add this line `username ALL=(ALL) NOPASSWD: ALL`
   Now User can run sudo commands without being prompted for a password.

Q. Where is user password info stored
A. `/etc/shadow` is stored the pass related into.

Q. How do you recover if `/etc/passwd` is corrupted?
A. system keep a backup of this file as  `/etc/passwd-` and we can copy it from this backup file  `/etc/passwd-`
   `cp /etc/passwd- /etc/passwd` same applies for `/etc/shadow`

Q. Difference between `/etc/passwd` and `/etc/shadow`
A. Passwd: Stored User info.
   Shadow: Stored Password info.

Q. How to restrict user from SSH access?
A. `usermod -s /sbin/nologin username` # Change the login shell
    `passwd -l username` # Lock the password.

---

**2. File Management**
   File operations: create, delete, move, copy, rename (touch, rm, mv, cp)
   File permissions and ownership: chmod, chown, chgrp
   Access Control Lists (ACL): getfacl, setfacl
   Sudo privilege configuration (/etc/sudoers and visudo)
   
  ### Common File Management Tasks and Commands
   - From the ls -al command output, explain what each column represents.
     - ```-rw-r--r--   1 root root         207 Nov  3  2024 alerts.yml```
       - File type & permissions 
       - Number of links
       - Owner (user)
       - Group
       - File size
       - Last modified date & time
       - File or directory name
   - How to identify directory, files and soft links
     - Directory: d at the beginning of permissions (e.g., drwxr-xr-x)
     - File: - at the beginning of permissions (e.g., -rw-r--r--)
     - Soft link: l at the beginning of permissions (e.g., lrwxrwxrwx)
   - How to identify if ACL is set on a file or directory
     - Use `ls -l` command; a '+' sign at the end of the permissions indicates ACL is set (e.g., -rw-r--r--+)
   - How to see if chattr attributes are set on a file
     - Use `lsattr <filename>` command to view attributes
   - What are the octal values for file permissions
     - Read (r) = 4
     - Write (w) = 2
     - Execute (x) = 1
     - No permission (-) = 0
     - Example: rwxr-xr-- = 754
   - Difference between hard link and soft link
     - Hard Link: Points directly to the inode of a file; kind of copy of the file.
     - Soft Link (Symbolic Link): Points to the filename; it's a shortcut.
   - What is SUID, SGID, and Sticky bit
    - SUID (Set User ID): To allow normal user to run root privilege program, we will set SUID on that program.
    - SGID (Set Group ID): After setting SGID on a directory, files created within that directory automatically inherit the group ownership of the directory.
    - Sticky Bit: If other user has full permission on the directory, and want to restrict them from deleting or renaming it we will set sticky bit on that directory.
   - How to set SUID, SGID, and Sticky bit
     - SUID: `chmod u+s <filename>`
     - SGID: `chmod g+s <directoryname>`
     - Sticky Bit: `chmod +t <directoryname>`
   - How to identify SUID, SGID, and Sticky bit
     - SUID: s in the user execute position (e.g., -rwsr-xr-x)
     - SGID: s in the group execute position (e.g., -rwxr-sr-x)
     - Sticky Bit: t in the others execute position (e.g., drwxrwxrwt)
   - What is the difference between chmod 777 and chmod 666
     - chmod 777: Read, write, and execute permissions for owner, group, and others.
     - chmod 666: Read and write permissions for owner, group, and others; no execute permissions.
   - How to change file ownership recursively
     - `chown -R <owner>:<group> <directoryname>`
   - How to view hidden files in a directory
     - Use `ls -a` command to list all files, including hidden ones (files starting with a dot).
   - What is umask and how to set it
     - Umask is a default permission setting that determines the permissions for newly created files and directories.
     - Set umask using `umask <value>` command (e.g., umask 022)
     - for permanent change, add umask value in ~/.bashrc or /etc/bashrc.
   - How to change user or group ownership of a file
     - `chown <owner>:<group> <filename>`
   - How to copy file to remote server
     - Use `scp <localfile> <user>@<remotehost>:<remotepath>` command
   - What is the difference between scp and rsync
     - scp: Securely copies files between hosts over SSH; simpler for one-time transfers. Everytime it will copy the full file.
     - rsync: it supports incremental transfers. It only transfers changed parts of files, making it more efficient for regular backups and synchronization.
   - How to find a file by name
     - Use `find /path -name <filename>` command
   - How to compress and decompress files
     - Compress: `tar -czvf archive.tar.gz /path/to/directory_or_file`
       - c: create archive
       - z: compress with gzip
       - v: verbose output
       - f: specify filename "archive.tar.gz"
     - Example: `tar -czvf archive.tar.gz myfolder/`
     - Decompress: `tar -xzvf archive.tar.gz`
       - x: extract archive
       - z: decompress with gzip
     - gzip: `gzip <filename> (to compress)`
     - gunzip: `gunzip <filename>.gz (to decompress)`
   - How to see the contents of a compressed file without extracting
     - Use `zcat <filename>.gz` for gzip files
     - Use `tar -tzvf <archive.tar.gz>` for tar.gz files
       - t: list contents of archive
   - What is ACL and how to set it
     - Access Control List (ACL) allows setting more granular permissions for files and directories beyond the traditional owner/group/others model. We can set permissions for specific users or groups apart from traditional owner/group/others.
     - Set ACL using `setfacl` command (e.g., setfacl -m u:username:rwx <filename>)
   - How to view ACL of a file
     - Use `getfacl <filename>` command to view ACL entries
   - How to restrict root user access to a file
     - Use `chattr +i <filename>` to make the file immutable, preventing even root from modifying or deleting it.
   - How to give permission to a user to run specific commands with sudo without password
     - Run `visudo` command to edit the sudoers file safely.
     - Add the following line: `username ALL=(ALL) NOPASSWD: ALL`
   - How to allow only few commands for a user with sudo
     - Run `visudo` command to edit the sudoers file safely.
     - Add the following line: `username ALL=(ALL) NOPASSWD: /path/to/command1, /path/to/command2`
   - How to restrict few commands for a user with sudo
     - Run `visudo` command to edit the sudoers file safely.
     - Add the following line: `username ALL=(ALL) ALL, !/path/to/restricted_command`
   - What if we use vim /etc/sudoers instead of visudo
     - Directly editing /etc/sudoers with vim can lead to syntax errors that may lock you out of sudo access. visudo performs syntax checking before saving changes, preventing such issues.

---
    
**3. Package and Repository Management**
   Package operations: install, update, remove/uninstall
   Tools: yum, dnf, apt (depending on distribution)
   Repository configuration files and management

  ### Common Package and Repository Management Tasks and Commands
  - How to check installed package version
    - `rpm -q <package-name>`
    - `yum list installed <package-name> `
  - how to install any package
    - `yum install <package-name>`
  - How to remove/uninstall any package
    - `yum remove <package-name>`
  - How to install package from rpm url
    - `yum install <package-url.rpm>`
    - Example: `dnf install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm`
  - How to update all packages to the latest version
    - `yum update`
  - How to update only security-related packages
    - `yum update --security`
  - How to check available updates for packages
    - `yum check-update`
  - How to list all enabled repositories
    - `yum repolist`
  - How to list all repositories (enabled and disabled)
    - `yum repolist all`
  - How to add a new repository
    - Create a new .repo file in `/etc/yum.repos.d/` with repository details
    - new repo file should contain:
      ```
      [repo-name]
      name=Repository Name
      baseurl=http://path/to/repo/
      enabled=1 # If it is 1 means repo is enabled, 0 means disabled
      gpgcheck=1 # If it is 1 means GPG check is enabled, 0 means disabled. 
      gpgkey=http://path/to/gpgkey # GPG key URL
      ```
  - What is gpgcheck in yum repository
    - gpgcheck is a security feature that verifies the authenticity and integrity of packages using GPG signatures.
  - How to disable a repository temporarily
    - `yum --disablerepo=<repo-name> <command>`
  - How to disable a repository permanently
    - Edit the .repo file in /etc/yum.repos.d/ and set enabled=0
  - How to clean yum cache
    - `yum clean all`
  - What is the extension of yum repository files
    - .repo
  - How to check package dependencies
    - `yum deplist <package-name>`
  - How to list all the installed packages
    - `yum list installed`
  - How to list all the available packages from the repositories
    - `yum list available`
  - How to rollback a package to a previous version
    - `yum history`
    - `yum history info <transaction-id>`
    - `yum history undo <transaction-id>`

---

4. Service Management
   Manage services: systemctl start|stop|restart|status
   Service configuration files: /etc/systemd/system/
   Check logs and errors: journalctl -u <service-name>
   
   ## Common Service Management Tasks and Commands
   - How to list all services 
     - `systemctl list-units --type=service --all`
   - How to list only running services
     - `systemctl list-units --type=service --state=running`
   - What is the path for systemd service files
     - `/etc/systemd/system/` for custom services
     - `/usr/lib/systemd/system/` for package-installed services
   - How to enable a service to start on boot
     - `systemctl enable <service-name>`
   - How to disable a service from starting on boot
     - `systemctl disable <service-name>`
   - How to check the status of a service
     - `systemctl status <service-name>`
   - How to check if a service is enabled or disabled
     - `systemctl is-enabled <service-name>`
   - How to start a service
     - `systemctl start <service-name>`
   - How to stop a service
     - `systemctl stop <service-name>`
   - How to restart a service
     - `systemctl restart <service-name>`
   - How to reload a service configuration without restarting
     - `systemctl reload <service-name>`
   - How to view service logs
     - `journalctl -u <service-name>`
   - How to daemon-reload systemd manager configuration
     - `systemctl daemon-reload`
   - Q. What is the difference between systemctl start and systemctl enable?
     - A. `systemctl start <service-name>` starts the service immediately, while `systemctl enable <service-name>` configures the service to start automatically at boot time.
   - How many run levels are there in Linux?
     - There are 7 runlevels in Linux (0-6), but with systemd, these are replaced by targets.
     - Runlevel 0: Halt
     - Runlevel 1: Single-user mode
     - Runlevel 2: Multi-user mode without networking (NFS Won't work)
     - Runlevel 3: Multi-user mode with networking (text mode)
     - Runlevel 4: Undefined/Custom
     - Runlevel 5: Multi-user mode with GUI
     - Runlevel 6: Reboot
---

5. Process Management
   Commands: ps, top, htop, kill, nice, renice
   Start, stop, and monitor running processes
   Identify high CPU/memory consuming processes

   ## Common Process Management Tasks and Commands
   - How to view all running processes
     - `ps aux`
      - a: show processes for all users
      - u: display the process's user/owner
      - x: show processes not attached to a terminal
   - How to list the process for specific user
     - `ps -u <username>`
   - How to kill a process by PID
     - `kill <PID>`
   - How to kill a process by name
     - `pkill <process-name>`
   - What is the difference between kill and pkill
     - kill: Terminates a process using its PID.
     - pkill: Terminates processes based on name or other attributes.
   - How to force kill a process
     - `kill -9 <PID>`
   - How to gracefully stop a process
     - `kill -15 <PID>` or simply `kill <PID>` (default signal is SIGTERM)
   - What is the difference between SIGKILL and SIGTERM
     - SIGKILL (signal 9): Forcefully terminates a process; cannot be caught or ignored.
     - SIGTERM (signal 15): Politely requests a process to terminate; can be caught and handled.
   - How to find processes consuming high CPU or memory
     - `ps aux --sort=-%cpu | head` (for CPU)
   - How to view real-time system resource usage
     - `top` or `htop`
   - How to sort processes by memory or CPU usage in top command
     - In `top`, press 'M' to sort by memory and 'P' to sort by CPU
   - How to change the priority of a process
     - `renice <priority> -p <PID>`
   - How to start a process with a specific priority
     - `nice -n <priority> <command>`
   - What is the range of nice values
     - Nice values range from -20 (highest priority) to 19 (lowest priority)
   - What is the difference between renice and nice
     - nice: Starts a new process with a specified priority.
     - renice: Changes the priority of an already running process.
   - How to monitor a specific process continuously
     - `top -p <PID>` or `htop` and filter by PID
   - how to list the PPID and UID of a process
     - `ps -o pid,ppid,uid,cmd -p <PID>`
     - pidof <process-name> to get PID of a process
   - How many types of process states are there in Linux?
     - There are several process states in Linux, including:
       - R (Running): The process is currently running or ready to run.
       - S (Sleeping): The process is waiting for an event or resource.
       - D (Uninterruptible Sleep): The process is in a sleep state that cannot be interrupted.
       - Z (Zombie): The process has completed execution but still has an entry in the process table.
       - T (Stopped): The process has been stopped, usually by a signal.
       - I (Idle): The process is idle and not currently executing.
       - orphaned: A process whose parent has terminated.
   - What is the 1st process started by the Linux kernel?
     - The 1st process started by the Linux kernel is the "systemd" process, which has PID 1. It is responsible for initializing the system and starting all other processes.
   - How to run any command in background
     - Append `&` at the end of the command (e.g., `command &`)
     - example: `sleep 300 &`

---

6. Patching and Updates
   Apply OS patches and security updates
   Validate kernel and package versions before/after patching
   Rollback strategies and verification

    ## Common Patching and Update Tasks and Commands
    - How to check current kernel version
      - `uname -r`
      - `cat /proc/version`
    - How to list all installed kernels
      - `yum list kernel`
      - `ls /boot/vmlinuz-*`
    - How to update the kernel to the latest version.
      - `yum update kernel`
      - Reboot the system to apply the new kernel
    - What is the directory where kernel images are stored
      - `/boot/`
    - How to see the severity of available updates
      - `yum updateinfo list security`
      - `yum updateinfo summary`
    - RedHat doc url
     - https://access.redhat.com/security/security-updates/
     - https://access.redhat.com/security/updates/classification

---

7. Log Management and Debugging
   View system logs using journalctl
   Analyze logs under /var/log/
   Understand key logs: messages, secure, dmesg, boot.log

    ## Common Log Management and Debugging Tasks and Commands
    - How to view system logs using journalctl
      - `journalctl`
    - How to view logs for a specific service for real time.
      - `journalctl -u <service-name> -f`
    - How to view logs from the current boot
      - `journalctl -b`
    - How to view logs from previous boots
      - `journalctl -b -1` (for the previous boot)
    - How to view authentication and ssh logs
      - `cat /var/log/secure` (on RHEL-based systems)
    - How to view system messages log
      - `cat /var/log/messages` (on RHEL-based systems)
    - How to view boot logs
      - `cat /var/log/boot.log` 
    - What is the location of journal logs
      - `/var/log/journal/`
    - What is the journald service file location
      - `/etc/systemd/journald.conf`
    - What is the rsyslog.service
      - rsyslog takes logs from journald and stores them in related text log files like /var/log/messages and /var/log/secure.
    - How to rotate the logs to save disk space
      - `logrotate` is used to rotate logs based on configuration in `/etc/logrotate.conf` and `/etc/logrotate.d/`

---

8. File System and Disk Management
   Partition and manage disks: fdisk, parted, lsblk, df, du
   LVM management: pvcreate, vgcreate, lvcreate, lvextend, lvreduce
   Mount and unmount file systems (mount, umount, /etc/fstab)

  ## Common File System and Disk Management Tasks and Commands
    - How to view disk partitions
        - `lsblk`
        - `fdisk -l`
    - How to view disk usage
        - `df -hT` (for filesystem disk space usage)
        - `du -sh /path/to/directory` (for directory size)
    - How to see the mount point of any directory
        - `df -hT /path/to/directory`
    - How to mount a filesystem
        - `mount /dev/sdX1 /mnt/point` (replace X and 1 with appropriate letter and partition number)
    - How to unmount a filesystem
        - `umount /mnt/point` or `umount /dev/sdX1`
    - How to make a filesystem mount persistent across reboots
        - Add an entry in `/etc/fstab`
        Example:
        ```
         disk      mount point  filesystem   mountoptions     backup  fsck
        /dev/sdX1   /mnt/point   xfs           defaults        0       0
        ```
    - How to create a new filesystem
        - `mkfs.xfs /dev/sdb1` (xfs is filesystem and /dev/sdb1 partition number)
    - How to check and repair a filesystem
        - `fsck /dev/sdX1` (replace X and 1 with appropriate letter and partition number)
        - umount the filesystem before running fsck otherwise it will cause data corruption.
    - How to create a new partition
        - `fdisk /dev/sdX` (replace X with the appropriate letter)
            - n: new partition
            - p: primary partition
            - 1-4: partition number
            - Enter: accept default first sector
            - Enter: accept default last sector or specify size (+size)
            - P: print partition table
            - t: change partition type
            - w: write changes
    - How to extend a partition in AWS EC2 instance EBS volume (Non LVM setup)
        - First, increase the EBS volume size from AWS console or CLI
        - Then, rescan the SCSI bus to detect the new size
          - `echo 1 > /sys/class/block/sdX/device/rescan` (replace X with appropriate letter)   
        - Finally, use `growpart` to extend the partition
          - `growpart /dev/sdX 1` (replace X with appropriate letter and 1 with partition number)
        - Then use `resize2fs /dev/sdX1` (for ext4) or `xfs_growfs /mnt/point` (for xfs) to resize the filesystem
        - verify the new size using `df -hT`
    - What is LVM
        - LVM stand for (Logical Volume Manager) it is a logical volume of combination of one or more physical volumes.
        - We can extend the LVM on the fly without unmounting it.
        - When any application required more size we can extend the LVM volume and filesystem without downtime.
    - How to see the LVM configuration
        - `pvdisplay` (to display Physical Volumes)
        - `vgdisplay` (to display Volume Groups)
        - `lvdisplay` (to display Logical Volumes)
    - What is VG, PV, LV
        - PV (Physical Volume): It is the physical storage device (like /dev/sdb) that is used in LVM.
        - VG (Volume Group): It is a pool of storage created by combining one or more Physical Volumes.
        - LV (Logical Volume): It is a virtual partition created from the Volume Group that can be used like a regular partition.
    - How to create LVM
        - Create Physical Volume (PV)
            - `pvcreate /dev/sdX` (replace X with appropriate letter)
        - Create Volume Group (VG)
            - `vgcreate vgname /dev/sdX` (replace vgname with desired volume group name and X with appropriate letter)
        - Create Logical Volume (LV)
            - `lvcreate -L <size> -n <lvname> <vgname>` (replace size with desired size, lvname with desired logical volume name,and vgname with the volume group name)
        - Create Filesystem on LV
            - `mkfs.xfs /dev/vgname/lvname` (for xfs) or `mkfs.ext4 /dev/vgname/lvname` (for ext4)
        - Mount the LV
            - `mount /dev/vgname/lvname /mnt/point` (replace vgname, lvname, and /mnt/point with appropriate values)
            - make fstab entry for persistent mount
    - How to extend LVM in AWS
        - First, increase the EBS volume size from AWS console or CLI
        - Then, rescan the SCSI bus to detect the new size
          - `echo 1 > /sys/class/block/sdX/device/rescan` (replace X with appropriate letter)   
        - Finally, use `growpart` to extend the partition
          - `growpart /dev/sdX 1` (replace X with appropriate letter and 1 with partition number)
        - Next, extend the Physical Volume (PV)
          - `pvresize /dev/sdX` (replace X with appropriate letter)
        - Then, extend the Logical Volume (LV)
          - `lvextend -L +<size> /dev/vgname/lvname` (replace size with desired additional size, vgname with volume group name, and lvname with logical volume name)
        - Finally, resize the filesystem
          - `xfs_growfs /mnt/point` (for xfs) or `resize2fs /dev/vgname/lvname` (for ext4)
        - verify the new size using `df -hT`
    - What is the difference between primary and extended partition
        - Primary Partition: A primary partition is a type of partition that can contain a file system and can be used to boot an operating system. A disk can have up to four primary partitions.
        - Extended Partition: An extended partition is a special type of partition that can contain multiple logical partitions. It is used to overcome the limitation of having only four primary partitions on a disk. An extended partition itself does not contain data directly; instead, it acts as a container for logical partitions.
    - What is the difference between MBR and GPT partitioning scheme
        - MBR (Master Boot Record): An older partitioning scheme that supports up to four primary partitions and a maximum disk size of 2TB. It uses a single boot sector located at the beginning of the disk.
        - GPT (GUID Partition Table): A modern partitioning scheme that supports a virtually unlimited number of partitions and can handle disks larger than 2TB. It uses multiple copies of the partition table for redundancy and includes CRC32 checksums for data integrity.
    - What is the difference between fdisk and gdisk
        - fdisk: A command-line utility used for managing MBR partition tables. We can create 3 primary partitions and 1 extended partition on MBR disks.
        - gdisk: A command-line utility used for managing GPT partition tables. We can create 128 primary partitions on GPT disks.
    - What is mounting.
        - Mounting is the process of making a filesystem accessible at a specific directory (mount point) in the Linux directory tree. It allows the operating system and users to access files and directories stored on different storage devices.
    - What is the command to see the mount related information
        - `mount` command without any arguments displays all currently mounted filesystems along with their mount points and options.

---

9. Linux Runlevels and System Targets
   Legacy runlevels (init, /etc/inittab)
   Systemd targets (systemctl get-default, systemctl isolate)
   Multi-user, graphical, and rescue modes

  ## Common Linux Runlevels and System Targets Tasks and Commands
   - What is the difference between runlevels and systemd targets
     - Runlevels are a traditional way of defining system states in SysV init systems, while systemd targets are a more modern and flexible way of managing system states in systemd-based systems.
   - How to check the current default target
     - `systemctl get-default`
   - How to change the default target
     - `systemctl set-default <target-name>`
     - Example: `systemctl set-default multi-user.target`
   - How to switch to a different target without rebooting
     - `systemctl isolate <target-name>`
     - Example: `systemctl isolate rescue.target`
   - What are the common systemd targets
     - `multi-user.target`: Equivalent to runlevel 3 (text mode with networking)
     - `graphical.target`: Equivalent to runlevel 5 (graphical mode)
     - `rescue.target`: Single-user mode for system recovery
     - `emergency.target`: Minimal environment for emergency repairs
     - `default.target`: The default target that the system boots into
   - How to view the current runlevel (for SysV init systems)
     - `runlevel` command
     - or `who -r` command
   - What is the equivalent target for runlevel 3 and runlevel 5
     - Runlevel 3: `multi-user.target`
     - Runlevel 5: `graphical.target`
   - What is the command to check the target dependencies
     - `systemctl list-dependencies <target-name>`
     - Example: `systemctl list-dependencies multi-user.target`
---

    
10. Log Rotation
    Understand and configure log rotation (/etc/logrotate.conf, /etc/logrotate.d/)
    Manually trigger rotation using logrotate -f

    ## Common Log Rotation Tasks and Commands
    - What is logrotate
      - Logrotate is a utility that manages the automatic rotation, compression, and removal of log files to prevent them from consuming excessive disk space.
    - How to view logrotate configuration files
      - Main configuration file: `/etc/logrotate.conf`
      - Per-service configuration files: `/etc/logrotate.d/`
    - How to manually trigger log rotation
      - `logrotate -f /etc/logrotate.conf`
      -  When logrotate runs, it checks the last rotation date from /var/lib/logrotate/logrotate.status. Based on the rotation interval set in the config (daily, weekly, etc.), it decides whether to rotate the log. If rotation happens, it updates the status file with the new date.
    - Logrotate service file location
      - `/etc/systemd/system/logrotate.service`
      - `/etc/systemd/system/logrotate.timer`
    - How to check the logrotate service status
      - `systemctl status logrotate.service`
    - How to check the status of last log rotation
      - `cat /var/lib/logrotate/logrotate.status`
    - What are common logrotate options
      - `daily`, `weekly`, `monthly`: Frequency of rotation
      - `rotate <number>`: Number of old log files to keep
      - `compress`: Compress old log files
      - `missingok`: Do not error if the log file is missing
      - `notifempty`: Do not rotate the log if it is empty
      - `postrotate` and `prerotate`: Scripts to run before or after rotation
    - Dry Run Logrotate: To check when rotation would happen
      - `logrotate -d /etc/logrotate.conf`
    - Sample Logrotate Configuration
      - ```
        vim /etc/logrotate.d/messages ## And keep the below entry.
        # Target log file to be rotated
        /var/log/messages              # Target log file to be rotated
        {
           weekly                     # Rotate weekly
           rotate 4                   # Keep 4 backups
           compress                   # Compress old logs using gzip
           delaycompress              # Delay compression until next cycle (safer with copytruncate)
           missingok                  # Don’t error if log file is missing
           notifempty                 # Don’t rotate if the log file is empty
           copytruncate               # Copy + truncate the original log (keeps syslogd happy) don't use for rsyslogd.
           create 0640 root root      # Create new log with correct permissions
           sharedscripts              # Run postrotate only once for all files (if multiple)
           postrotate
                 [ -f /var/run/syslogd.pid ] && /bin/kill -HUP $(cat /var/run/syslogd.pid) 2> /dev/null || true
           endscript
        }
        ```
---


11. Scheduled Tasks (Cron Jobs)
    Create and manage cron jobs: crontab -e, /etc/cron.*
    System-wide vs user-level cron jobs
    Verify job execution and logs

    ## Common Scheduled Tasks and Cron Jobs Tasks and Commands
   - How to edit user-specific cron jobs
      - `crontab -e`
   - How to view user-specific cron jobs
      - `crontab -l`
   - How to remove user-specific cron jobs
      - `crontab -r`
   - Where are system-wide cron jobs located
      - `/etc/crontab`
      - `/etc/cron.d/`
      - `/etc/cron.daily/`, `/etc/cron.hourly/`, `/etc/cron.weekly/`, `/etc/cron.monthly/`
   - How to schedule a user level cron job
      - Edit the crontab file using `crontab -e` and add a line in the format:
        ```
        * * * * * /path/to/command 
        ```
        (Minute, Hour, Day of Month, Month, Day of Week)
      Note: if we are using crontab -e to schedule cron job, we don't need to specify the user. As this is user specific cron job.
   - How to check cron job execution logs
      - Check `/var/log/cron` for cron job execution logs
      Note: If any cron jon failed or executed successfully, it will be logged in /var/log/cron file and if failed we can see the reason from this log post that we will fix the issue.
   - What is the difference between system-wide and user-level cron jobs
      - System-wide cron jobs are defined in `/etc/crontab` and `/etc/cron.d/` and can run with different user privileges. User-level cron jobs are specific to individual users and are managed using `crontab -e`.
   - How to set system wide cron job
      - Edit `/etc/crontab` or create a file in `/etc/cron.d/` with the desired schedule and command.
      Example entry in /etc/crontab:
      ```
      * * * * * root /path/to/command
      ```
      Note: root is the username here, which means this cron job will run with root privilege.
   - Create a script to delete the 30 days old files from /tmp directory and schedule it in cron to run daily at 12 midnight.
      - Create the script:
        ```
        vim /usr/local/bin/cleanup_tmp.sh
        ```
        Add the following content:
        ```bash
        #!/bin/bash
        find /tmp -type f -mtime +30 -exec rm -f {} \;
        ```
      - Make the script executable:
        ```
        chmod +x /usr/local/bin/cleanup_tmp.sh
        ```
      - Schedule the script in cron:
        ```
        vim /etc/crontab
        ```
        Add the following line to run the script daily at 2 AM:
        ```
        0 0 * * * root /usr/local/bin/cleanup_tmp.sh
        ```
---


12. Important Configuration Files to Know
    
    | File                         | Purpose                                               |
    |------------------------------|-------------------------------------------------------|
    | `/etc/passwd`                | User account information                              |
    | `/etc/shadow`                | Encrypted passwords and password policies             |
    | `/etc/group`                 | User group information                                |
    | `/etc/sudoers`               | Sudo privileges configuration                         |
    | `/etc/fstab`                 | Filesystem mount configuration                        |
    | `/etc/hosts`                 | Static hostname to IP mapping                         |
    | `/etc/resolv.conf`           | DNS resolver configuration                            |
    | `/etc/hostname`              | System hostname                                       |
    | `/etc/ssh/sshd_config`       | SSH daemon configuration                              |
    | `/etc/logrotate.conf`        | Log rotation main config                              |
    | `/etc/logrotate.d/`          | Log rotation per-service configs                      |
    | `/etc/crontab`               | System-wide cron jobs                                 |
    | `/etc/systemd/system/`       | Custom systemd service unit files                     |
    | `/usr/lib/systemd/system/`   | Default systemd service unit files                    |
    | `/etc/nginx/nginx.conf`      | Main Nginx configuration file                         |
    | `/etc/nginx/conf.d/`         | Site-specific or service-specific configuration files |
    | `/var/log/nginx/`            | Nginx access and error logs directory                 |
    | `/var/log/nginx/access.log`  | Nginx access access logs                              |
    | `/var/log/nginx/error.log`   | Nginx access and error logs                           |
    | `/etc/systemd/journald.conf` | Systemd journal configuration                         |
    | `/etc/rsyslog.conf`          | Rsyslog daemon configuration                          |
    | `/etc/login.defs`            | User login and password policies                      |
    | `/etc/yum.repos.d/`          | Yum repository configuration files                    |
    | `/etc/bashrc`                | System-wide bash shell configuration                  |
    | `~/.bashrc`                  | User-specific bash shell configuration                |
    | `/etc/profile`               | System-wide profile configuration                     |

---

13. Important Ports to Know
    Port Number | Service
    ------------|----------------------
    22          | SSH
    3389        | RDP
    80          | HTTP
    443         | HTTPS
    25          | SMTP
    21          | FTP
    23          | Telnet
    2049        | NFS
    1028        | NFS-Client
    123         | NTP
    53          | DNS
    3306        | MySQL
    5432        | PostgreSQL
    6379        | Redis
    27017       | MongoDB 
    9090        | Prometheus
    3000        | Grafana
    9100        | Node Exporter
---


14. Important directory to Know
    | Directory         | Purpose                                         |
    |-------------------|-------------------------------------------------|
    | `/etc/`           | System configuration files                      |
    | `/var/log/`      | System and application log files                 |
    | `/home/`         | User home directories                            |
    | `/root/`         | Root user home directory                         |
    | `/usr/bin/`      | Normal User command binaries                     |
    | `/usr/sbin/`     | Root user command binaries                       |
    | `/bin/`          | Essential command binaries                       |
    | `/sbin/`         | System administration binaries                   |
    | `/var/www/`      | Web server document root                         |
    | `/tmp/`          | Temporary files                                  |
    | `/opt/`          | Optional software packages                       |
    | `/dev/`          | Device files                                     |
    | `/proc/`         | Virtual filesystem for process and kernel info   |
    | `/mnt/`          | Temporary mount points                           |
    | `/media/`        | Removable media mount points                     |
    | `/lib/`          | Essential shared libraries                       |
    | `/boot/`         | Bootloader files and kernel images               |
---

15. DNS

  ## Common DNS Tasks and Commands
   - What is DNS
     - DNS stands for Domain Name System is used to translates/resolve domain names (like www.example.com) into IP addresses and vice versa.
   - How to check DNS resolution for a domain
     - `nslookup <domain-name>`
     - `dig <domain-name>`
   - How to check reverse DNS resolution for an IP address
     - `nslookup <IP-address>`
     - `dig -x <IP-address>`
   - What is A record in DNS. 
     - A record (Address Record) maps a domain name to its corresponding IPv4 address.
     - Note: A record also known as forward DNS record.
   - What is CNAME record in DNS
     - CNAME record (Canonical Name Record) maps a domain name to another domain name (aliasing).
   - What is MX record in DNS
     - MX record (Mail Exchange Record) specifies the mail servers responsible for receiving email for a domain.
   - What is TXT record in DNS
     - TXT record (Text Record) is used to store arbitrary text data associated with a domain, often used for verification and security purposes (like SPF, DKIM).
   - What is PTR record in DNS
     - PTR record (Pointer Record) is used for reverse DNS lookups, mapping an IP address to its corresponding domain name.
     - Note: PTR record also known as reverse DNS record.
   - AAAA is for which IP version
     - AAAA record maps a domain name to its corresponding IPv6 address.
   - What is Name Server
     - Name Server (NS) is a server that stores DNS records and responds to DNS queries for a specific domain or set of domains.
   - What is local dns file in Linux
     - The local DNS file in Linux is `/etc/hosts`, which is used to map hostnames to IP addresses locally on the system.
   - Which file is used to configure DNS resolvers in Linux
     - The file used to configure DNS resolvers in Linux is `/etc/resolv.conf`, which contains the IP addresses of DNS servers that the system should use for name resolution.
   - Server always looks for local dns file before querying the dns server.
---


16. Networking Commands
    ## Common Networking Tasks and Commands
     - How to check network interfaces and their IP addresses
         - `ip addr show` or `ifconfig`
     - How to check active network connections and listening ports
         - `netstat -tulnp` or `ss -tuln`
           - t: TCP
           - u: UDP
           - l: listening
           - n: numeric addresses
           - p: show process using the port
         - `netstat -tulnp | grep 22` (to filter for port 22 is listening or not)
     - How to test connectivity to a remote host
         - `ping <hostname/IP-address>`
     - How to trace the route to a remote host
         - `traceroute <hostname/IP-address>` or `tracepath <hostname/IP-address>`
     - How to display DNS information for a domain
         - `dig <domain-name>` or `nslookup <domain-name>`
     - How to display open ports on a remote host
         - `nmap <hostname/IP-address>`
         - `nmap <hostname/IP-address> -p <port>`
           - open: port is open and reachable
           - closed: port is open in SG but no application is running on that port
           - filtered: port is blocked by Firewall/SG/NACL
     - How to display network statistics (optional)
         - `netstat -s` or `ss -s`
---


17. Network File Sharing - NFS

    ## Common NFS Tasks and Commands
    - What is NFS
      - NFS stands for Network File System, it is a centralized file server used to share files and directories over a network between Linux/Unix systems.
    - How to install NFS server packages
      - `yum install nfs-utils` (on RHEL-based systems)
    - How to start and enable NFS server service
      - `systemctl start nfs-server`
      - `systemctl enable nfs-server`
    - How to configure NFS exports
      - Edit the `/etc/exports` file to define directories to be shared and their access permissions.
      Example entry:
      ```
      /mnt/nfs_share *(rw,sync,no_root_squash) ## To all clients
      /mnt/nfs_share 192.168.1.0/24(rw,sync,no_root_squash) ## To specific subnet
        ```
        - Here, `/mnt/nfs_share` is the directory to be shared, `rw` allows read and write access, `sync` ensures data is written to disk before responding to requests, and `no_root_squash` allows root users on client machines to have root access on the NFS share.
        - After editing the exports file, run `exportfs -avr` to apply the changes.
        - Restart the NFS server service using `systemctl restart nfs-server`
        - Allow NFS ports in firewall using: (Only if firewall is enabled)
          - `firewall-cmd --permanent --add-service=nfs`
          - `firewall-cmd --reload`
          - If it's EC2 instance, make sure the security group allows NFS port 2049.
    - How to mount NFS share on client machines
      - Create a mount point:
        - `mkdir -p /mnt/nfs_client`
      - Mount the NFS share:
        - `mount -t nfs <nfs-server-ip>:/mnt/nfs_share /mnt/nfs_client`
      - To make the mount persistent across reboots, add an entry in `/etc/fstab`:
        ```
        <nfs-server-ip>:/mnt/nfs_share  /mnt/nfs_client  nfs  defaults  0  0
        ```
    - How to check NFS client mounts
      - `showmount -e <nfs-server-ip>`
---

18. Boot Process: Explain what happens from the moment you power on a Linux system till you get the login prompt?

**Answer:**
1. **BIOS** – Initializes hardware and finds boot device.
2. **MBR/GPT** – Loads the bootloader (GRUB).
3. **GRUB** – Loads the kernel and initrd.
4. **Kernel** – Initializes drivers, mounts root FS.
5. **init/systemd** – Starts target(enabled) services.
6. **Login Prompt** – System ready for user login.

---
19. Important Commands

    | Command         | Purpose                                          |
    |-----------------|------------------------------------------------- |
    | `ls`            | List directory contents                          |
    | `cd`            | Change directory                                 |
    | `pwd`           | Print working directory                          |
    | `cp`            | Copy files and directories                       |
    | `mv`            | Move or rename files and directories             |
    | `rm -rf`        | Remove files and directories                     |
    | `touch`         | Create an empty file                             |
    | `mkdir`         | Create a new directory                           |
    | `cat`           | display file contents                            |
    | `head -n 10`    | Display the first few lines of a file            |
    | `tail -f`       | Display the last few lines of a file             |
    | `echo`          | Print text to the terminal or a file             |
    | `grep`          | Search for patterns in files                     |
    | `find`          | Search for files and directories                 |
    | `chmod`         | Change file permissions                          |
    | `chown`         | Change file ownership                            |
    | `df`            | Display disk space usage                         |
    | `du`            | Display directory size                           |
    | `ps`            | Display running processes                        |
    | `top`           | Monitor system processes in real-time            |
    | `kill`          | Terminate processes                              |
    | `systemctl`     | Manage systemd services                          |
    | `journalctl`    | View system logs                                 |
    | `yum`           | Package management (RHEL-based systems)          |
    | `ssh`           | Securely connect to remote systems               |
    | `scp`           | Securely copy files between systems              |
    | `rsync`         | Synchronize files and directories                |
    | `sed`           | Stream editor for filtering and transforming text|
    | `awk`           | Pattern scanning and processing language         |
    | `curl`          | Command Line browser                             |