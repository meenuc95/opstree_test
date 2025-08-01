# Redis Documentation

| Author | Created on | Version | Last updated by | Last edited on |
|--------|------------|---------|-----------------|----------------|
| Meenu Chauhan | 15-01-2025 | Version 1.0 | Meenu Chauhan | 15-01-2025 |

## Introduction

This document provides comprehensive guide to Redis — a fast, open-source, in-memory data store used as a database, cache, and message broker. Redis supports a variety of data types like strings, hashes, lists, and sets, which makes it flexible for different kinds of applications. 
It explains what Redis is, its main features, how to install and configure it, and how to use it.  By the end, you'll have a solid understanding of why Redis is well-suited for modern applications

## Purposes

Redis is built to handle situations where speed really matters. It stores data in memory, so it can quickly read and write 
information without waiting on disk access. This makes it especially useful for things like caching frequently-used data,
keeping track of user sessions, handling real-time messages. In short, Redis helps make your application faster and more responsive 
by keeping important data close at hand.

## Key Features

- **In-Memory Storage**: Ultra-fast data access with sub-millisecond response times
- **Data Structures**: Support for strings, hashes, lists, sets, sorted sets, and more
- **Persistence**: Optional disk persistence with RDB snapshots and AOF logging
- **Replication**: Master-slave replication for high availability
- **Clustering**: Automatic sharding across multiple nodes
- **Pub/Sub**: Real-time messaging capabilities
- **Expiration**: Automatic key expiration with TTL support
- **Security**: Authentication and SSL/TLS encryption


### Software Overview

| Software | Version |
|----------|---------|
| Redis | 7.2.0 |

## System Requirement

| Requirement           | Minimum Recommendation     |
|------------------------|----------------------------|
| Processor/Instance Type | Dual-Core / t2.medium instance |
| RAM                    | 4 GB or Higher              |
| ROM (Disk Space)       | 10 GB or Higher             |
| OS Required            | Linux (Ubuntu 20.04+)       |

### Important Ports

| Ports | Description |
|-------|-------------|
| 6379 | Default Redis server port for client connections |
| 22 | SSH port for server administration |

### Dependencies


#### Other Dependency

| Other Dependency | Version | Description |
|------------------|---------|-------------|
| Systemd | 230+ | Service management (optional) |
| Logrotate | 3.8+ | Log rotation (optional) |

## How to Setup/Install Redis

### Step-by-step Installation Instruction

#### Package Manager Installation (Ubuntu/Debian)

```bash
# Update package list
sudo apt update

# Install Redis
sudo apt install redis-server

# Start Redis service
sudo systemctl start redis-server

# Enable Redis to start on boot
sudo systemctl enable redis-server
```

### Run command to start redis

```bash
# Start Redis server
redis-server

# Start Redis with configuration file
redis-server /etc/redis/redis.conf

# Start Redis as a service
sudo systemctl start redis-server

# Check Redis status
sudo systemctl status redis-server
```

## Configuration

### Basic Configuration

To configure Redis properly, edit the redis default configuration  file usually located at /etc/redis/redis.conf

Edit `/etc/redis/redis.conf`:

```bash
# Network configuration
bind 127.0.0.1               # Restrict Redis to localhost. Use 0.0.0.0 for external access (not recommended without firewall).
port 6379                    # Default Redis port.

# Memory configuration
maxmemory 256mb              # Maximum memory Redis can use.

# RDB Persistence (Snapshotting)
save 900 1                   # Save DB if 1 key changes in 900 seconds.
save 300 10                  # Save DB if 10 keys change in 300 seconds.
save 60 10000                # Save DB if 10000 keys change in 60 seconds.

# Security
requirepass your_strong_password # Set a password to protect Redis access.

# Logging
loglevel notice              # Log level: debug, verbose, notice, warning.
logfile /var/log/redis/redis-server.log # Log file location.

```

### Advanced Configuration

```bash
# Enable AOF (Append Only File) persistence
appendonly yes               # Enable AOF logging for durability.
appendfsync everysec         # Sync AOF to disk every second (good performance-durability tradeoff).

# Replication - Make this instance a replica of another Redis server
replicaof <master-ip> <master-port> # Replace with actual IP and port of the master node.

# Clustering - Enable Redis Cluster for distributed setup
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000    # Timeout for detecting node failures (in milliseconds).

```

## Maintenance

### Upgrade Redis Version on Ubuntu

```bash
# Stop Redis service
sudo systemctl stop redis-server

# Backup data
sudo cp -r /var/lib/redis /var/lib/redis.backup

# Install new version
sudo apt update && sudo apt install redis-server

# Start Redis service
sudo systemctl start redis-server

# Verify version
redis-server --version
```

### Restart Commands

```bash
# Restart Redis service
sudo systemctl restart redis-server

# Restart with configuration changes
sudo systemctl reload redis-server

# Force restart
sudo systemctl stop redis-server && sudo systemctl start redis-server
```

## Monitoring

### Check Service Status

```bash
# Check if Redis is running
sudo systemctl status redis-server

# Check Redis process
ps aux | grep redis

# Check Redis port
netstat -tlnp | grep 6379
```

### Redis CLI Commands

| Command               | Description                                              |
| --------------------- | -------------------------------------------------------- |
| redis-cli             | Opens an interactive Redis command-line interface.       |
| redis-cli ping        | Tests if Redis is running and reachable. Returns PONG.   |
| redis-cli info        | Displays detailed information about the Redis server.    |
| redis-cli monitor     | Streams real-time commands received by the Redis server. |
| redis-cli info memory | Shows memory usage statistics of the Redis server.       |
| redis-cli client list | Lists all connected client sessions to the Redis server. |

### Log Files

Monitoring Redis logs is important for troubleshooting and understanding the health of Redis server. Here is some of commands to monitor and debug redis logs.

