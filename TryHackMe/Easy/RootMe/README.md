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

