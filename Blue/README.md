# Blue

> Evan Diamantidis | May 27th, 2022

--------------------------

<br />

# 1. Introduction to Meterpreter

1.1: Scan the machine. (If you are unsure how to tackle this, I recommend checking out the Nmap room)

Scanning with nmap to gather information about our target. The output of the "initial" scan created under the "nmap" folder:
```
# Nmap 7.92 scan initiated Fri May 27 16:56:27 2022 as: nmap -sC -sV -oN nmap/initial 10.10.233.71
Nmap scan report for 10.10.233.71
Host is up (0.012s latency).
Not shown: 991 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  tcpwrapped
| ssl-cert: Subject: commonName=Jon-PC
| Not valid before: 2022-05-26T15:54:24
|_Not valid after:  2022-11-25T15:54:24
|_ssl-date: 2022-05-27T15:57:46+00:00; +3s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: JON-PC
|   NetBIOS_Domain_Name: JON-PC
|   NetBIOS_Computer_Name: JON-PC
|   DNS_Domain_Name: Jon-PC
|   DNS_Computer_Name: Jon-PC
|   Product_Version: 6.1.7601
|_  System_Time: 2022-05-27T15:57:31+00:00
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49158/tcp open  msrpc        Microsoft Windows RPC
49160/tcp open  msrpc        Microsoft Windows RPC
Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h00m02s, deviation: 2h14m10s, median: 1s
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_nbstat: NetBIOS name: JON-PC, NetBIOS user: <unknown>, NetBIOS MAC: 02:b1:26:ee:b5:6b (unknown)
| smb2-time: 
|   date: 2022-05-27T15:57:31
|_  start_date: 2022-05-27T15:54:23
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Jon-PC
|   NetBIOS computer name: JON-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2022-05-27T10:57:31-05:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri May 27 16:57:43 2022 -- 1 IP address (1 host up) scanned in 76.15 seconds
```

```
No answer needed
```

1.2: How many ports are open with a port number under 1000?
```
3
```

1.3: What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)
```

```

<br />
<br />

# 2. Meterpreter Flavors
	
2.1: No answer needed.

```
No answer needed
```
<br />
<br />

# 3. Meterpreter Commands