```bash
# View Redis logs
sudo tail -f /var/log/redis/redis-server.log

# Check system logs
sudo journalctl -u redis-server -f

# View error logs
sudo grep ERROR /var/log/redis/redis-server.log
```
## Disaster Recovery

Disaster recovery is crucial to ensure that your Redis data can be restored in case of unexpected failures. Below are simple, step-by-step instructions to set up backup and recovery processes.

---

### Backup Strategies

Backups are essential to protect your data. Redis offers two main ways to create backups: RDB (snapshot) and AOF (append-only file). Below are the steps for each.

#### 1. Create an RDB (Snapshot) Backup

The RDB file is a point-in-time snapshot of redis dataset.

```bash
# This command tells Redis to create a snapshot in the background.
redis-cli BGSAVE
```
- The backup file is usually saved as `dump.rdb` in `/var/lib/redis/`.

#### 2. Create an AOF (Append-Only File) Backup

The AOF file logs every write operation received by the server.

```bash
# This command tells Redis to rewrite the append-only file in the background.
redis-cli BGREWRITEAOF
```
- The AOF file is usually saved as `appendonly.aof` in `/var/lib/redis/`.

#### 3. Manual Backup (Copy the Backup File)

To keep copies of your backups (recommended!), copy the backup files to a safe location.

```bash
# Copy the RDB backup file with today's date for easy identification.
sudo cp /var/lib/redis/dump.rdb /backup/redis-$(date +%Y%m%d).rdb
```
- Make sure the `/backup` directory exists. You can create it using:  
  `sudo mkdir -p /backup`

---

### Recovery Procedures

If you need to restore your Redis data from a backup, follow these steps:

#### 1. Stop the Redis Service

Before restoring, stop the Redis server to prevent data corruption.

```bash
sudo systemctl stop redis-server
```

#### 2. Restore the RDB Backup

Copy your backup file to the Redis data directory.

```bash
# Replace "/backup/redis-backup.rdb" with your actual backup file path.
sudo cp /backup/redis-backup.rdb /var/lib/redis/dump.rdb
```

#### 3. Set File Permissions

Ensure the Redis process can access the backup file.

```bash
sudo chown redis:redis /var/lib/redis/dump.rdb
```

#### 4. Start the Redis Service

After restoring the backup, restart the Redis server.

```bash
sudo systemctl start redis-server
```

---


### Best Practices

- **Regular Backups**: Schedule daily RDB and AOF backups
- **Off-site Storage**: Store backups in multiple locations
- **Testing**: Regularly test recovery procedures
- **Monitoring**: Set up alerts for backup failures

## High Availability

High availability ensures  Redis service stays online even if some servers fail. Below are beginner-friendly steps for setting up replication, Sentinel, and a Redis cluster.

---

### 1. Replication

- **Replication** Replication allows one Redis server data (the master) to be copied (replicated) to one or more Redis servers (the replicas/slaves).
- If the master fails, a replica can take over as the new master, minimizing downtime.
- Replication helps distribute read requests and provides data redundancy.

### 2. Sentinel

- **Redis Sentinel** is a monitoring system for Redis.
- It automatically detects if the master server goes down and initiates a failover by promoting a replica to master.
- Sentinel also notifies clients about the new master, so applications can reconnect automatically.

### 3. Cluster

- **Redis Cluster** allows you to run Redis across multiple servers (nodes) with automatic data sharding and replication.
- Data is split across nodes, so each node holds a portion of the dataset.
- Each part of the data has a replica for redundancy.
- If a node fails, the cluster can continue to operate, and a replica can be promoted to replace the failed node.

---


## Troubleshooting

### Common Issues

#### 1. Redis Won't Start

**Issue**: Redis service fails to start
```bash
# Check error logs
sudo tail -f /var/log/redis/redis-server.log

# Check configuration syntax
redis-server /etc/redis/redis.conf --test
```

**Solution**: Verify configuration file syntax and permissions

#### 2. Memory Issues

**Issue**: Redis runs out of memory
```bash
# Check memory usage
redis-cli info memory

# Check memory policy
redis-cli config get maxmemory-policy
```

**Solution**: Adjust `maxmemory` and `maxmemory-policy` settings

#### 3. Connection Refused

**Issue**: Cannot connect to Redis
```bash
# Check if Redis is running
sudo systemctl status redis-server

# Check port binding
netstat -tlnp | grep 6379

# Check firewall
sudo ufw status
```

**Solution**: Ensure Redis is running and port 6379 is accessible

#### 4. Slow Performance

**Issue**: Redis response times are slow
```bash
# Check Redis info
redis-cli info stats

# Monitor commands
redis-cli monitor

# Check memory fragmentation
redis-cli info memory
```

**Solution**: Optimize configuration, check for memory fragmentation

#### 5. Authentication Errors

**Issue**: Authentication failed
```bash
# Check password configuration
redis-cli config get requirepass

# Connect with password
redis-cli -a your_password
```

**Solution**: Verify password configuration and use correct authentication


## Contact Information
| **Name**           | **Email address**                           |
|--------------------|---------------------------------------------|
| Meenu Chauhan      |  meenu.chauhan.snaatak@mygurukulam.co |

---

## References
| Reference               | Link                                                                           |
|-------------------------|--------------------------------------------------------------------------------|
| Redis Documentation    | [Click here](https://redis.io/docs/latest/develop/get-started/) |
| Redis Introduction     | [Click here](https://www.ibm.com/think/topics/redis) |
| Redis Configuration  | [Click here](https://www.geeksforgeeks.org/system-design/complete-tutorial-of-configuration-in-redis/) |
| Redis Backup Strategies | [Click here](https://trilio.io/resources/redis-backup/) | 

