# SOP for Gunicorn Installation with Python

## Objective

This guide details the process for installing Gunicorn with Python on a Linux system. Gunicorn is a production-grade WSGI HTTP server commonly used to serve Python web applications.

---

## Prerequisites

- Linux server (Ubuntu/Debian/CentOS/RHEL)
- Python 3.7 or newer installed
- pip and python3-venv installed 
- Sudo/root privileges

---


## 1. Install Python 3, pip and python3-venv (if not already installed)

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt upgrade -y
```
```bash
sudo apt install python3 python3-pip python3-venv -y
```

---

## 2. Create a Python Virtual Environment

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

## 3. Install Gunicorn

In your (activated) virtual environment, run:
```bash
pip install gunicorn
```

*Alternatively, to install system-wide (not recommended for most cases):*
```bash
sudo pip3 install gunicorn
```

---

## 4. Verify Gunicorn Installation

Check the installed version:
```bash
gunicorn --version
```

---


## 5. Common Gunicorn Usage

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



## 9. Additional Resources

- [Gunicorn Documentation](https://docs.gunicorn.org/en/stable/)
- [Flask Deployment Options](https://flask.palletsprojects.com/en/latest/deploying/wsgi-standalone/)
- [Django with Gunicorn](https://docs.djangoproject.com/en/stable/howto/deployment/wsgi/gunicorn/)

---
