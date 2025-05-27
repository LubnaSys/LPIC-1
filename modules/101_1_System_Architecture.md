
# TOPIC 101: SYSTEM ARCHITECTURE

## System Architecture

System architecture refers to the fundamental design and layout of a computer system's components, focusing on how hardware and software interact to perform computing tasks efficiently. It includes the configuration and management of essential hardware elements such as CPUs, memory, mass storage devices, input/output interfaces, and peripheral devices. This structure also involves understanding system resources like I/O ports, IRQs, and DMA channels, as well as using tools like `lspci`, `lsusb`, and `modprobe` to inspect and configure hardware. Additionally, core Linux frameworks such as `sysfs`, `udev`, and `dbus` play a critical role in device management and communication within the system.

# 101.1 Determine and Configure Hardware Settings

This section covers how to inspect, configure, and manage various hardware settings in a Linux system, an essential skill for system administrators preparing for the LPIC-1 certification.

---

## Enable and Disable Integrated Peripherals

The lsmod command is used to display all active kernel modules, which includes drivers for integrated peripherals. Here's a breakdown of some common modules and what they control:
| Module Name     | Device Type                      | Description                                                                    |
| :-------------- | :------------------------------- | :----------------------------------------------------------------------------- |
| `usbhid`        | USB Human Interface Device       | Handles input devices like USB keyboards and mice.                             |
| `hid_generic`   | Generic HID driver               | Supports generic HID-compliant devices.                                        |
| `hid`           | HID Core                         | Base module for HID device communication.                                      |
| `psmouse`       | PS/2 Mouse                       | Supports standard PS/2 mouse (often built-in or USB-PS/2 adapter).             |
| `i2c_piix4`     | I²C Controller                   | Integrated controller for low-speed communication (e.g., sensors, SMBus).      |
| `snd_intel8x0`  | Audio Controller                 | Integrated Intel AC’97 audio controller.                                       |
| `snd_ac97_codec`| Audio Codec                      | Audio codec driver used with AC'97-compatible hardware.                        |
| `e1000`         | Network Interface Card (NIC)     | Intel Pro/1000 integrated Ethernet controller.                                 |
| `ahci`          | SATA Controller                  | Advanced Host Controller Interface for integrated SATA storage devices.        |
| `libahci`       | AHCI Support Library             | Dependency for AHCI support.                                                   |
| `lp` / `parport`| Printer Port (Parallel Port)     | Legacy printer port module (often integrated).                                 |

🔧 How to Enable/Disable Peripherals
Below are examples of how to disable and enable various integrated peripheral modules using Linux commands:

**1. Line Printer (Parallel Port)**

# Disable
sudo rmmod lp

# Verify it's disabled
lsmod | grep lp

# Enable
sudo modprobe lp

# Verify it's enabled
lsmod | grep lp

**2. Parallel Port Device**

# Disable
sudo rmmod parport

# Verify it's disabled
lsmod | grep parport

# Enable
sudo modprobe parport

# Verify it's enabled
lsmod | grep parport

**3. PC Parallel Port Driver**

# Disable
sudo rmmod parport_pc

# Verify it's disabled
lsmod | grep parport_pc

# Enable
sudo modprobe parport_pc

# Verify it's enabled
lsmod | grep parport_pc

**4. HDMI Audio Output (if module not loaded by default)**

# Enable
sudo modprobe snd_hda_codec_hdmi

# Verify it's enabled
lsmod | grep snd_hda_codec_hdmi

# Disable
sudo rmmod snd_hda_codec_hdmi

# Verify it's disabled
lsmod | grep snd_hda_codec_hdmi

**5. Bluetooth USB Driver (if not loaded by default)**

# Enable
sudo modprobe btusb

# Verify it's enabled
lsmod | grep btusb

# Disable
sudo rmmod btusb

# Verify it's disabled
lsmod | grep btusb

---

## Differentiate Between the Various Types of Mass Storage Devices

## What Are Mass Storage Devices?  
Mass storage devices are used to store large amounts of data for a long time. They hold your  
files, software, OS, videos, photos, etc.  
Examples are HDD, SSD.


### Show mass storage devices in CLI  :
`lsblk` 
- **SDA:** It shows primary HDD and SSD.  
- **sr0:** It shows CD / DVD-ROM drive.

