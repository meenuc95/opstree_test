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
<img width="1000" height="271" alt="Screenshot 2025-07-22 at 12 53 57 PM" src="https://github.com/user-attachments/assets/d38e4beb-7246-4874-b5ba-c95cf4d676aa" />

**Verify installation:**

```bash
python3 --version
pip3 --version
```
<img width="1000" height="100" alt="Screenshot 2025-07-22 at 12 56 18 PM" src="https://github.com/user-attachments/assets/b6c7f08d-4335-497a-97b9-d75717fc1216" />


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
<img width="1000" height="198" alt="Screenshot 2025-07-22 at 12 57 36 PM" src="https://github.com/user-attachments/assets/02017b66-4de1-4dc8-a5ec-f3b795007c94" />

---

## Install Gunicorn

In your (activated) virtual environment, run:
```bash
pip3 install gunicorn
```
<img width="1000" height="193" alt="Screenshot 2025-07-22 at 12 57 57 PM" src="https://github.com/user-attachments/assets/ef7b9590-0d01-4cfd-884b-c24177e1ae1e" />

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
<img width="1000" height="241" alt="Screenshot 2025-07-22 at 12 58 15 PM" src="https://github.com/user-attachments/assets/7409b2c2-5d09-4426-b38b-8261eeab39e2" />

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
<img width="1000" height="181" alt="Screenshot 2025-07-22 at 12 59 24 PM" src="https://github.com/user-attachments/assets/22d91773-3007-42a1-a895-61842ab459e5" />

<img width="1000" height="68" alt="Screenshot 2025-07-22 at 1 00 04 PM" src="https://github.com/user-attachments/assets/270d4c80-38d5-4992-bce8-52b11a5bb046" />



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
## Troubleshooting

If Gunicorn is not working as expected, follow the steps below to troubleshoot common issues:

### Check Gunicorn Processes

Verify if Gunicorn is running:

```bash
ps aux | grep gunicorn
```

### Test Gunicorn Manually
We can manually test if  Gunicorn server works by running it directly from your project directory:

```bash
cd /path/to/your/backend
source venv/bin/activate
gunicorn --bind 0.0.0.0:8000 app:app
```

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
