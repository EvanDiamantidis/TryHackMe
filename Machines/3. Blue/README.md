# **[Blue](https://tryhackme.com/room/blue)**

<br />

> Evan Diamantidis | May 30th, 2022

<br />

# Table of contents

### [1. Enumeration](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/3.%20Blue#1-enumeration-1)

### [2. Access](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/3.%20Blue#2-access-1)

### [3. Escalate](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/3.%20Blue#3-escalate-1)

### [4. Cracking](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/3.%20Blue#4-cracking-1)

### [5. Find flags!](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/3.%20Blue#5-find-flags-1)

<br />
<br />

## 1. Enumeration

<br />

Let's gather some information about our target. For the purposes of this box, since task 1.3 is asking *what this machine is vulnerable to*, we will be adding the `-sS --script=vuln` argument to our `nmap` scan:

```
# Nmap 7.92 scan initiated Mon May 30 07:24:31 2022 as: nmap -sC -sV -sS --script=vuln -oN nmap/initial 10.10.239.152
Nmap scan report for 10.10.239.152
Host is up (0.015s latency).
Not shown: 991 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  tcpwrapped
|_ssl-ccs-injection: No reply from server (TIMEOUT)
| rdp-vuln-ms12-020: 
|   VULNERABLE:
|   MS12-020 Remote Desktop Protocol Denial Of Service Vulnerability
|     State: VULNERABLE
|     IDs:  CVE:CVE-2012-0152
|     Risk factor: Medium  CVSSv2: 4.3 (MEDIUM) (AV:N/AC:M/Au:N/C:N/I:N/A:P)
|           Remote Desktop Protocol vulnerability that could allow remote attackers to cause a denial of service.
|           
|     Disclosure date: 2012-03-13
|     References:
|       http://technet.microsoft.com/en-us/security/bulletin/ms12-020
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0152
|   
|   MS12-020 Remote Desktop Protocol Remote Code Execution Vulnerability
|     State: VULNERABLE
|     IDs:  CVE:CVE-2012-0002
|     Risk factor: High  CVSSv2: 9.3 (HIGH) (AV:N/AC:M/Au:N/C:C/I:C/A:C)
|           Remote Desktop Protocol vulnerability that could allow remote attackers to execute arbitrary code on the targeted system.
|           
|     Disclosure date: 2012-03-13
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0002
|_      http://technet.microsoft.com/en-us/security/bulletin/ms12-020
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49158/tcp open  msrpc        Microsoft Windows RPC
49160/tcp open  msrpc        Microsoft Windows RPC
Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: NT_STATUS_ACCESS_DENIED
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|_      https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon May 30 07:26:11 2022 -- 1 IP address (1 host up) scanned in 100.47 seconds
```

<br />
<br />

## 2. Access

<br />	

Searching Metasploit for the exploit the machine is vulnerable to will show the relevant modules suited for our attack.

