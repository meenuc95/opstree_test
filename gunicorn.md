# Gunicorn Installation with Python(SOP)

## Author Information

| Author          | Created on | Version   | Last updated by | Internal Reviewer |
|-----------------|------------|-----------|------------------|--------------------|
| Meenu Chauhan       | 17-07-25   | version 1 | 18-07-25              | Siddarth          | 

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
Gunicorn is a Production-ready Python WSGI server used to serve Python web applications that receive requests sent to the Web Server from a Client and forwards 
them onto the Python applications or Web Frameworks (such as Flask or Django). WSGI is a standard interface that enables Python applications and 
frameworks to communicate with web servers. Gunicorn generate  multiple worker processes to handle multiple requests simultaneously, improving performance.

---

## Prerequisites

- Linux server (Ubuntu/Debian/CentOS/RHEL)
- Python 3.7 or newer installed
- pip and python3-venv installed 
- Sudo/root privileges

---


## Install Python 3, pip and python3-venv (if not already installed)

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt upgrade -y
```
```bash
sudo apt install python3 python3-pip python3-venv -y
```

---

## Create a Python Virtual Environment

Navigate to your project directory or create a new one:
```bash
mkdir ~/myproject
cd ~/myproject
```

Create and activate a virtual environment:
```bash
python3 -m venv venv
source venv/bin/activate
```

---

## Install Gunicorn

In your (activated) virtual environment, run:
```bash
pip install gunicorn
```

*Alternatively, to install system-wide (not recommended for most cases):*
```bash
sudo pip3 install gunicorn
```

---

## Verify Gunicorn Installation

Check the installed version:
```bash
gunicorn --version
```

---


## Common Gunicorn Usage

- **Specify number of worker processes:**
  ```bash
  gunicorn -w 4 app:app
  ```
- **Bind to a specific IP address and port:**
  ```bash
  gunicorn -b 0.0.0.0:8080 app:app
  ```
- **Run as a background (daemon) process:**
  ```bash
  gunicorn -D app:app
  ```

---
## Contact Information

| Name            | Email address |
|-----------------|---------------|
| Meenu Chauhan       | meenu.chauhan.snaatak@mygurukulam.co

## References
| **Link**                                                                 | **Description**                                   |
|--------------------------------------------------------------------------|---------------------------------------------------|
| [Redhat Official Document](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/kernel_administration_guide/working_with_sysctl_and_kernel_tunables) | Document format followed from this link.          |
| [Phoenixnap Document](https://phoenixnap.com/kb/sysctl) | Document  link.          |



## Additional Resources

- [Gunicorn Documentation](https://docs.gunicorn.org/en/stable/)
- [Flask Deployment Options](https://flask.palletsprojects.com/en/latest/deploying/wsgi-standalone/)
- [Django with Gunicorn](https://docs.djangoproject.com/en/stable/howto/deployment/wsgi/gunicorn/)

---
