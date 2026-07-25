Linux has 6 steps of Boot Process. The steps are as follows:
1. BIOS: It check all the hardware and load the bootloader from the disk.
2. MBR: It contains the bootloader and partition table.
3. GRUB: It load the kernel.
4. Kernel: It start 1st process systemd, and mounts the root filesystem.
5. Systemd: It start all the enabled services.
6. Run Level: It start the default run level and user can login to the system.