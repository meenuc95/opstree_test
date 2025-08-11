# DNS Implementation 

| Author         | Created on   | Version | Last Updated On | Reviewed By     |
|----------------|--------------|---------|-----------------|-----------------|
| Meenu Chauhan  | 05-08-2025   | V1.0     | 05-08-2025      | -               |

---

## Table of Contents

1. [Introduction](#1-introduction)  
2. [What is DNS?](#2-what-is-dns)  
3. [Why Do You Need DNS?](#3-why-do-you-need-dns)  
4. [Prerequisites](#4-prerequisites)  
5. [DNS Record Types](#5-dns-record-types)  
6. [Configuring DNS in GoDaddy](#6-configuring-dns-in-godaddy)  
    - [Step 1: Register a Domain](#step-1-register-a-domain)
    - [Step 2: Access DNS Management on GoDaddy](#step-2-access-dns-management-on-godaddy)
7. [Test Your Setup](#7-test-your-setup)  
8. [Frequently Asked Questions (FAQs)](#8-frequently-asked-questions-faqs)  
9. [Contact Information](#9-contact-information)  
10. [References](#10-references)


---

## 1. Introduction

This document provides a comprehensive overview for beginners to set up DNS for their website or application using GoDaddy as the domain and DNS provider.

---

## 2. What is DNS?

**DNS (Domain Name System)** is like the phonebook of the internet. DNS servers maintain a database of public IP addresses and their associated hostnames. The primary function of DNS servers is to translate domain names (easy for people to remember) into IP addresses (used by computers to connect).  
For example, when you enter `www.example.com` into your browser, a DNS server translates that into an IP address like `192.0.2.1`.

---

## 3. Why Do You Need DNS?

- Lets users access your website/app using a memorable domain name.
- Connects your domain name to your server or hosting provider.
- Enables email, subdomains, and other services.

---

## 4. Prerequisites

- A GoDaddy account ([Sign up here](https://www.godaddy.com/)).
- A purchased domain name in your GoDaddy account.
- The IP address or hostname of your web server or hosting provider.

---

## 5. DNS Record Types 

DNS (Domain Name System) records are instructions stored on DNS servers that tell the internet how to route your domain’s traffic. Most commonly used DNS record types:


| Record Type | Name                                  | Purpose                                                                                   |
| ----------- | ------------------------------------- | ----------------------------------------------------------------------------------------- |
| A           | Address Record                        | Maps a domain to an IPv4 address (e.g., example.com → 192.0.2.1).                         |
| AAAA        | IPv6 Address Record                   | Maps a domain to an IPv6 address (e.g., example.com → ::1).                               |
| CNAME       | Canonical Name                        | Maps a domain to another domain (alias). Useful for subdomains.                           |
| MX          | Mail Exchange                         | Specifies mail servers for the domain to handle email.                                    |
| TXT         | Text Record                           | Stores arbitrary text data. Commonly used for SPF, DKIM, and verification.                |
| NS          | Name Server                           | Specifies the authoritative DNS servers for the domain.                                   |

## Examples Explained

- **A Record:**  
  If you want `yourdomain.com` to show your website hosted on a server with IP `123.45.67.89`, add an A record pointing to that IP.

- **CNAME Record:**  
  If you use a service like GitHub Pages or Heroku, you might point `blog.yourdomain.com` to `username.github.io` with a CNAME.

- **MX Record:**  
  To receive emails at `yourname@yourdomain.com`, set up MX records as instructed by your email provider.
---


## 6. Configuring DNS in GoDaddy

### Step 1: Register a Domain

- Follow the [link](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/SCRUM-110-divya-mishra/Domain-Security/DNS/POC/README.md#61-buy-a-domain-from-godaddy)

---

### Step 2: Access DNS Management on GoDaddy

- Follow the [link](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/SCRUM-110-divya-mishra/Domain-Security/DNS/POC/README.md#63-configure-dns-records-via-godaddy)


- DNS changes may take anywhere from a few minutes to 48 hours to update across the internet.

---

## 7. Test Your Setup

- Use [DNS Checker](https://dnschecker.org/) to verify your records.
- Open your domain in a browser to check if it loads your site.
- Use commands like:
  ```bash
  nslookup yourdomain.com
  dig www.yourdomain.com
  ```
<img width="475" height="188" alt="Screenshot 2025-08-05 at 6 55 40 PM" src="https://github.com/user-attachments/assets/2cd096e8-aa51-4729-b3a8-1776a91d152b" />

---

## 8. Frequently Asked Questions (FAQs)

### 1. How long does DNS propagation take?
It can be immediate, but sometimes up to 48 hours worldwide.

### 2. Can I change my DNS records anytime?
Yes, but remember each change may require propagation time.

### 3. What is the difference between A and CNAME records?
A records point to IP addresses, CNAME records point to domain names.

### 4. How do I set up email for my domain?
Add the MX records provided by your email service (e.g., Google Workspace, Microsoft 365).

---

## 9. Contact Information

| Name           | Email address                           |
|----------------|----------------------------------------|
| Meenu Chauhan  | meenu.chauhan.snaatak@mygurukulam.co   |

---

## 10. References

| Reference               | Link                                                                           |
|-------------------------|--------------------------------------------------------------------------------|
| DNS Configure         | [Click here](https://www.godaddy.com/resources/skills/configuring-and-working-with-domains-dns) |
| DNS Checker Tool           | [Click here](https://dnschecker.org/)           |


