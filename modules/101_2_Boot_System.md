# TOPIC 102: Boot the System

## Provide common commands to the boot loader and options to the kernel at boot time 

This section provides a step-by-step guide on how to access and modify GRUB boot parameters.

**Steps:**

1.  **Open VM:** Start your Virtual Machine.
2.  **Access GRUB Menu:** As soon as the black screen appears after the VM's splash screen, repeatedly press the `Esc` key (or `Shift` key in some cases, depending on your GRUB configuration) to open the GRUB menu.
3.  **Edit Boot Entry:** Once in the GRUB menu, use the arrow keys to highlight the desired boot entry (usually the default Ubuntu entry), and then press `e` to edit. This will display the kernel boot parameters.
4.  **Locate Kernel Line:** Scroll down and find the line that starts with `linux` or `kernel`. This line contains the parameters passed to the kernel.

### Common Kernel Parameters

#### 1.1 `single` (Single User Mode)

Adding `single` to the kernel line boots the system into single-user mode, typically providing a root shell without requiring a password.

**Example:**
* Locate the `linux` or `kernel` line.
* Add `single` at the end of the line (e.g., `... ro quiet splash single`).
* Press `Ctrl+x` or `F10` to boot.

**Expected Behavior:**

After booting, you might see a prompt asking to press `Ctrl+d` to continue or `Enter` for maintenance.

* Press `Ctrl+d`.
* Then press `Enter` again.
* It might then ask for your root password (depending on the system configuration and whether `init=/bin/bash` was used).

**Demonstration Commands in Single User Mode (or root shell):**

