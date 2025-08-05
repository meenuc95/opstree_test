# DNS Implementation 

| Author         | Created on   | Version | Last Updated On | Reviewed By     |
|----------------|--------------|---------|-----------------|-----------------|
| Meenu Chauhan  | 05-08-2025   | V1.0     | 05-08-2025      | -               |

---

## Table of Contents

1. [Introduction](#introduction)  
2. [What is DNS?](#what-is-dns)  
3. [Why Do You Need DNS?](#why-do-you-need-dns)  
4. [Prerequisites](#prerequisites)  
5. [Configuring DNS in GoDaddy](#configuring-dns-in-godaddy)  
6. [Save and Wait for Propagation](#save-and-wait-for-propagation)  
7. [Test Your Setup](#test-your-setup)  
8. [FAQs](#frequently-asked-questions-faqs)  
9. [Contact Information](#contact-information)  
10. [References](#references)

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


## 5. Configuring DNS in GoDaddy

### Step 1: Register & Access DNS Management on GoDaddy

- Log in to your GoDaddy account.
- Go to "My Products".
- Select your domain and click "DNS" or "Manage DNS".

---

### Step 2: Add DNS Records

#### A. Add an A Record (Points domain to server IP)
1. In the DNS Records section, click "Add" and select "A" from the Type dropdown.
2. Set "Host" to `@` (for the root domain) or `www` (for a subdomain).
3. Enter your server’s IP address in "Points to".
4. Save the record.

<img width="1347" height="396" alt="Screenshot 2025-08-05 at 11 55 14 AM" src="https://github.com/user-attachments/assets/09ab74b8-5b3e-40bb-9d55-13a45a6cb21d" />


## 6. Save and Wait for Propagation

- Click "Save" after adding or editing each record.
- DNS changes may take anywhere from a few minutes to 48 hours to update across the internet.

<img width="1354" height="326" alt="Screenshot 2025-08-05 at 11 55 20 AM" src="https://github.com/user-attachments/assets/ce120758-7565-4fee-9dcf-81f24e58ea21" />

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


