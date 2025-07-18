# Linux Kernel Parameter  Management with sysctl (SOP)

## Author Information

| Author          | Created on | Version   | Last updated by | Internal Reviewer | L0     | L1      | L2     |
|-----------------|------------|-----------|------------------|--------------------|--------|---------|--------|
| Meenu Chauhan       | 17-07-25   | version 1 | 18-07-25              | Siddarth          | Imran | Shashi | Mahesh Kumar |

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Viewing Kernel Parameters](#viewing-kernel-parameters)
- [Temporarily Applying (Runtime) Changes](#temporarily-applying-runtime-changes)
- [Persisting Changes Across Reboots](#persisting-changes-across-reboots)
- [Performance Tuning Kernel Parameters](#performance-tuning-kernel-parameters)
- [Security Tuning Kernel Parameters](#security-tuning-kernel-parameters)
- [Validation](#validation)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)
- [Contact Information](#contact-information)
- [References](#references)


## Introduction

The sysctl command in Linux allows users to read and modify the kernel parameters at runtime. These parameters are accessible in the /proc/sys/ directory. sysctl provides a more convenient way to interact with these settings, without needing to directly edit files in the /proc/sys/ directory. This utility is crucial for system administrators and developers who need to tune the operating system for optimal performance and security.  


---
## Prerequisites

Before modifying kernel parameters, ensure the following prerequisites are met:

| Requirement                  | Description                                                                                 |
|-----------------------------|---------------------------------------------------------------------------------------------|
| **Root or Sudo Access**     | Required to view or modify kernel parameters and configuration files.                      |
| **Text Editor Installed**   | Ensure `vim`, `nano`, or similar is available to edit config files.                        |
| **Sysctl Installed**        | Linux system with `sysctl` utility installed (default in most distributions)         |

---


## Viewing Kernel Parameters

- **View all parameters and their values:**
  ```bash
  sysctl -a
  ```
- **View a specific parameter (example: IP forwarding):**
  ```bash
  sysctl net.ipv4.ip_forward
  ```
- **Display Parameter Names without Values:**
  ```bash
  sysctl -a -N
  ```

---

## Temporarily Applying (Runtime) Changes

- **Change a parameter for the current session (lost after reboot):**
  ```bash
  sudo sysctl -w parameter=value
  ```
  Example:
  ```bash
  sudo sysctl -w net.ipv4.ip_forward=1
  ```

- **Check your change:**
  ```bash
  sysctl net.ipv4.ip_forward
  ```

---

## Persisting Changes Across Reboots

Making changes permanent means they will be applied automatically every time the system starts.

- **Method 1: Edit `/etc/sysctl.conf`**
  1. Open the file:
     ```bash
     sudo vi /etc/sysctl.conf
     ```
  2. Add or update your parameter:
     ```
     net.ipv4.ip_forward=1
     ```
  3. Apply changes immediately:
     ```bash
     sudo sysctl -p /etc/sysctl.conf
     ```

- **Method 2: Create/Edit a file in `/etc/sysctl.d/` (Recommended)**
  1. Create a custom config file:
     ```bash
     vim /etc/sysctl.d/99-custom.conf
     ```
  2. Include the variables you wish to set and save the file
     ```bash
     net.ipv4.ip_forward=1
     ```
  3. Apply all sysctl settings:
     ```bash
     sudo sysctl --system
     ```

---
## Validation

- **Verify a specific parameter:**
  ```bash
  sysctl <parameter>
  ```
  Example:
  ```bash
  sysctl net.ipv4.ip_forward
  ```
- **Verify all current sysctl settings:**
  ```bash
  sysctl -a
  ```

---


## Performance Tuning Kernel Parameters

| Parameter                            | Description                                                                                       | Typical Example Value | Impact                                                                                         |
|---------------------------------------|---------------------------------------------------------------------------------------------------|----------------------|-----------------------------------------------------------------------------------------------------|
| **vm.swappiness**                     | Controls the tendency of the kernel to move processes out of physical memory and onto the swap disk| 10                   | Lower values reduce swap usage (good for most servers), higher values make the kernel swap more.     |
| **vm.dirty_ratio**                    | Maximum % of system memory that can be filled with dirty pages before writing to disk              | 15                   | Lower values cause more frequent disk writes.                                                                    
| **fs.file-max**                       | Maximum number of open file handles allowed system-wide                                            | 2097152              | Raise for high-connection servers (databases, web servers).                                          |

---

## Security Tuning Kernel Parameters

| Parameter | Description | Recommended Value |
|----------|-------------|-------------------|
| `net.ipv4.ip_forward` | **Disable IP forwarding** to prevent the host from acting as a router (good for non-networking servers) | `0` |
| `net.ipv4.tcp_syncookies` | Enable SYN cookies to protect against SYN flood DDoS attacks | `1` |



## Troubleshooting

- **Backup Configurations:**  
  Before making permanent changes, backup configuration files:
  ```bash
  sudo cp /etc/sysctl.conf /etc/sysctl.conf.bak
  ```
- **Quick Reversion:**  
  If an issue arises, quickly restore the backup and reload:
  ```bash
  sudo cp /etc/sysctl.conf.bak /etc/sysctl.conf
  sudo sysctl -p
  ```
- **Persistence Troubleshooting:**  
  If a parameter value does not persist after reboot:
  1. **Check File Syntax:**  
     Ensure no syntax errors or typos in `/etc/sysctl.conf` or `/etc/sysctl.d/*.conf`.
  2. **File Extensions:**  
     Custom files in `/etc/sysctl.d/` must end with `.conf`.
  3. **Check for Conflicts:**  
     Identify duplicate/conflicting parameters:
     ```bash
     sudo grep -r net.ipv4.ip_forward /etc/sysctl*
     ```
---

## Best Practices

- **Test Changes Temporarily:**  
  Always use temporary changes (`sysctl -w`) to validate effects before making them persistent.
- **Implement Incremental Changes:**  
  Modify and test one parameter at a time; monitor impact before further changes.

---


## Contact Information

| Name            | Email address |
|-----------------|---------------|
| Meenu Chauhan       | meenu.chauhan.snaatak@mygurukulam.co

## References

- Phoenixnap Document: https://phoenixnap.com/kb/sysctl
- Redhat Official Document: https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/kernel_administration_guide/working_with_sysctl_and_kernel_tunables

---
