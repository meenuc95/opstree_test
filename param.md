# Common Linux Kernel Parameters for Security and Performance Tuning (Detailed)

This guide details the most widely used Linux kernel parameters for tuning system performance and security, with explanations of what they do, why they're important, and typical tuning considerations.

---

## Performance Tuning Parameters

### 1. `vm.swappiness`
- **What it does:**  
  Controls how aggressively the kernel moves processes out of physical RAM and onto swap space (disk).
- **Typical Value:**  
  `10` (range: 0-100)
- **Why tune it:**  
  A lower value (e.g., 10) means the kernel avoids swapping unless absolutely necessary, keeping more processes in RAM for faster performance—ideal for most servers with sufficient memory. A higher value increases swap usage, which can help avoid out-of-memory (OOM) situations but at the cost of slower disk access.
- **Impact:**  
  - Low value: Faster performance, less swapping, reduced disk I/O.
  - High value: More swapping, can prevent OOM errors but may slow down the system.

---

### 2. `vm.dirty_ratio`
- **What it does:**  
  Sets the maximum percentage of system RAM that can contain "dirty" (modified but not yet written) pages before the kernel forces them to be flushed to disk.
- **Typical Value:**  
  `15`
- **Why tune it:**  
  Lower values cause more frequent, smaller disk writes, improving data safety (especially on sudden power loss). Higher values allow more dirty data to accumulate, which can improve burst performance but risks larger data loss if the system crashes.
- **Impact:**  
  - Low value: Frequent, smaller writes; more I/O but safer.
  - High value: Less frequent, larger writes; better for bursty workloads but riskier for data integrity.

---

### 3. `fs.file-max`
- **What it does:**  
  Maximum number of open file handles (files, sockets, pipes, etc.) allowed system-wide.
- **Typical Value:**  
  `2097152`
- **Why tune it:**  
  High-traffic servers (web servers, databases) can hit the default limit, causing "Too many open files" errors. Increasing this value ensures that applications can handle more concurrent connections/files.
- **Impact:**  
  - Higher value: Supports more concurrent users/connections.
  - Too low: Limits scalability, can cause service failures under load.

---

### 4. `net.core.somaxconn`
- **What it does:**  
  Sets the maximum number of connections that can be queued for acceptance by a listening socket.
- **Typical Value:**  
  `1024` or higher
- **Why tune it:**  
  Important for web servers and proxies to handle spikes in incoming connections. Low values can cause connection drops under heavy load.
- **Impact:**  
  - Higher value: Handles more simultaneous incoming connections.
  - Too low: Risk of dropped connections during traffic spikes.

---

### 5. `net.ipv4.tcp_max_syn_backlog`
- **What it does:**  
  Maximum number of queued SYN requests (half-open connections) waiting for completion of the TCP handshake.
- **Typical Value:**  
  `2048` or higher on busy servers
- **Why tune it:**  
  Helps web servers and load balancers handle more concurrent connection attempts, reducing risk of SYN flood-related DoS.
- **Impact:**  
  - Higher value: Protects against SYN floods and connection drops.
  - Too low: May reject legitimate connections under high load.

---

## Security Tuning Parameters

### 1. `net.ipv4.ip_forward`
- **What it does:**  
  Controls whether the host forwards IP packets between network interfaces (i.e., acts as a router).
- **Recommended Value:**  
  `0` (disabled)
- **Why tune it:**  
  Most servers do not need to act as routers. Disabling IP forwarding reduces the network attack surface, preventing the server from being used in routing attacks.
- **Impact:**  
  - Disabled (0): Increased security.
  - Enabled (1): Only enable for gateway/router roles.

---

### 2. `net.ipv4.tcp_syncookies`
- **What it does:**  
  Enables SYN cookies, a protection against TCP SYN flood attacks (a common form of DDoS).
- **Recommended Value:**  
  `1` (enabled)
- **Why tune it:**  
  SYN cookies allow the server to handle many half-open connections during SYN floods without exhausting resources.
- **Impact:**  
  - Enabled (1): Increased resilience to SYN flood attacks.
  - Disabled (0): Vulnerable to SYN flood DoS.

---

### 3. `fs.suid_dumpable`
- **What it does:**  
  Controls whether set-user-ID (setuid) or set-group-ID (setgid) programs can produce core dumps if they crash.
- **Recommended Value:**  
  `0` (disabled)
- **Why tune it:**  
  Core dumps can contain sensitive information. Disabling prevents leaking credentials or secrets through core files.
- **Impact:**  
  - Disabled (0): Increased confidentiality.
  - Enabled (1/2): Only for debugging in safe, controlled environments.

---

### 4. `net.ipv4.conf.all.accept_redirects`
- **What it does:**  
  Controls whether the server accepts ICMP redirect messages, which can alter routing tables.
- **Recommended Value:**  
  `0` (disabled)
- **Why tune it:**  
  Accepting redirects can be exploited for man-in-the-middle or spoofing attacks. Disabling improves security.
- **Impact:**  
  - Disabled (0): Better defense against spoofing.
  - Enabled (1): Only if explicitly needed for network design.

---

### 5. `kernel.randomize_va_space`
- **What it does:**  
  Enables Address Space Layout Randomization (ASLR) for security.
- **Recommended Value:**  
  `2` (full randomization)
- **Why tune it:**  
  ASLR makes it harder for attackers to predict process memory layout, mitigating many types of exploits.
- **Impact:**  
  - Enabled (2): Increased system security.
  - Disabled (0): For debugging only.

---

## Summary Table

| Parameter                               | Purpose                                 | Typical/Recommended Value | Tuning Effect                         |
|------------------------------------------|-----------------------------------------|--------------------------|---------------------------------------|
| `vm.swappiness`                          | Memory performance                      | 10                       | Less swapping, better performance     |
| `vm.dirty_ratio`                         | Write performance/data safety           | 10-20                    | Frequent, safer writes                |
| `fs.file-max`                            | System scalability                      | 2097152                  | More open files/connections allowed   |
| `net.core.somaxconn`                     | Connection handling                     | 1024 or higher           | Fewer dropped connections             |
| `net.ipv4.tcp_max_syn_backlog`           | SYN attack protection/performance       | 2048 or higher           | Resilience under heavy SYN load       |
| `net.ipv4.ip_forward`                    | Routing security                        | 0                        | Disables unnecessary routing          |
| `net.ipv4.tcp_syncookies`                | SYN flood protection                    | 1                        | Prevents SYN flood DoS                |
| `fs.suid_dumpable`                       | Confidentiality                         | 0                        | Prevents sensitive data leaks         |
| `net.ipv4.conf.all.accept_redirects`     | Spoofing defense                        | 0                        | Prevents route spoofing               |
| `kernel.randomize_va_space`              | Exploit mitigation                      | 2                        | Enables ASLR for process memory       |

---

**Best Practice:**  
Always test parameter changes in a staging environment before deploying to production. Monitor system logs and performance after each change, and document all modifications for future reference.

---
