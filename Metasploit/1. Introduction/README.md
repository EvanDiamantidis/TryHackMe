# Metasploit: Introduction 

> Evan Diamantidis | May 26th, 2022

--------------------------

<br />

# 1. Introduction to Metasploit
```
No answer needed
```

<br />
<br />

# 2. Main Components of Metasploit
	
2.1: What is the name of the code taking advantage of a flaw on the target system?
```
Exploit
```
2.2: What is the name of the code that runs on the target system to achieve the attacker's goal?
```
Payload
```
2.3: What are self-contained payloads called?
```
Singles
```
2.4: Is "windows/x64/pingback_reverse_tcp" among singles or staged payload?
```
Singles
```

<br />
<br />

# 3. Msfconsole

3.1: How would you search for a module related to Apache?
```
search apache
```

3.2: Who provided the auxiliary/scanner/ssh/ssh_login module?
```
info auxiliary/scanner/ssh/ssh_login
```
![image](https://user-images.githubusercontent.com/14150485/170449506-54bdbef2-7072-459a-ad5e-456ee73dabee.png)

```
todb
```

<br />
<br />

# 4. Working with modules

4.1: How would you set the LPORT value to 6666?
```
set LPORT 6666
```
4.2: How would you set the global value for RHOSTS  to 10.10.19.23 ?
```
setg RHOSTS 10.10.19.23
```
4.3: What command would you use to clear a set payload?
```
unset PAYLOAD
```
4.4: What command do you use to proceed with the exploitation phase?
```
exploit
```

<br />
<br />