### Check Disk Type (HDD or SSD)  :  
`cat /sys/block/sd*/queue/rotational`
- Returns `1` = HDD (spinning)  
- Returns `0` = SSD (solid state)

### Check Mounted Drives and File Systems: 
`df -h`


### List USB Storage Devices:
`lsusb`
******

# 3. Determine hardware resources for devices.

Hardware resources are the physical components of a computer system that provide computing  
power, storage, and connectivity.

| Resource        | Description                                            |
|-----------------|--------------------------------------------------------|
| IRQ (Interrupt Request) | Helps devices communicate with CPU               |
| I/O Ports       | Channels through which CPU communicates with devices  |
| DMA Channels    | Allow devices to transfer data without CPU             |
| Memory Addresses| Reserved sections of RAM used by devices               |
| CPU/Memory Usage| How much processing or memory a device uses            |

### See Memory Addresses Used by Devices:
`cat /proc/iomen`

---

## Determine Hardware Resources for Devices

1. **CPU:**  
   - `lscpu`

2. **Memory (RAM):**  
   - `free -h` : Displays total amount of free space.

3. **Disk:**  
   - `lsblk` : Check all disks

4. **GPU:**  
   - `lspci | grep -i vga` : Check GPU details.

5. **Network:**  
   - `ip a` : List Network Interfaces

6. **USB Devices:**  
   - `lsusb`

7. **Kernel & System:**  
   - `uname -a` : Check Linux Kernel Version

8. **Hardware Summary:**  
   - `lshw`

9. **PCI devices:**  
   - `lspci`

10. **Check SATA/ SCSI Device:**  
    ➢ SCSI stands for Small Computer System Interface. It's a set of standards used to connect  
    and transfer data between computers and peripheral devices, like:  
    - Hard drives  
    - CD/DVD drives  
    - Scanners  
    - Tape drives  
    - SSDs  

    ➢ SATA (Serial ATA) stands for Serial Advanced Technology Attachment.  
    It’s a modern interface used to connect storage devices like:  
    - Hard disk drives (HDDs)  
    - Solid-state drives (SSDs)  
    - Optical drives (CD/DVD)  

    - `lsscsi`

---

## Tools and Utilities to List Various Hardware Information (e.g., lsusb, lspci)

Gain practical knowledge of tools such as:
- `lsusb` for USB devices
- `lspci` for PCI devices
- `dmidecode` for BIOS and motherboard info
- `hwinfo` and `lshw` for complete hardware overviews

---

## Tools and Utilities to list various hardware info

1. **lshw – List Hardware**  
   Displays detailed hardware information (CPU, RAM, disks, USB, PCI, etc.).  
   - `sudo lshw` : Shows full hardware details (requires root).  
   - `sudo lshw -short` : Displays a brief summary.  
   - `sudo lshw -class CPU` : Filters output for CPU-related info.  
   - `sudo lshw -class disk -class storage` : Shows disk and storage controllers.  
   - `sudo lshw -html > hardware.html` : Exports hardware info to an HTML file.  
   
   *It didn’t work to see it type command*  
   *It will open like this*  

2. **lscpu – CPU Information**  
   Displays CPU architecture detail, showing CPU details (cores, threads, sockets).  
   - `lscpu -e` : Lists CPU cores in a readable format.  
   - `lscpu -p` : Displays a human-readable summary of your CPU.  

3. **lsblk – List Block Devices**  
   Shows storage devices (disks, partitions, mounts). Lists all block devices (disks, partitions).  
   - `lsblk -f` : Includes filesystem info (ext4, NTFS, etc.).  

4. **free – CPU Information Memory (RAM) Usage**  
   Displays RAM and swap usage.  
   - `free -h` : Shows RAM in human-readable format (GB/MB).  
   - `free -m` : Displays memory in megabytes.  
   - `free -t` : Includes a total line for RAM + swap.  

5. **df – Disk Space Usage**  
   Shows filesystem disk space usage.  
   - `df -h` : Human-readable disk space (GB/MB).  
   - `df -Th` : Includes filesystem info (ext4, NTFS, etc.).  
   - `df -i` : Shows inode usage instead of disk space.  

