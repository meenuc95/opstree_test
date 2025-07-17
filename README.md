# SOP: Viewing, Applying, and Persisting Kernel Parameter Changes using sysctl
## Objective

This SOP explains how to view, apply, and persist Linux kernel parameter (kernel tunable) changes for performance or security tuning using the sysctl command.  


---
## Prerequisites

Before modifying kernel parameters, ensure the following prerequisites are met:

| Requirement                  | Description                                                                                 |
|-----------------------------|---------------------------------------------------------------------------------------------|
| **Root or Sudo Access**     | Required to view or modify kernel parameters and configuration files.                      |
| **Text Editor Installed**   | Ensure `vim`, `nano`, or similar is available to edit config files.                        |
| **Sysctl Installed**        | Linux system with `sysctl` utility installed (default in most distributions)         |

---

## 1. Ways to Modify Kernel Tunables

There are three common ways to change Linux kernel parameters:

1. **Using the sysctl command**  
   - This method allows you to instantly view and change kernel parameters at runtime, impacting the current session only.
2. **By editing configuration files in the `/etc/sysctl.d/` directory**  
   - This method involves creating or editing .conf files within the /etc/sysctl.d/ directory. These files are loaded automatically at boot, ensuring your changes persist across reboots.
3. **Directly interacting with the virtual file system at `/proc/sys`**  
   - Advanced method; changes are immediate but not persistent after reboot.

---

## 2. Viewing Kernel Parameters

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

## 3. Temporarily Applying (Runtime) Changes

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

## 4. Persisting Changes Across Reboots

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
## 5.Validation

1. To verify if the kernel parameters have been set correctly:

 ```bash
sysctl <parameter>
```
Example:

 ```bash
sysctl net.ipv4.ip_forward
```
2. To verify all current sysctl settings:

```bash
sysctl -a
```

## 6. Performance Tuning Kernel Parameters

| Parameter                            | Description                                                                                       | Typical Example Value | Impact                                                                                         |
|---------------------------------------|---------------------------------------------------------------------------------------------------|----------------------|-----------------------------------------------------------------------------------------------------|
| **vm.swappiness**                     | Controls the tendency of the kernel to move processes out of physical memory and onto the swap disk| 10                   | Lower values reduce swap usage (good for most servers), higher values make the kernel swap more.     |
| **vm.dirty_ratio**                    | Maximum % of system memory that can be filled with dirty pages before writing to disk              | 15                   | Lower values cause more frequent disk writes.                                                                    
| **fs.file-max**                       | Maximum number of open file handles allowed system-wide                                            | 2097152              | Raise for high-connection servers (databases, web servers).                                          |
| **net.core.somaxconn**                | Maximum number of connections that can be queued for acceptance by a listening socket              | 1024                 | Important for high-load network servers.                                                             |

---

## 7. Security Tuning Kernel Parameters

| Parameter | Description | Recommended Value |
|----------|-------------|-------------------|
| `net.ipv4.ip_forward` | **Disable IP forwarding** to prevent the host from acting as a router (good for non-networking servers) | `0` |
| `net.ipv4.tcp_syncookies` | Enable SYN cookies to protect against SYN flood DDoS attacks | `1` |
| `net.ipv4.conf.all.accept_redirects`   | Disable ICMP redirect messages to avoid spoofing attacks.                            | `0`|
| `fs.suid_dumpable`                     | Prevents setuid program crashes from dumping memory (for confidentiality).           | `0`|



## 8. Troubleshooting and best practices

- Always test changes temporarily before making them permanent.
- Before making permanent changes, create backups of the configuration files (e.g., cp /etc/sysctl.conf /etc/sysctl.conf.bak).
- If a setting causes issues, quickly revert to the backup: cp /etc/sysctl.conf.bak /etc/sysctl.conf && sudo sysctl -p.
- Avoid applying many changes at once. Adjust one parameter at a time and monitor its impact before proceeding to the next.
- Be careful: Some kernel parameters can affect system security and stability.
- If your value doesn’t persist:
   1. Check file syntax: Ensure no extra spaces, typos in /etc/sysctl.conf or /etc/sysctl.d/*.conf

   2. Ensure the file has .conf extension in /etc/sysctl.d/

   3. Check for overrides in multiple sysctl files using:
     
     ```bash
      sudo grep -r net.ipv4.ip_forward /etc/sysctl*
     ```



---

## References

- [man sysctl](https://man7.org/linux/man-pages/man8/sysctl.8.html)
- [Linux Kernel Documentation: sysctl](https://www.kernel.org/doc/html/latest/admin-guide/sysctl/index.html)

---
