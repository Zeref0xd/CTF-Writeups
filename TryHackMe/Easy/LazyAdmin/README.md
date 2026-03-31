# LazyAdmin

**Platform:** TryHackMe  
**Difficulty:** Easy  
**Category:** Red Team  

## Overview

Easy linux machine to practice your skills

## Enumeration

### Command Used
```bash
sudo nmap 10.48.135.56 -n -T4 -sC -sV -oN Nmap-Scan
```
### Output
```ansi
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-31 14:54 +0530
Nmap scan report for 10.48.135.56
Host is up (0.0077s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.52 seconds
```
### Analysis

Based on the Nmap results:

- Port 22 (SSH) → Possible brute-force or credential reuse
- Port 80 (HTTP) → Web application to enumerate

## Web Enumeration

Accessed:

- http://10.48.135.56/

## Directory & File Brute Forcing

### Command Used
```bash
sudo feroxbuster -u http://10.48.135.56/ -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt
```
### Found 
```ansi
http://10.48.135.56/content/

recursive

- http://10.48.135.56/content/js/
- http://10.48.135.56/content/inc/
- http://10.48.135.56/content/as/
```
/contentt/

![Sweet Rice CMS](screenshots/1.png)

/content/js/

![JS](screenshots/2.png)

/content/as/

![as](screenshots/3.png)

/content/inc/

![inc](screenshots/4.png)

A CMS is running at /Content called **Sweet Rice CMS**

at http://10.48.135.56/content/inc/mysql_backup/ we Found a mysql backup file

![Mysql](screenshots/5.png)

Download it.

## Credential Discovery

### Observation

A backup SQL file was discovered containing application configuration data.

---

### Extracted Credentials

```
Username: manager
Password Hash: 42f749ade7f9e195bf475f37a44cafcb
```

---

### Analysis

The password was stored as an MD5 hash and was cracked using a wordlist attack.

### Command Used
```bash
echo "42f749ade7f9e195bf475f37a44cafcb" > hash.txt
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```
### Result

```
Password: Password123

### Result

```
Username: manager
Password: Password123

### Conclusion

Valid administrative credentials were obtained, allowing access to the web application.

- Logged in with the creds at /content/as

![as admin](screenshots/6.png)