* **`ls`**:
    ```
    # ls
    snap
    ```
    This shows a directory (or file) named `snap` in the current directory (which was root's home directory, indicated by `~#`).

* **`mount`**:
    ```
    # mount
    # (Output will vary, showing mounted filesystems)
    ```

* **`pwd` (Print Working Directory):**
    ```
    # pwd
    /root
    ```
    This indicates you are in the `/root` directory, which is the root user's home directory.

* **`ls -l` (List in Long Format):**
    ```
    # ls -l
    drwx------ 3 root root 4096 May 20 17:38 snap
    ```
    **Explanation of `ls -l` output:**
    * **`d`**: The first character indicates the file type. `d` means it's a directory.
    * **`rwx------`**: These are the file permissions.
        * `rwx`: The owner (`root`) has read, write, and execute permissions.
        * `---`: The group (`root`) has no permissions.
        * `---`: Others (anyone else) have no permissions.
    * **`3`**: This number represents the number of hard links to this directory.
    * **`root`**: The owner of the directory is the `root` user.
    * **`root`**: The group owner of the directory is the `root` group.
    * **`4096`**: The size of the directory in bytes. For directories, this often represents the size of the metadata within the directory, not the size of its contents.
    * **`May 20 17:38`**: The last modification date and time of the directory.
    * **`snap`**: The name of the directory. This directory is related to Snap packages, a software deployment and package management system for Linux.

#### 1.2 `cd /` (Change Root Directory)

This command moves you to the main, top-level directory of your entire Linux system.

* Type `cd /`
* Verify your current directory with `pwd`.
* Then, `ls -l` will show a list of core Linux directories.

#### 1.3 `cd` (Go Back to Home Directory)

Typing `cd` (without any arguments) will take you back to your current user's home directory (e.g., `/root` if you are the root user).

#### 1.4 Removing `quiet` and `splash`

* **Purpose:** These parameters suppress boot messages and show a graphical splash screen. Removing them allows you to see all kernel and system initialization messages during boot.
* **Action:** Restart your VM, open the GRUB menu, press `e`, and then delete `quiet` and `splash` from the kernel line.
* **Result:** You will see verbose boot messages instead of the splash screen.

#### 1.5 Adding `nomodeset`

* **Purpose:** The `nomodeset` option prevents the Linux kernel from loading advanced graphics drivers early in the boot process. This forces the system to use very basic, generic video modes.
* **When useful:** Primarily for troubleshooting display issues, black screens, or freezes during boot on systems with problematic graphics cards or drivers.
* **Action:** Add `nomodeset` to the kernel line.
* **Note:** This may not show a perfect visual difference in a VirtualBox VM as the display emulation is already quite basic.

#### 1.6 Demonstrating `ro` (Read-Only) vs. `rw` (Read-Write)

By default, the root filesystem often mounts as `ro` (read-only) during GRUB modifications to prevent accidental damage. However, the system later remounts it as `rw` (read-write) for normal operation.

* **Observation:** If you are in a temporary root shell (e.g., via `init=/bin/bash` before remounting), the filesystem might be read-only.
* **Test:** Try to create an empty file, e.g., `touch testfile.txt`.
* **Result:** The file might be created even if `ro` was temporarily set in GRUB, because the system will usually remount `rw` before giving you a full shell.

#### 1.7 Adding `emergency`

* **Purpose:** Boots the system into an emergency shell, bypassing most services and bringing you to a minimal root shell. This is often used for critical system recovery.
* **Action:** Add `emergency` to the kernel line.
* **Expected Output:** You will typically see a prompt similar to:
    ```
    Press Ctrl+D to continue, or type systemctl default or ^D for emergency mode.
    ```

#### 1.8 Adding `init=/bin/bash`

* **Purpose:** This parameter tells the kernel to execute `/bin/bash` as the very first process (PID 1) instead of the standard init system (like `systemd`). This provides a raw root shell with minimal services running.

**➤ BEFORE Adding `init=/bin/bash` (Normal Boot Process):**

`BIOS/UEFI` → `GRUB` → `Linux Kernel` → `/sbin/init` (like `systemd`) → `Start services (networking, login, GUI, etc.)`
* You get a full Linux system: login prompt or desktop, network, users, everything works.

**➤ AFTER Adding `init=/bin/bash` (Interrupted Boot Process):**

`BIOS/UEFI` → `GRUB` → `Linux Kernel` → `/bin/bash`
* **Result:** You only get:
    * A single `bash` shell.
    * Running as `root`.
    * No login screen.
    * No networking.
    * No background services.
    * No file system write access (initially, usually mounted `ro`).

**Demonstration:**

1.  **Check Read-Only Status:** Try to create a test file (`touch testfile.txt`). It will likely fail or report a read-only filesystem.
2.  **Remount Root Filesystem Read-Write:**
    * Type `mount -o remount,rw /`
    * Now, `touch testfile.txt` should succeed.
3.  **Run Normal Setup:**
    * Type `reboot -f` to force a reboot and return to a normal system startup.

#### 1.9 `noapic`, `nolapic`

* **Purpose:** These parameters disable specific Advanced Programmable Interrupt Controller (APIC) functionalities. APIC manages hardware interrupts.
* **When useful:** For troubleshooting system freezes or hangs, especially on older hardware or systems with specific interrupt-related issues.
* **Note:** You are unlikely to see a visual difference or impact in a VirtualBox VM as these are hardware-specific issues.

#### 1.10 `pci=nomsi`

* **Purpose:** Disables Message Signaled Interrupts (MSI) for PCI devices. MSI is a modern and efficient way for hardware devices to communicate interrupt requests to the CPU.
* **When useful:** For troubleshooting freezes, crashes, or instability related to PCI devices, especially when their MSI implementation is problematic.

#### 1.11 `fsck.mode=force`

* **Purpose:** Forces a filesystem check on the root filesystem during boot, even if it appears to have been shut down cleanly.
* **When useful:** Primarily for troubleshooting filesystem corruption or ensuring integrity after a crash or improper shutdown.

#### 1.12 `debug`

* **Purpose:** Tells the Linux kernel to output significantly more information, messages, and debugging data to the console during startup.
* **Prerequisite:** Requires removing `quiet` and `splash` from the kernel line.
* **When useful:** For detailed troubleshooting when the system is failing to boot and standard logging doesn't provide enough information.

#### 1.13 `pci=noaer`

* **Purpose:** Disables Advanced Error Reporting for PCI Express devices.
* **When useful:** For troubleshooting system freezes, crashes, or severe slowdowns that might be linked to faulty PCI Express devices constantly reporting errors.
* **Note:** Similar to `noapic`, you are very unlikely to see any visual or behavioral difference in your VirtualBox VM.

#### 1.14 `systemd.unit=rescue.target`

* **Purpose:** Boots the system into a minimal, yet functional, command-line environment with root privileges. It's similar to single-user mode but managed by `systemd`.
* **Most Common Use:** For resetting a forgotten root password or any user's password. You can gain a root shell without a password, remount the root filesystem read-write (`mount -o remount,rw /`), and then use the `passwd` command.
* **Note:** If the underlying filesystem is already read-only (as it might be in some recovery scenarios), you'll need to remount it as read-write.

#### 1.15 `acpi=off`

* **Purpose:** Disables Advanced Configuration and Power Interface (ACPI) support. ACPI manages power management, plug-and-play, and other hardware configurations.
* **When useful:** For troubleshooting boot failures or system instability on older hardware, or hardware with buggy ACPI implementations.
* **Note:** This can cause the system to get stuck or behave unexpectedly in a VM, as VMs rely on ACPI for proper virtual hardware interaction.

#### 1.16 `mem`

* **Purpose:** Limits the amount of RAM (memory) that the Linux kernel will recognize and use.
* **Example:** `mem=2G` would limit the kernel to using 2 Gigabytes of RAM.
* **When useful:** For testing memory-constrained environments or troubleshooting memory-related issues on systems with faulty RAM.

#### 1.17 `vga`

* **Purpose:** Specifies the text mode resolution for the console.
* **Example:** `vga=791` for 1024x768.
* **When useful:** For setting a specific console resolution when the default is not suitable, or for troubleshooting display issues in text mode.

#### 1.18 `maxcpus`

* **Purpose:** Limits the number of CPUs (or CPU cores) that the Linux kernel will use.
* **Example:** `maxcpus=1` would force the system to use only one CPU core.
* **When useful:** For troubleshooting multi-core related issues, testing single-core performance, or working around problems with specific CPU configurations.

---

## 2. Demonstrate knowledge of the boot sequence from BIOS/UEFI to boot completion 

Understanding the Linux boot process is crucial for effective troubleshooting.

### Linux Boot Process Stages

This table outlines the typical steps involved in the Linux boot process, along with relevant Ubuntu commands you can use to inspect different stages (where applicable).

| Step | Boot Process Stage          | Description                                                                     | Ubuntu Command to Inspect                                                              |
| :--- | :-------------------------- | :------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------- |
| 1    | POST (Power-On Self Test)   | Hardware check by BIOS/UEFI before OS is involved.                              | *Not accessible from Ubuntu (handled before OS loads)* |
| 2    | BIOS/UEFI Initializes H/W   | Initializes CPU, memory, storage controllers, etc.                              | *Not accessible from Ubuntu (handled before OS loads)* |
| 3    | Boot Device Selected        | BIOS/UEFI selects disk/partition to boot from.                                  | `[ -d /sys/firmware/efi ] && echo "UEFI"`                                              |
| 4    | MBR / Bootloader Executed   | Loads bootloader (like GRUB from MBR or EFI).                                   | `lsblk`, `sudo fdisk -l` (shows boot partitions), `sudo efibootmgr -v` (UEFI boot entries), `cat /boot/grub/grub.cfg` (GRUB config) |
| 5    | OS Kernel is Loaded         | Bootloader loads Linux kernel into memory.                                      | `dmesg` (for kernel messages once booted)                                              |
| 6    | `initrd`/`initramfs`        | Loads essential drivers/modules required before the root filesystem is mounted. | `lsinitramfs /boot/initrd.img-$(uname -r)` (see contents)                              |
| 7    | System Initialization       | `systemd`/`init` starts services, targets, devices, and brings up the system.   | `systemd-analyze` (boot time), `sudo systemctl status`                                 |

---

## 3. Understanding SysVinit and systemd

These are two common "init" systems in Linux, responsible for initializing the system after the kernel loads and managing services.

### 3.1 SysVinit (System V Init)

| Feature      | Description                                                 |
| :----------- | :---------------------------------------------------------- |
| **Age** | Very old (used in early Unix and early Linux distros).      |
| **Process** | Executes shell scripts (e.g., `/etc/init.d/`) one by one in a fixed order. |
| **Speed** | Slower boot (due to serial execution).                      |
| **Files** | Uses `/etc/inittab` to define runlevels and services.       |
| **Runlevels** | Uses runlevels (0–6) to define system states (e.g., `3` for multi-user, `5` for GUI). |
| **Example** | `service nginx start` (manually start a service).           |

### 3.2 systemd

| Feature      | Description                                                 |
| :----------- | :---------------------------------------------------------- |
| **Modern** | Default in most modern Linux distros (Ubuntu, Fedora, Debian, etc.). |
| **Speed** | Faster boot using parallelization.                          |
| **Smart** | Uses "units" instead of runlevels (e.g., `.service`, `.target`, etc.). |
| **Config Files** | Located in `/etc/systemd/`, `/lib/systemd/`, `/usr/lib/systemd/`. |
| **Tool** | Managed with the `systemctl` command.                       |
| **Example** | `sudo systemctl start nginx.service`.                       |

**Command to check which init system you are using:**
`ps -p 1 -o comm=`
**Systemd commands :**
o Start service: sudo systemctl start [service_name] 
o Stop service: sudo systemctl stop [service_name] 
o Enable on boot: sudo systemctl enable [service_name] 
o Check status: systemctl status [service_name] 
