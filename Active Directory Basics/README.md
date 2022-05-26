# Active Directory Basics

> Evan Diamantidis | May 25th, 2022

--------------------------

<br />

# 1. Introduction
	
1.1: I understand what Active Directory is and why it is used.
```
No answer needed
```

<br />
<br />

# 2. Physical Active Directory
	
2.1: What database does the AD DS contain?
```
NTDS.dit
```
2.2: Where is the NTDS.dit stored?
```
%systemroot%\NTDS
```
2.3: What type of machine can be a domain controller?
```
Windows server
```

<br />
<br />

# 3. The Forest

3.1: What is the term for a hierarchy of domains in a network?
```
Tree
```
3.2: What is the term for the rules for object creation?
```
Domain schema
```
3.3: What is the term for containers for groups, computers, users, printers, and other OUs?
```
Organizational unit
```
<br />
<br />

# 4. Users + Groups

4.1: Which type of groups specify user permissions?
```
Security groups
```
4.2: Which group contains all workstations and servers joined to the domain?
```
Domain computers
```
4.3: Which group can publish certificates to the directory?
```
Cert publishers
```
4.4: Which user can make changes to a local machine but not to a domain controller?
```
Local administrator
```
4.5: Which group has their passwords replicated to read-only domain controllers?
```
Allowed RODC Password Replication Group
```
<br />
<br />

# 5. Trusts + Policies

5.1: What type of trust flows from a trusting domain to a trusted domain?
```
Directional
```
5.2: What type of trusts expands to include other trusted domains?
```
Transitive
```
<br />
<br />

# 6. Active Directory Domain Services + Authentication 

6.1: What type of authentication uses tickets?
```
Kerberos
```
6.2: What domain service can create, validate, and revoke public key certificates?
```
Certificate services
```
<br />
<br />

# 7. AD in the Cloud

7.1: What is the Azure AD equivalent of LDAP?
```
Rest APIs
```
7.2: What is the Azure AD equivalent of Domains and Forests?
```
Tenants
```
7.3: What is the Windows Server AD equivalent of Guests?
```
Trusts
```
<br />
<br />

# 8. Hands-On Lab

a) Deploy the Machine
<br />

b) SSH or RDP into the machine (or use the browser-based instance)
<br />
<br />

Username: Administrator
<br />

Password: password123@
<br />

Domain: CONTROLLER.local
<br />
<br />

8.1: Deploy the Machine
<br />
<br />
8.2: What is the name of the Windows 10 operating system?
```
Get-NetComputer -fulldata | select operatingsystem
```
![screenshot](https://user-images.githubusercontent.com/14150485/170381642-095138c0-730e-470b-a926-8562a7eddbe7.png)
```
Windows 10 Enterprise Evaluation
```
8.3: What is the second "Admin" name?
```
Get-ADUser -Filter {samAccountName -like '*admin*'} | Select samAccountName
```
![image](https://user-images.githubusercontent.com/14150485/170381931-fc1e8aa9-2ad5-491c-8a92-6d803fccdd4a.png)
```
Admin2
```
8.4: Which group has a capital "V" in the group name?
```
Get-ADGroup -Filter {samAccountName -like '*V*'} | Select samAccountName
```
![image](https://user-images.githubusercontent.com/14150485/170382267-2b43fee7-8a38-43e0-b51c-5d4ad7ad18e3.png)
```
Hyper-V Administrators
```
8.5: When was the password last set for the SQLService user?
```
Get-ADUser -Identity SQLService -Properties * | Select PasswordLastSet
```
![image](https://user-images.githubusercontent.com/14150485/170382503-59715782-8e61-4277-ad31-f777f11f2677.png)
```
5/13/2020 8:26:58 PM
```
<br />
<br />

# 9. Conclusion

9.1: I understand the basics of Active Directory
```
No answer needed
```
