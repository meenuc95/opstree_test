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

```bash
# View Redis logs
sudo tail -f /var/log/redis/redis-server.log

# Check system logs
sudo journalctl -u redis-server -f

# View error logs
sudo grep ERROR /var/log/redis/redis-server.log
```

## Disaster Recovery

### Backup Strategies

```bash
# Create RDB backup
redis-cli BGSAVE

# Create AOF backup
redis-cli BGREWRITEAOF

# Manual backup
sudo cp /var/lib/redis/dump.rdb /backup/redis-$(date +%Y%m%d).rdb
```

### Recovery Procedures

```bash
# Stop Redis service
sudo systemctl stop redis-server

# Restore from RDB backup
sudo cp /backup/redis-backup.rdb /var/lib/redis/dump.rdb

# Set proper permissions
sudo chown redis:redis /var/lib/redis/dump.rdb

# Start Redis service
sudo systemctl start redis-server
```

### Best Practices

- **Regular Backups**: Schedule daily RDB and AOF backups
- **Off-site Storage**: Store backups in multiple locations
- **Testing**: Regularly test recovery procedures
- **Documentation**: Maintain detailed recovery procedures
- **Monitoring**: Set up alerts for backup failures

## High Availability

### Replication Setup

```bash
# On master server (redis.conf)
bind 0.0.0.0
port 6379

# On slave server (redis.conf)
bind 0.0.0.0
port 6379
replicaof <master-ip> 6379
```

### Sentinel Configuration

```bash
# Create sentinel configuration
sudo nano /etc/redis/sentinel.conf

# Add configuration
port 26379
sentinel monitor mymaster <master-ip> 6379 2
sentinel down-after-milliseconds mymaster 5000
sentinel failover-timeout mymaster 10000
```

### Cluster Setup

```bash
# Create cluster configuration
redis-cli --cluster create <node1-ip>:6379 <node2-ip>:6379 <node3-ip>:6379 \
  <node4-ip>:6379 <node5-ip>:6379 <node6-ip>:6379 --cluster-replicas 1
```



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




This comprehensive Redis documentation provides all the necessary information for installation, configuration, maintenance, and troubleshooting of Redis in production environments.
