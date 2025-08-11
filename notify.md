# POC Of Notification Worker

## Author Information

| Created by      | Created on         | Version          | Last updated On   | pre Reviewer       | L0 Reviewer     | L1 Reviewer          |    L2 Reviewer    |
|-----------------|--------------------|------------------|-------------------|--------------------|-----------------|----------------------|-------------------|
| Meenu Chauhan   |  29-07-2025        | V 1.0            |        |  -          |  -      |     -   |   - |

# Table of Contents

1. [Pre-Requisites](#pre-requisites)  
2. [System Requirements](#system-requirements)  
3. [Architecture](#architecture)  
4. [Ports](#ports)  
5. [API Setup and Execution](#api-setup-and-execution)  
   - [Installing Dependencies for Notification Worker](#installing-dependencies-for-notification-worker)  
   - [Elasticsearch Configuration](#elasticsearch-configuration)  
   - [SMTP Configuration](#smtp-configuration)  
   - [Working with the Notification Worker Repository](#working-with-the-notification-worker-repository)  
   - [Access the Application](#access-the-application)  
   - [Test Complete Workflow](#test-complete-workflow)  
6. [Conclusion](#conclusion)  
7. [Contact Information](#contact-information)  
8. [References](#references)

---

# Pre-Requisites

The Notification Worker application has some external service dependencies and package requirements. Some dependencies are optional and some are mandatory. To run the application, we need Python as the runtime environment, but for full functionality following things are required:

| Tool/Resource                          | Description                                                                                              |
|----------------------------------------|----------------------------------------------------------------------------------------------------------|
| [Python 3](https://www.python.org/)    | A high-level, interpreted programming language used for the notification worker application. |
| [Elasticsearch](https://www.elastic.co/) | A distributed search and analytics engine used to store and retrieve employee data. |
| [Gmail SMTP](https://medium.com/rails-to-rescue/how-to-set-up-smtp-credentials-with-gmail-for-your-app-send-email-cf236d11087d) | Email service for sending notifications using Gmail's SMTP server. |

---

# System Requirements

| Requirement   | Details                      |
|---------------|------------------------------|
| OS            | Ubuntu or other Linux-based OS    |
| RAM           | 2 GB minimum                |
| Disk Space    | 8 GB                        |
| Processor     | Dual-core recommended       |
| Instance Type | t2.medium                   |
| Python        | Python 3.7 or higher        |
| Network       | Internet access for SMTP     |

---

# Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Notification  │    │   Elasticsearch │    │   Gmail SMTP    │
│     Worker      │◄──►│   (Employee     │    │   Server        │
│   (Python App)  │    │    Database)    │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Config File   │    │   Employee      │    │   Email         │
│  (config.yaml)  │    │   Management    │    │   Notifications │
│                 │    │   Index         │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

**Flow:**
1. Notification Worker reads configuration from `config.yaml`
2. Connects to Elasticsearch to fetch employee data
3. Sends email notifications via Gmail SMTP server
4. Runs on scheduled intervals or on-demand

---

# Ports

| *Port* | *Protocol/Service*       | *Description*                                                                 |
|----------|----------------------------|---------------------------------------------------------------------------------|
| 22       | SSH                        | Used for secure shell access to the server.                                    |
| 9200     | HTTP/HTTPS (Elasticsearch) | Default port for connecting to Elasticsearch server.                          |
| 80       | HTTP                       | Used for standard web traffic and serving HTTP requests.                      |
| 443      | HTTPS                      | Used for secure web traffic and serving HTTPS requests.                       |

---

# API Setup and Execution

## Installing Dependencies for Notification Worker

| *Tool*           | *Installation Steps*                                                                                              |
|------------------|--------------------------------------------------------------------------------------------------------------------|
| *Python 3*       | Follow this [link](https://github.com/Snaatak-Cloudops-Crew/documentation/tree/scrum-16-sonal/Softwares/Python/Installation/Manual) to install Python 3.7+ |
| *Elasticsearch*  | Follow this [link](https://www.fosstechnix.com/how-to-install-elastic-stack-on-ubuntu-22-04/) to install Elasticsearch |

### Python Dependencies
The application requires the following Python packages (automatically installed via requirements.txt):
- `emails==0.6` - Email sending library
- `elasticsearch==7.8.0` - Elasticsearch client
- `config-with-yaml==0.1.0` - YAML configuration parser
- `schedule==0.6.0` - Task scheduling library


## Elasticsearch Configuration

### EC2 Instance Setup (Elasticsearch)
Since Elasticsearch is installed on an EC2 instance, follow these steps:

**Step 1: Install Java**
```bash
sudo apt update
sudo apt install openjdk-17-jdk openjdk-17-jre
java --version
```
<img width="1146" height="73" alt="Screenshot 2025-07-31 at 11 50 34 AM" src="https://github.com/user-attachments/assets/10d9e1f4-9557-418f-8f8a-4f999234b5f1" />

**Step 2: Install Elasticsearch**
```bash
sudo apt update -y
apt install apt-transport-https -y
sudo apt install apt-transport-https -y
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt update
sudo apt install elasticsearch
```
<img width="1416" height="102" alt="Screenshot 2025-07-31 at 11 52 15 AM" src="https://github.com/user-attachments/assets/b9ce8e9b-08ed-4239-b07e-a7695b055790" />

**Step 3: Start Elasticsearch Service**
```bash
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service
sudo systemctl start elasticsearch.service
sudo systemctl status elasticsearch.service
```
<img width="1417" height="347" alt="Screenshot 2025-07-31 at 11 59 23 AM" src="https://github.com/user-attachments/assets/c0582720-095c-452f-93b0-35f9ad1b74cd" />

**Step 4: Configure Elasticsearch**
```bash
sudo vi /etc/elasticsearch/elasticsearch.yml
```

**Add/Update these properties in elasticsearch.yml:**
```yaml
network.host: 0.0.0.0
xpack.security.http.ssl:  # to disable ssl
  enabled: false

discovery.seed_hosts: [ ]
```

<img width="1117" height="144" alt="Screenshot 2025-07-31 at 12 00 27 PM" src="https://github.com/user-attachments/assets/e3737f40-a1db-4b5a-811f-43142a5d6faa" />

**Step 5: Generate Password**
```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
```
<img width="1312" height="140" alt="Screenshot 2025-07-31 at 12 47 25 PM" src="https://github.com/user-attachments/assets/20185f90-102c-429a-860c-e3fbc5e25229" />

**Step 6: Restart and Verify**
```bash
sudo systemctl restart elasticsearch
curl -u <elastic_user>:<elastic_password> http://localhost:9200
```
<img width="1110" height="301" alt="Screenshot 2025-07-31 at 1 06 43 PM" src="https://github.com/user-attachments/assets/231dedb2-e10e-442c-bc0d-88654aa2822b" />

5. **Test Elasticsearch:**
   ```bash
   curl -u <elastic_user>:<elastic_password> http://localhost:9200
   ```


### Create Employee Index and Test Data

**Replace 'your-password' with the password generated in step 5**

```bash
# Create index 
curl -X PUT "http://localhost:9200/employee-management" -u elastic:<your-password>

# Add sample employee
curl -X POST "http://localhost:9200/employee-management/_doc" -u elastic:your-password -H "Content-Type: application/json" -d '{"email_id": "meenutest95@gmail.com", "name": "meenu"}'

```
<img width="1418" height="128" alt="Screenshot 2025-07-31 at 2 41 41 PM" src="https://github.com/user-attachments/assets/a25de24b-e195-4669-b1aa-fd031a9f4373" />

---


## SMTP Configuration

### Gmail SMTP Setup
1. Enable 2-Factor Authentication on your Gmail account
2. Generate App Password:
   - Go to Google Account Settings
   - Security → 2-Step Verification → App passwords
     <img width="1058" height="310" alt="Screenshot 2025-07-31 at 1 21 32 PM" src="https://github.com/user-attachments/assets/896c7c21-5b9a-48ed-816e-ec519a9104c5" />

   - Enter App Name and generate password
     
  <img width="1030" height="400" alt="Screenshot 2025-07-31 at 1 22 20 PM" src="https://github.com/user-attachments/assets/54592b97-fab2-4f66-b94e-e886620cee2b" />

     
3. Use the generated password in config.yaml

**Example Gmail configuration:**
```yaml
smtp:
  from: "your-email@gmail.com"
  username: "your-email@gmail.com"
  password: "your-16-digit-app-password"
  smtp_server: "smtp.gmail.com"
  smtp_port: "587"
```

## Working with the Notification Worker Repository


**Step 1: Install Python and Git**
```bash
sudo apt update
sudo apt upgrade -y
sudo apt install python3 python3-pip python3-venv -y
python3 --version
```
<img width="948" height="52" alt="Screenshot 2025-07-31 at 12 06 57 PM" src="https://github.com/user-attachments/assets/e064479a-cd2a-44a2-95f9-e37da669021f" />

**Step 2: Clone and Setup Repository**
```bash
git clone https://github.com/OT-MICROSERVICES/notification-worker.git
cd notification-worker/
ls
```

<img width="1307" height="188" alt="Screenshot 2025-07-31 at 12 07 25 PM" src="https://github.com/user-attachments/assets/0a565664-4ed2-44f4-b743-ee4fb0854614" />

**Step 3: Create Virtual Environment**
```bash
python3 -m venv venv
source venv/bin/activate
```
<img width="1139" height="199" alt="Screenshot 2025-07-31 at 12 08 24 PM" src="https://github.com/user-attachments/assets/61557d46-3d78-428e-86c1-a7b7c7687854" />

**Step 4: Install Dependencies**
```bash
pip3 install -r requirements.txt
```

**Step 5: Update application configuration**
```bash
sudo vi config.yaml
```

**Example configuration:**
```yaml
---
smtp:
  from: "your-email@gmail.com"
  username: "your-email@gmail.com"
  password: "your-app-password"
  smtp_server: "smtp.gmail.com"
  smtp_port: "587"

elasticsearch:
  username: "elastic"
  password: "your-password"
  host: "your-elasticsearch-host"
  port: 9200
```

**Step 7: Set Environment Variable**
```bash
export CONFIG_FILE=config.yaml
```

**Step 8: Run Application**

*1. Run in external mode (send once and exit):*
```bash
python3 notification_api.py --mode external
```
<img width="1210" height="131" alt="Screenshot 2025-07-31 at 2 44 24 PM" src="https://github.com/user-attachments/assets/4de63ff1-f2c1-4a65-8756-e7d5071e36ec" />

*2. Run in scheduled mode (continuous):*
```bash
python3 notification_api.py --mode scheduled
```

*3. Create a service file (optional):*

```bash
sudo nano /etc/systemd/system/notification-worker.service
```

**Service file content:**
```ini
[Unit]
Description=Notification Worker Service
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/notification-worker
ExecStart=/home/ubuntu/notification-worker/venv/bin/python notification_api.py --mode scheduled
SuccessExitStatus=143
Restart=on-failure
Environment=CONFIG_FILE=/home/ubuntu/notification-worker/config.yaml

[Install]
WantedBy=multi-user.target
```

*4. Service File Commands*

| Command                                  | Description                                                              |
|------------------------------------------|--------------------------------------------------------------------------|
| sudo systemctl daemon-reload           | Reloads systemd manager configuration to recognize changes in service files. |
| sudo systemctl enable notification-worker.service | Enables the service to start automatically on system boot.              |
| sudo systemctl start notification-worker.service | Starts the notification-worker service immediately.                            |
| sudo systemctl status notification-worker.service | Displays the current status of the service, including logs and errors.  |

## Access the Application

The Notification Worker runs as a background service and doesn't expose a web interface. You can:

1. **Check logs:**
```bash
sudo journalctl -u notification-worker.service -f
```

2. **Send emails immediately:**
```bash
python3 notification_api.py --mode external
```

3. **Monitor scheduled runs:**
```bash
python3 notification_api.py --mode scheduled
```

---

## Test Complete Workflow
```bash
# 1. Ensure Elasticsearch is running on EC2 instance
# 2. Ensure employee-management index exists with data
# 3. Update config.yaml with EC2 IP address
# 4. Run notification worker
python3 notification_api.py --mode external
```

# Conclusion

The Notification Worker is a robust Python application designed to send automated email notifications to employees. It leverages Elasticsearch for employee data management, SMTP servers for email delivery, and scheduling capabilities for automated execution. The application is designed for reliability, scalability, and easy integration with existing systems.

**Key Features:**
- Automated email notifications
- Integration with Elasticsearch for employee data
- Configurable SMTP settings
- Scheduled and on-demand execution modes
- Comprehensive logging and error handling
- Easy deployment and maintenance

**Benefits:**
- Reduces manual notification tasks
- Ensures timely delivery of important messages
- Scalable for large employee bases
- Easy to configure and maintain
- Uses Gmail SMTP for reliable email delivery

---

## Contact Information
| **Name**           | **Email address**                           |
|--------------------|---------------------------------------------|
| Meenu Chauhan      | [meenutest95@gmail.com](mailto:meenutest95@gmail.com) |

---

## References
| Reference               | Link                                                                           |
|-------------------------|--------------------------------------------------------------------------------|
| Python Documentation    | [Click here](https://docs.python.org/3/) |
| Elasticsearch Guide     | [Click here](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html) |
| SMTP Configuration For Gmail  | [Click here](https://medium.com/rails-to-rescue/how-to-set-up-smtp-credentials-with-gmail-for-your-app-send-email-cf236d11087d) |
| Python Virtual Environments | [Click here](https://docs.python.org/3/tutorial/venv.html) | 