![image](https://user-images.githubusercontent.com/14150485/171114110-e0c005f4-9378-4351-a843-6a417c0e65b6.png)

<br />

The `EternalBlue` module we will be using for the purposes of this machine is a well-known SMB exploit also used in various other TryHackMe rooms.

As mentioned in the task description, using the `set payload windows/x64/shell/reverse_tcp` command is optional, however anything that benefits us in our learning process is always welcome!

Having said that, my personal experience using this payload with Blue was problematic due to multiple sessions that interfered both with my console, as well as my overall bandwidth, so feel free to skip this in case you run into similar issues.

Let us proceed with updating the module options with the appropriate values and run the exploit to gain access to the target machine.

![image](https://user-images.githubusercontent.com/14150485/171119152-d06c8d10-5932-483b-9dde-5b8150205f7b.png)

<br />
<br />

## **3. Escalate**

<br />

We push the current shell to the background using `Ctrl + Z` so we can use the `shell_to_meterpreter` module hinted on the question.

![image](https://user-images.githubusercontent.com/14150485/171119350-819f41db-0d4b-40fc-8165-3cc20900b5af.png)

<br />

Next step is to update the `SESSION` value with the background session ID before executing this module.

![image](https://user-images.githubusercontent.com/14150485/171120026-1779fb73-d2ae-4066-8487-fa8f5e69403d.png)

<br />

Upon completion we receive a prompt informing us that the module was successfully executed.

![image](https://user-images.githubusercontent.com/14150485/171120275-7833ac39-45d4-423c-a0e8-a788b87beeb3.png)

<br />

Next up we switch to the newly created session and use the `shell` and `whoami` commands to view our privileges on the target system, as instructed on the task.

![image](https://user-images.githubusercontent.com/14150485/171121162-aef02aea-2cf5-4878-9171-75042643ab5d.png)

<br />

To view the list of running processes on the target we should push the Windows prompt in the background once again using `Ctrl + Z` and run the `ps` command on the Meterpreter shell.

![image](https://user-images.githubusercontent.com/14150485/171121665-fb781d18-eee0-472d-a18b-fa4480b4ba4a.png)

<br />

In this step we will be switching over to an `NT AUTHORITY\SYSTEM` process from the list using the `migrate PID` command, however please note this might take a few attempts before successful migration.

![image](https://user-images.githubusercontent.com/14150485/171121728-f81e6336-fda7-461d-8220-9eb654018fc1.png)

<br />
<br />

## **4. Cracking**

<br />

Following the instructions on the task, using the `hashdump` command in Meterpreter will list all known user hashes on the target machine.

![image](https://user-images.githubusercontent.com/14150485/173420036-8cec8dcb-388f-4e2f-857d-3279bdf5a88f.png)

<br />

There are 3 users listed along with their password hashes, one of which appears to be non-default.

Since this is a Windows machine, we can expect to see NT hashes (also known as NTLM). We can confirm this by checking the hash against a hash identifier. My personal favorite is https://hashes.com/en/tools/hash_identifier, however feel free to use your application of choice.

![image](https://user-images.githubusercontent.com/14150485/173413609-11f99292-4422-4cee-9157-c99b8a19e873.png)

<br />

Now that we have confirmed the hash type we can easily proceed to crack it using John The Ripper. To do so, create a file at a location of your choice and paste the hash into it. Then let John do what he does best!

![image](https://user-images.githubusercontent.com/14150485/173413855-5d3356bc-7ccf-4a9f-8b76-8711e12ac325.png)

<br />
<br />

## **5. Find flags!**

<br />

This should be an easy one - The root folder on Windows systems is `C:\`, as also subtly hinted on the question.

![image](https://user-images.githubusercontent.com/14150485/171124609-030be4c9-1e72-44db-93a8-f1bfe75e6a9a.png)

<br />

The `flag1.txt` is indeed placed under this folder and contains the answer to the first question.

![image](https://user-images.githubusercontent.com/14150485/173416426-758b163a-0c81-4b75-a622-e23a95f8c216.png)

<br />

Moving on to the next one, local user account details on Windows systems are stored under the `C:\Windows\system32\config\` directory. 

![image](https://user-images.githubusercontent.com/14150485/171125148-2b4d2599-edbf-4fbd-b716-6b6f42ac63cf.png)

<br />

Navigating to this location we confirm that `flag2.txt` is located in this directory and that its contents answer the second question.

![image](https://user-images.githubusercontent.com/14150485/173417083-704c86c0-64f2-494e-b192-eda77d982509.png)

<br />

There are multiple ways of varying complexity that we can employ to approach the final task.

The question is hinting us at *Administrators* and *things* they save. As we saw in another task earlier, there are 3 accounts on this machine. Navigating to the `C:\Users\` folder, however, we only see one listed along with default accounts, such as `All Users`, `Default`, `Public`, etc.

![image](https://user-images.githubusercontent.com/14150485/173415291-0781cd4f-caad-4979-a0ce-61ee698e87c1.png)

<br />

Interesting, as this means we are most likely to find something in there! 
<br />
*Administrators usually have pretty interesting things saved*, and it just so happens that we are looking for a flag which, more often than not, is a string saved in a text file. This should prompt us to have a look around, but mainly under `Documents` and see if we can find anything useful.

![image](https://user-images.githubusercontent.com/14150485/173416014-327e1a25-c6ae-40e0-973c-cd2b42d1ba97.png)

<br />

The `flag3.txt` file is in here, indeed!

![image](https://user-images.githubusercontent.com/14150485/173416288-86c9effc-06f7-4bb7-beed-28efeda98470.png)

<br />

We just pwned Blue!

<br />
<br />

# *** **SPOILER** ***

<br />

There is a way we can actually get all the flag file paths using a single command, simply by searching the machine for any text files that include the word `flag` in the file name.

![image](https://user-images.githubusercontent.com/14150485/173417608-27d527f9-357b-4833-8c33-f2236efa9c01.png)

<br />

Although this is the most efficient solution to the challenge, it might be counter-productive to the process - Reason being that we should ideally aim to conduct our own research based on the hints provided for each flag as part of our learning process around the use of Metasploit, Meterpreter, shell and session switching, as well as Windows filesystems in general. That said, knowing how and when to take advantage of Meterpreter commands is essential.

<br />

