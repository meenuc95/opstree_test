<img width="900" height="400" alt="fae862fff4f6100d000a1c01c4030db0" src="https://github.com/user-attachments/assets/ab934a4d-7d93-4e33-9972-e6db6e8aa5a6" />

# POC Of Full Stack API

## Author Information

| Created by      | Created on         | Version  | Last updated On   | Pre Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer |
|-----------------|--------------------|----------|-------------------|--------------|-------------|-------------|-------------|
| Meenu Chauhan   | 05-08-2025         | V 1.0    | -                 | Siddarth/Sahil| -           | -           | -           |

---

# Table of Contents
1. [Introduction](#introduction)
2. [Pre-Requisites](#pre-requisites)
3. [System Requirements](#system-requirements)
4. [Architecture](#architecture)
5. [Ports](#ports)
6. [API Setup and Execution](#api-setup-and-execution)
   - [Installing Dependencies](#installing-dependencies)
   - [Working with the Full Stack API Git Repository](#working-with-the-full-stack-api-git-repository)
   - [Starting the Application](#starting-the-application)
   - [Access the API](#access-the-api)
7. [Configuring Frontend to Connect to Backend](#configuring-frontend-to-connect-to-backend)
   - [Environment Variable Setup](#environment-variable-setup)
   - [Using API URLs in Code](#using-api-urls-in-code)
   - [CORS Setup on Backend](#cors-setup-on-backend)
   - [Proxying API Requests (Optional)](#proxying-api-requests-optional)
   - [Testing and Troubleshooting](#testing-and-troubleshooting)
8. [Conclusion](#conclusion)
9. [Contact Information](#contact-information)
10. [References](#references)

---

# Introduction

This documentation walks you through the end-to-end setup of the Full Stack API, including installation of required tools, configuration, and deployment. It covers backend technologies (Java, Python, Golang), databases (ScyllaDB, PostgreSQL), cache (Redis), and frontend (ReactJS, Swagger UI for API testing). You will also learn how to connect your frontend (this repo) to multiple backend APIs.

---

## Pre-requisites

The Full Stack API application has dependencies on other REST APIs from **[OT-Microservices](https://github.com/OT-MICROSERVICES)**. To run the application successfully, ensure the following APIs are configured and running:

* **[Employee API](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/SCRUM-67-Sneha-Joshi/OT-Microservices/Applications/Employee-API/POC/README.md)**
* **[Attendance API](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-7-aryan-mishra/OT-Microservices/Applications/Attendance-Api/Introduction/README.md)**
* **[Salary API](https://github.com/Snaatak-Cloudops-Crew/documentation/tree/e64036f22062fc2620205cbcfbba243c15db701f/OT-Microservices/Applications/Salary-API/POC)**
* **[Frontend API](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/SCRUM-71-Ashutosh/OT-Microservices/Applications/frontend-api/POC/README.md)**

---

# System Requirements

| Requirement   | Details                      |
|---------------|-----------------------------|
| **OS**        | Ubuntu or other Linux-based OS |
| **RAM**       | 4 GB minimum                |
| **Disk Space**| 40 GB                       |
| **Processor** | Dual-core recommended       |
| **Instance**  | t2.large                    |

---

# Architecture

<img width="2912" height="1156" alt="frontend (1)" src="https://github.com/user-attachments/assets/357cc233-d73c-4615-b625-e35342c782e9" />

---

# Ports

| Port  | Protocol/Service  | Description                                         |
|-------|-------------------|-----------------------------------------------------|
| 22    | SSH               | Secure shell access to the server                   |
| 8080  | HTTP (Swagger UI) | Access Swagger UI for API documentation             |
| 9042  | ScyllaDB          | Default port for ScyllaDB NoSQL database            |
| 6379  | Redis             | Default port for Redis cache                        |
| 80    | HTTP              | Standard web traffic                                |
| 443   | HTTPS             | Secure web traffic                                  |

---

# API Setup and Execution

## Setup Backend and Frontend API

| API Name       | Link                                                                                                                                                                   |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Employee API   | [Configure Employee API](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/SCRUM-67-Sneha-Joshi/OT-Microservices/Applications/Employee-API/POC/README.md)            |
| Attendance API | [Configure Attendance API](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-7-aryan-mishra/OT-Microservices/Applications/Attendance-Api/Introduction/README.md) |
| Salary API     | [Configure Salary API](https://github.com/Snaatak-Cloudops-Crew/documentation/tree/e64036f22062fc2620205cbcfbba243c15db701f/OT-Microservices/Applications/Salary-API/POC)    |
| Frontend API   | [Configure Frontend API](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/SCRUM-71-Ashutosh/OT-Microservices/Applications/frontend-api/POC/README.md)               |

---

# Configuring Frontend to Connect to Backend

Connecting your frontend (ReactJS or vanilla JavaScript) with backend APIs is essential for full-stack development. Below are detailed steps and code examples.

---

## Environment Variable Setup

- In the `frontend` directory, create or update a `.env` file:
  <img width="600" height="91" alt="Screenshot 2025-08-06 at 7 30 02 PM" src="https://github.com/user-attachments/assets/a7268a2b-408e-46e3-acdc-a04ae285b981" />

- Change URLs/ports as per your backend setup.
- When you update `.env`, restart your frontend server to apply changes.

---

## Using API URLs in Code

Update your frontend code to use environment variables for backend URLs, which makes switching environments easy.

**Example (ReactJS/JavaScript):**

  src/EmployeeList.js

<img width="827" height="191" alt="Screenshot 2025-08-06 at 7 30 34 PM" src="https://github.com/user-attachments/assets/ac7f9154-7ac4-41b1-8c6b-eee38c368867" />


**Example Files to Update:**

| File Name                | Example Endpoint(s) Used                |
|--------------------------|-----------------------------------------|
| src/AttendanceForm.js    | `/attendance/create`                    |
| src/AttendanceList.js    | `/attendance/search`                    |
| src/EmployeeForm.js      | `/employee/create`, `/notification/send`|
| src/EmployeeData.js      | `/employee/search/all`, `/employee/search/status`, etc. |
| src/EmployeeList.js      | `/employee/search/all`                  |
| src/ListSalary.js        | `/salary/search/all`                    |

> **Tip:** Always use environment variables (e.g., `process.env.REACT_APP_EMPLOYEE_API_URL`) for API endpoints in your code.

---

## CORS Setup on Backend

- **CORS (Cross-Origin Resource Sharing)** must be enabled on the backend to allow requests from your frontend.
- Example for Express (Node.js):
  ```js
  const cors = require('cors');
  app.use(cors({
    origin: 'http://localhost:3000'
  }));
  ```
- For Java/Spring, add appropriate CORS filters.
- For Python/Flask:
  ```python
  from flask_cors import CORS
  CORS(app, origins=["http://localhost:3000"])
  ```

---

## Serving React Build with NGINX

After building React app for production (`npm run build`), we can use NGINX to serve the static files efficiently. Here’s how you can set this up:

---

### 1. Build Your React App

```bash
npm run build
```
This generates a `build` directory containing your production-ready React files.

---

### 2. Install NGINX

**On Ubuntu:**
```bash
sudo apt update
sudo apt install nginx
```

---

### 3. Configure NGINX

Create or edit the NGINX configuration file (e.g., `/etc/nginx/sites-available/react-app`):

```nginx
server {
    listen 80;
    server_name your-domain.com; # Change to your domain or server IP

    root /var/www/react-app/build;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # Optional: Proxy API requests to backend
    location /api/ {
        proxy_pass http://localhost:5000/api/; # Adjust backend URL/port as needed
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

**Steps:**
- Copy your React `build` directory to `/var/www/react-app/build` .
- Link your config:  
  ```bash
  sudo ln -s /etc/nginx/sites-available/react-app /etc/nginx/sites-enabled/
  ```
- Test NGINX config:
  ```bash
  sudo nginx -t
  ```
- Reload NGINX:
  ```bash
  sudo systemctl reload nginx
  ```

---

### 4. Access Your Application

- Go to `http://your-domain.com` or your server’s IP in a browser.
- Your React app should load and client-side routing will work.

---


## Testing and Troubleshooting

- Start backend services (Employee, Attendance, Salary APIs) and ensure they are running.
- Start your frontend (`npm start` or `yarn start`).
- Use browser DevTools (Network tab) to check API calls and responses.
- If you change `.env` or code, restart your frontend server.
- Common issues:
  - **CORS errors:** Check backend CORS settings.
  - **404/500 errors:** Check endpoint URLs and backend status.
  - **Network errors:** Ensure backend is running and accessible.

---

# Conclusion

With these steps, frontend will successfully interact with the backend APIs (Employee, Attendance, Salary) for a robust full-stack application. Always use environment variables for API endpoints and ensure proper CORS configuration.

---

## Contact Information

| Name           | Email address                           |
|----------------|-----------------------------------------|
| Meenu Chauhan  | meenu.chauhan.snaatak@mygurukulam.co    |

---

## References

| Reference               | Link                                                                           |
|-------------------------|--------------------------------------------------------------------------------|
| Environment Variables In React   | [Click here](https://medium.com/@bhairabpatra.iitd/env-file-in-react-js-09d11dc77924)                                       |
| CORS Guide     | [Click here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS) |
| Chrome DevTools Guide  | [Click here](https://www.freecodecamp.org/news/chrome-devtools/) |
