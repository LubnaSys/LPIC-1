## System Architecture

System architecture refers to the fundamental design and layout of a computer system's components, focusing on how hardware and software interact to perform computing tasks efficiently. It includes the configuration and management of essential hardware elements such as CPUs, memory, mass storage devices, input/output interfaces, and peripheral devices. This structure also involves understanding system resources like I/O ports, IRQs, and DMA channels, as well as using tools like `lspci`, `lsusb`, and `modprobe` to inspect and configure hardware. Additionally, core Linux frameworks such as `sysfs`, `udev`, and `dbus` play a critical role in device management and communication within the system.

# TOPIC 101: SYSTEM ARCHITECTURE

## System Architecture

System architecture refers to the fundamental design and layout of a computer system's components, focusing on how hardware and software interact to perform computing tasks efficiently. It includes the configuration and management of essential hardware elements such as CPUs, memory, mass storage devices, input/output interfaces, and peripheral devices. This structure also involves understanding system resources like I/O ports, IRQs, and DMA channels, as well as using tools like `lspci`, `lsusb`, and `modprobe` to inspect and configure hardware. Additionally, core Linux frameworks such as `sysfs`, `udev`, and `dbus` play a critical role in device management and communication within the system.

---

## 101.1 Determine and configure hardware settings

This section details the steps for identifying and configuring hardware components.

### 101.1 Lesson 1

This lesson covers the initial steps in hardware setup and inspection.

#### Introduction

This section provides an overview of what you will learn in this lesson.

#### Device Activation

Steps required to activate a device for use with the system.

#### Device Inspection in Linux

How to inspect device properties and status using Linux commands.
Example: `lsusb` command usage.
```bash
root@ubuntu16-1:~# lsusb
Bus 001 Device 002: ID 0951:1625 Kingston Technology DataTraveler 101 II
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 003: ID 0e0f:0002 VMware, Inc. Virtual USB Hub
Bus 002 Device 002: ID 0e0f:0003 VMware, Inc. Virtual Mouse
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
