# Active Directory Basics

> Evan Diamantidis | May 25th, 2022

--------------------------

<br />

# Table of contents

### [1. Introduction](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#1-introduction-1)
#### [&nbsp; 1.1: I understand what Active Directory is and why it is used.](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#11-i-understand-what-active-directory-is-and-why-it-is-used)

<br />

### [2. Physical Active Directory](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#2-physical-active-directory)
#### [&nbsp; 2.1: What database does the AD DS contain?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#21-what-database-does-the-ad-ds-contain)
#### [&nbsp; 2.2: Where is the NTDS.dit stored?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#22-where-is-the-ntdsdit-stored)
#### [&nbsp; 2.3: What type of machine can be a domain controller?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#23-what-type-of-machine-can-be-a-domain-controller)

<br />

### [3. The Forest](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#3-the-forest-1)
#### [&nbsp; 3.1: What is the term for a hierarchy of domains in a network?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#31-what-is-the-term-for-a-hierarchy-of-domains-in-a-network)
#### [&nbsp; 3.2: What is the term for the rules for object creation?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#32-what-is-the-term-for-the-rules-for-object-creation)
#### [&nbsp; 3.3: What is the term for containers for groups, computers, users, printers, and other OUs?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#33-what-is-the-term-for-containers-for-groups-computers-users-printers-and-other-ous)

<br />

### [4. Users + Groups](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#4-users--groups-1)
#### [&nbsp; 4.1: Which type of groups specify user permissions?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#41-which-type-of-groups-specify-user-permissions)
#### [&nbsp; 4.2: Which group contains all workstations and servers joined to the domain?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#42-which-group-contains-all-workstations-and-servers-joined-to-the-domain) 
#### [&nbsp; 4.3: Which group can publish certificates to the directory?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#43-which-group-can-publish-certificates-to-the-directory) 
#### [&nbsp; 4.4: Which user can make changes to a local machine but not to a domain controller?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#44-which-user-can-make-changes-to-a-local-machine-but-not-to-a-domain-controller) 
#### [&nbsp; 4.5: Which group has their passwords replicated to read-only domain controllers?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#45-which-group-has-their-passwords-replicated-to-read-only-domain-controllers) 

<br />

### [5. Trusts + Policies](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#5-trusts--policies-1)
#### [&nbsp; 5.1: What type of trust flows from a trusting domain to a trusted domain?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#51-what-type-of-trust-flows-from-a-trusting-domain-to-a-trusted-domain)
#### [&nbsp; 5.2: What type of trusts expands to include other trusted domains?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#52-what-type-of-trusts-expands-to-include-other-trusted-domains)

<br />

### [6. Active Directory Domain Services + Authentication](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#6-active-directory-domain-services--authentication-1)
#### [&nbsp; 6.1: What type of authentication uses tickets?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#61-what-type-of-authentication-uses-tickets)
#### [&nbsp; 6.2: What domain service can create, validate, and revoke public key certificates?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#62-what-domain-service-can-create-validate-and-revoke-public-key-certificates)

<br />

### [7. AD in the Cloud](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#7-ad-in-the-cloud-1)
#### [&nbsp; 7.1: What is the Azure AD equivalent of LDAP?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#71-what-is-the-azure-ad-equivalent-of-ldap)
#### [&nbsp; 7.2: What is the Azure AD equivalent of Domains and Forests?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#72-what-is-the-azure-ad-equivalent-of-domains-and-forests)
#### [&nbsp; 7.3: What is the Windows Server AD equivalent of Guests?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#73-what-is-the-windows-server-ad-equivalent-of-guests)

<br />

### [8. Hands-On Lab](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#8-hands-on-lab-1)
#### [&nbsp; 8.1: Deploy the Machine](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#81-deploy-the-machine)
#### [&nbsp; 8.2: What is the name of the Windows 10 operating system?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#82-what-is-the-name-of-the-windows-10-operating-system)
#### [&nbsp; 8.3: What is the second "Admin" name?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#83-what-is-the-second-admin-name)
#### [&nbsp; 8.4: Which group has a capital "V" in the group name?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#84-which-group-has-a-capital-v-in-the-group-name)
#### [&nbsp; 8.5: When was the password last set for the SQLService user?](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#85-when-was-the-password-last-set-for-the-sqlservice-user)

<br />

### [9. Conclusion](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#9-conclusion-1)
#### [&nbsp; 9.1: I understand the basics of Active Directory](https://github.com/EvanDiamantidis/TryHackMe/tree/main/Active%20Directory%20Basics#91-i-understand-the-basics-of-active-directory)


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
