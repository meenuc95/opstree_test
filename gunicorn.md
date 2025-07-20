# Gunicorn Installation with Python(SOP)

## Author Information

| Author          | Created on | Version   | Last updated by | Internal Reviewer |
|-----------------|------------|-----------|------------------|--------------------|
| Meenu Chauhan       | 17-07-25   | version 1 | 18-07-25              | Siddarth          | 

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Update System Packages](#update-system-packages)
- [Install Python 3, pip and python3-venv](#install-python-3-pip-and-python3-venv-if-not-already-installed)
- [Create a Python Virtual Environment](#create-a-python-virtual-environment)
- [Install Gunicorn](#install-gunicorn)
- [Verify Gunicorn Installation](#verify-gunicorn-installation)
- [Common Gunicorn Usage](#common-gunicorn-usage)
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

| Requirement           | Description                          |
|-----------------------|--------------------------------------|
| OS                    | Linux (Ubuntu/Debian/CentOS/RHEL)   |
| Python Version        | Python 3.7 or newer                  |
| Python Tools          | `pip`, `python3-venv` must be installed |
| User Privileges       | `sudo` or root access required       |


---
## Update System Packages

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt upgrade -y
```

## Install Python 3, pip and python3-venv (if not already installed)

**Ubuntu/Debian:**
```bash
sudo apt install python3 python3-pip python3-venv -y
```
**Verify installation:**

```bash
python3 --version
python3-pip --version
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

## 6. Test Gunicorn with a Sample Python App

Create a file named `app.py` in your project directory:
```python
# app.py
def app(environ, start_response):
    data = b"Hello, Gunicorn!\n"
    start_response("200 OK", [
        ("Content-Type", "text/plain"),
        ("Content-Length", str(len(data)))
    ])
    return iter([data])
```

Run Gunicorn:
```bash
gunicorn --bind 0.0.0.0:8000 app:app
```


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

## Troubleshootings 
# Check gunicorn processes
ps aux | grep gunicorn


# Test gunicorn manually
cd /path/to/your/backend
source venv/bin/activate
gunicorn --bind 0.0.0.0:8000 app:app
## Contact Information

| Name            | Email address |
|-----------------|---------------|
| Meenu Chauhan       | meenu.chauhan.snaatak@mygurukulam.co

## References
| **Link**                                                                 | **Description**                                   |
|--------------------------------------------------------------------------|---------------------------------------------------|
| [Gunicorn Documentation](https://gunicorn.org/) | Gunicorn setup document link.          |
| [Gunicorn]([https://phoenixnap.com/kb/sysctl](https://dev.to/doridoro/what-is-gunicorn-4n26)) | Gunicorn intoduction document  link.          |





---
