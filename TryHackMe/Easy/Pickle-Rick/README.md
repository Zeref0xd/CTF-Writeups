# Pickle Rick

**Platform:** TryHackMe  
**Difficulty:** Easy  
**Category:** Red Team  

## Overview
A Rick and Morty CTF. Help turn Rick back into a human!

This Rick and Morty-themed challenge requires you to exploit a web server and find three ingredients to help Rick make his potion and transform himself back into a human from a pickle.

## Enumeration

### Command Used
```bash
sudo nmap 10.49.150.68 -n -T4 -sC -sV -oN Nmap-Scan
```
### Nmap Result
```ansi
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-26 12:32 +0530
Nmap scan report for 10.49.150.68
Host is up (0.0093s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 bb:28:b6:e9:bd:d5:8d:e0:dc:9c:56:d7:9f:ce:79:9e (RSA)
|   256 ab:58:07:1f:61:ac:70:25:7b:ed:77:ef:17:75:d4:d8 (ECDSA)
|_  256 03:22:df:aa:d3:4c:47:b8:04:7e:27:aa:93:e1:77:ea (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Rick is sup4r cool
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.80 seconds
```
### Analysis

Based on the Nmap results:

- Port 22 (SSH) → Possible brute-force or credential reuse
- Port 80 (HTTP) → Web application to enumerate

### Web Enumeration

Accessed:
- http://10.49.150.68/

### Homepage 

![Website Homepage](screenshots/1.png)

### Source Code 

![Website Homepage Source Code](screenshots/2.png)

