# Active Directory Basics

> Evan Diamantidis | May 25th, 2022

--------------------------

<br />

# Table of contents

### [1. Introduction](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#1-introduction-1)
#### [&nbsp; 1.1: I understand what Active Directory is and why it is used.]()

<br />

### [2. Physical Active Directoryn](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#2-physical-active-directory)
#### [&nbsp; 2.1: What database does the AD DS contain?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#21-what-database-does-the-ad-ds-contain)




<br />

## 1. Introduction
	
### 1.1: I understand what Active Directory is and why it is used.
```
No answer needed
```

<br />
<br />

## 2. Physical Active Directory
	
### 2.1: What database does the AD DS contain?
```
NTDS.dit
```
<br />

### 2.2: Where is the NTDS.dit stored?
```
%systemroot%\NTDS
```
<br />

### 2.3: What type of machine can be a domain controller?
```
Windows server
```

<br />
<br />

## 3. The Forest

### 3.1: What is the term for a hierarchy of domains in a network?
```
Tree
```
<br />

### 3.2: What is the term for the rules for object creation?
```
Domain schema
```
<br />

### 3.3: What is the term for containers for groups, computers, users, printers, and other OUs?
```
Organizational unit
```

<br />
<br />

## 4. Users + Groups

### 4.1: Which type of groups specify user permissions?
```
Security groups
```
<br />

### 4.2: Which group contains all workstations and servers joined to the domain?
```
Domain computers
```
<br />

### 4.3: Which group can publish certificates to the directory?
```
Cert publishers
```
<br />

### 4.4: Which user can make changes to a local machine but not to a domain controller?
```
Local administrator
```
<br />

### 4.5: Which group has their passwords replicated to read-only domain controllers?
```
Allowed RODC Password Replication Group
```

<br />
<br />

## 5. Trusts + Policies

### 5.1: What type of trust flows from a trusting domain to a trusted domain?
```
Directional
```
<br />

### 5.2: What type of trusts expands to include other trusted domains?
```
Transitive
```
<br />
<br />

## 6. Active Directory Domain Services + Authentication 

### 6.1: What type of authentication uses tickets?
```
Kerberos
```
<br />

### 6.2: What domain service can create, validate, and revoke public key certificates?
```
Certificate services
```

<br />
<br />

## 7. AD in the Cloud

### 7.1: What is the Azure AD equivalent of LDAP?
```
Rest APIs
```
<br />

### 7.2: What is the Azure AD equivalent of Domains and Forests?
```
Tenants
```
<br />

### 7.3: What is the Windows Server AD equivalent of Guests?
```
Trusts
```

<br />
<br />

## 8. Hands-On Lab
```
Username: Administrator
```
```
Password: password123@
```
<br />

### 8.1: Deploy the Machine
```
No answer needed
```

<br />

### 8.2: What is the name of the Windows 10 operating system?
```
Get-NetComputer -fulldata | select operatingsystem
```
![screenshot](https://user-images.githubusercontent.com/14150485/170381642-095138c0-730e-470b-a926-8562a7eddbe7.png)
```
Windows 10 Enterprise Evaluation
```
<br />

### 8.3: What is the second "Admin" name?
```
Get-ADUser -Filter {samAccountName -like '*admin*'} | Select samAccountName
```
![screenshot](https://user-images.githubusercontent.com/14150485/170381931-fc1e8aa9-2ad5-491c-8a92-6d803fccdd4a.png)
```
Admin2
```
<br />

### 8.4: Which group has a capital "V" in the group name?
```
Get-ADGroup -Filter * | Where-Object {$_.samAccountName -clike '*V*'} | Select samAccountName
```
![screenshot](https://user-images.githubusercontent.com/14150485/170454508-cf4c56ca-25cc-4400-856a-6c96c84419a9.png)
```
Hyper-V Administrators
```
<br />

### 8.5: When was the password last set for the SQLService user?
```
Get-ADUser -Identity SQLService -Properties * | Select PasswordLastSet
```
![screenshot](https://user-images.githubusercontent.com/14150485/170382503-59715782-8e61-4277-ad31-f777f11f2677.png)
```
5/13/2020 8:26:58 PM
```
<br />
<br />

## 9. Conclusion

### 9.1: I understand the basics of Active Directory
```
No answer needed
```
<br />
