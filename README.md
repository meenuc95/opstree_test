# SOP: Viewing, Applying, and Persisting Kernel Parameter Changes using sysctl
## Objective

This SOP explains how to view, apply, and persist Linux kernel parameter (kernel tunable) changes for performance or security tuning.  
Linux kernel parameters are settings that control how the core of your operating system (the kernel) behaves. These parameters are located in the `/proc/sys/` directory. 
The sysctl command in Linux allows users to read and modify the kernel parameters at runtime. These parameters are accessible in the /proc/sys/ directory. sysctl provides a more convenient way to interact with these settings, without needing to directly edit files in the /proc/sys/ directory. This utility is crucial for system administrators and developers who need to tune the operating system for optimal performance and security.

---

## Ways to Modify Kernel Tunables

There are three common ways to change Linux kernel parameters:

1. **Using the sysctl command**  
   - Safely view and change kernel parameters on the fly.
2. **By editing configuration files in the `/etc/sysctl.d/` directory**  
   - Allows you to save changes so they are applied automatically at every boot.
3. **Directly interacting with the virtual file system at `/proc/sys`**  
   - Advanced method; changes are immediate but not persistent after reboot.

---

## 1. Viewing Kernel Parameters

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

## 2. Temporarily Applying (Runtime) Changes

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

## 3. Persisting Changes Across Reboots

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
## 4. Performance Tuning Kernel Parameters

| Parameter                            | Description                                                                                       | Typical Example Value | Impact                                                                                         |
|---------------------------------------|---------------------------------------------------------------------------------------------------|----------------------|-----------------------------------------------------------------------------------------------------|
| **vm.swappiness**                     | Controls the tendency of the kernel to move processes out of physical memory and onto the swap disk| 10                   | Lower values reduce swap usage (good for most servers), higher values make the kernel swap more.     |
| **vm.dirty_ratio**                    | Maximum % of system memory that can be filled with dirty pages before writing to disk              | 15                   | Lower values cause more frequent disk writes.                                                                    
| **fs.file-max**                       | Maximum number of open file handles allowed system-wide                                            | 2097152              | Raise for high-connection servers (databases, web servers).                                          |
| **net.core.somaxconn**                | Maximum number of connections that can be queued for acceptance by a listening socket              | 1024                 | Important for high-load network servers.                                                             |

---

## Security Tuning Kernel Parameters

| Parameter | Description | Recommended Value |
|----------|-------------|-------------------|
| `net.ipv4.ip_forward` | **Disable IP forwarding** to prevent the host from acting as a router (good for non-networking servers) | `0` |
| `net.ipv4.tcp_syncookies` | Enable SYN cookies to protect against SYN flood DDoS attacks | `1` |
| `net.ipv4.conf.all.accept_redirects`   | Disable ICMP redirect messages to avoid spoofing attacks.                            | `0`|
| `fs.suid_dumpable`                     | Prevents setuid program crashes from dumping memory (for confidentiality).           | `0`|



## 5. Best Practices

- Always test changes temporarily before making them permanent.
- Use `/etc/sysctl.d/` for organized and safe permanent changes.
- Document all changes and why you made them.
- Be careful: Some kernel parameters can affect system security and stability.

---

## References

- [man sysctl](https://man7.org/linux/man-pages/man8/sysctl.8.html)
- [Linux Kernel Documentation: sysctl](https://www.kernel.org/doc/html/latest/admin-guide/sysctl/index.html)

---
