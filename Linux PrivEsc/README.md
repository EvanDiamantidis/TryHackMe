# **Linux PrivEsc**

> Evan Diamantidis | June 2nd, 2022

--------------------------

<br />

## 1. Deploy the Vulnerable Debian VM

### 1.1: Deploy the machine and login to the "user" account using SSH.
```
No answer needed
```

Let's gather some information about our target with nmap. The "initial" output file under the "nmap" folder shows the following:
```
# Nmap 7.92 scan initiated Thu Jun  2 09:00:07 2022 as: nmap -sC -sV -oN nmap/initial 10.10.248.34
Nmap scan report for 10.10.248.34
Host is up (0.0087s latency).
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 5.5p1 Debian 6+squeeze5 (protocol 2.0)
| ssh-hostkey: 
|   1024 a4:6c:d1:c8:5b:03:f2:af:33:3f:84:15:cf:15:ed:ba (DSA)
|_  2048 08:84:3e:96:4d:9a:2f:a1:db:be:68:29:80:ab:f3:56 (RSA)
25/tcp   open  smtp    Exim smtpd 4.84
| smtp-commands: debian.localdomain Hello ip-10-8-76-102.eu-west-1.compute.internal [10.8.76.102], SIZE 52428800, 8BITMIME, PIPELINING, HELP
|_ Commands supported: AUTH HELO EHLO MAIL RCPT DATA NOOP QUIT RSET HELP
80/tcp   open  http    Apache httpd 2.2.16 ((Debian))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.2.16 (Debian)
111/tcp  open  rpcbind 2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/udp   nfs
|   100005  1,2,3      34933/udp   mountd
|   100005  1,2,3      58193/tcp   mountd
|   100021  1,3,4      49798/udp   nlockmgr
|   100021  1,3,4      50245/tcp   nlockmgr
|   100024  1          49563/tcp   status
|_  100024  1          52565/udp   status
2049/tcp open  nfs     2-4 (RPC #100003)
8080/tcp open  http    nginx 1.6.2
|_http-title: Welcome to nginx on Debian!
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: nginx/1.6.2
Service Info: Host: debian.localdomain; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Jun  2 09:00:16 2022 -- 1 IP address (1 host up) scanned in 8.28 seconds
```

<br />

The credentials we will be using to connect via SSH are:
```
Username: user
```
```
Password: password321
```
<br />

### 1.2: Run the "id" command. What is the result?
```
id
```

<br />

### 1.3: What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)
```
MS17-010
```

<br />
<br />

## 2. Gain Access
	
### 2.1: Start Metasploit
```
No answer needed
```

<br />

### 2.2: Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........)

<br />

Searching Metasploit for "MS17-010" will show the relevant modules we could use to attack the machine.
```
search MS17-010
```
![image](https://user-images.githubusercontent.com/14150485/171114110-e0c005f4-9378-4351-a843-6a417c0e65b6.png)

EternalBlue is a well-known SMB exploit which we have also used in other rooms:
```
use 0
```
Altenatively:
```
use exploit/windows/smb/ms17_010_eternalblue
```
The answer is therefore:
```
exploit/windows/smb/ms17_010_eternalblue
```

<br />