6. **lsusb – USB Devices**  
   Lists USB controllers and connected devices.  
   - `lsusb -v` : Shows detailed USB device info.  
   - `lsusb -t` : USB device tree hierarchy.  

7. **lspci – PCI Devices**  
   Lists PCI devices (GPUs, network cards, etc.).  
   - `lspci -v` : Shows detailed PCI device info.  
   - `lspci -nn` : Displays vendor and device IDs.  
   - `lspci -k` : Shows kernel drivers in use.  

8. **dmidecode – BIOS & Hardware Detail**  
   Retrieves BIOS, motherboard, and hardware info (requires root).  
   - `sudo dmidecode` : Shows all DMI (BIOS) hardware info.  
   - `sudo dmidecode -t memory` : Lists RAM slots and details.  
   - `sudo dmidecode -t system` : Displays system manufacturer/model.  

9. **hdparm – Disk Performance (for HDD/SSD)**  
   Tests disk speed and retrieves storage info.  
   - `sudo hdparm -I /dev/sda` : Shows detailed disk info.  
   - `sudo hdparm -Tt /dev/sda` : Benchmarks disk read speed.

---

## Tools and utilities to manipulate USB devices.  

1. List Connected USB  
• Basic Listing: `lsusb`  

• Detailed View: `lsusb -v`  

2. Identify USB Device Details  
• Device Hierarchy: `usb-devices`  

• Detailed View: `lsusb -v`  
• Kernel messages: `dmesg | grep usb`  

3. Check Mounted USB Partitions  
`mount | grep -i sd`  

4. Check USB Controller Info  
`lspci | grep -i usb`  

➢ Some other commands  
• Unmount USB  
`bash`
• Format USB
`sudo mkfs.vfat /dev/<usb-name>`
• Monitor USB traffic
`usbtop`
• Reset USB port
`echo '1-1' > /sys/bus/usb/...`
• Disable USB storage
`sudo modprobe -r usb_storage`
• Create bootable USB
`sudo dd if=file.iso of=/dev/<usb-name>`


---

## Conceptual Understanding of sysfs, udev, and dbus

**I. sysfs:**  
sysfs is a virtual filesystem in Linux, created by the kernel during the boot process and typically mounted at /sys. It provides a structured way for user space to interact with kernel objects like devices, drivers, and modules. Unlike normal filesystems, sysfs doesn’t store data permanently—it lives in RAM and disappears when the system shuts down. It organizes kernel information into a directory-like structure, allowing users and programs to view and even modify system settings in real-time.

**II. udev:**  
➢ udev (short for userspace /dev) is the device manager for the Linux kernel.  
➢ udev automatically creates, names, and manages the files in the /dev/ directory whenever hardware is added, removed, or changed — like plugging in a USB, Bluetooth adapter, or hard drive.  
➢ It is launched after .init process, when the system has enough features to fully manage devices.

**III. dbus:**  
D-Bus is like a messenger system for Linux that lets apps and services talk to each other.  
**What It Does**  
• Allows programs to send/receive messages (e.g., a music app telling the system to lower volume).  
• Used for hardware events (e.g., plugging in a USB stick triggers a notification).  
• Helps the system tray, notifications, and login managers work smoothly.  

➢ Check if D-bus is running: “systemctl status dbus”

**The used files, terms and utilities:**  
• /sys/ : Directory that holds device files used to communicate with hardware (e.g., USB, disks).  
• /proc/ : /proc/ is a virtual filesystem in Linux that provides real-time system and process information as readable files. /proc is a virtual filesystem that provides a real-time, structured view into the Linux kernel's current state and running processes.  
➢ cat /proc/cpuinfo: Shows CPU details (cores, speed).  
➢ cat /proc/meminfo: Displays RAM usage (free, cached).  
➢ ls /proc/[PID]/: Lists files for a running process.  
➢ cat /proc/loadavg: Checks system load (1/5/15 min avg).  
• /dev/ : Directory that holds device files used to communicate with hardware (e.g., USB, disks).  
• modprobe: Loads or removes kernel modules in Linux.  
• lsmod: List all kernel modules in Linux.  
• lspci: List all peripheral component interconnected.  
• lsusb: Lists all USB devices connected to your Linux system.
