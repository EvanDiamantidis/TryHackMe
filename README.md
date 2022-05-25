# Active Directory Basics

> Evan Diamantidis | May 25th, 2022


--------------------------


```
1. Introduction

	1.1: I understand what Active Directory is and why it is used.
```
```
2. Physical Active Directory

	2.1: What database does the AD DS contain?
	NTDS.dit

	2.2: Where is the NTDS.dit stored?
	%systemroot%\NTDS

	2.3: What type of machine can be a domain controller?
	Windows server
```
```
3. The Forest

	3.1: What is the term for a hierarchy of domains in a network?
	Tree

	3.2: What is the term for the rules for object creation?
	Domain schema

	3.3: What is the term for containers for groups, computers, users, printers, and other OUs?
	Organizational unit
```
```
4. Users + Groups

	4.1: Which type of groups specify user permissions?
	Security groups

	4.2: Which group contains all workstations and servers joined to the domain?
	Domain computers

	4.3: Which group can publish certificates to the directory?
	Cert publishers

	4.4: Which user can make changes to a local machine but not to a domain controller?
	Local administrator\

	4.5: Which group has their passwords replicated to read-only domain controllers?
	Allowed RODC Password Replication Group
```
```
5. Trusts + Policies

	5.1: What type of trust flows from a trusting domain to a trusted domain?
	Directional

	5.2: What type of trusts expands to include other trusted domains?
	Transitive
```
```
6. Active Directory Domain Services + Authentication 

	6.1: What type of authentication uses tickets?
	Kerberos

	6.2: What domain service can create, validate, and revoke public key certificates?
	Certificate services
```
```
7. AD in the Cloud

	7.1: What is the Azure AD equivalent of LDAP?
	Rest APIs

	7.2: What is the Azure AD equivalent of Domains and Forests?
	Tenants

	7.3: What is the Windows Server AD equivalent of Guests?
	Trusts
```
```
8. Hands-On Lab

	a) Deploy the Machine
	b) SSH or RDP into the machine (or use the browser-based instance)

	Username: Administrator
	Password: password123@
	Domain: CONTROLLER.local

	8.1: Deploy the Machine

	8.2: What is the name of the Windows 10 operating system?
	Get-NetComputer -fulldata | select operatingsystem
 
![Screenshot_2022-05-25_23-18-38](https://user-images.githubusercontent.com/14150485/170377475-6a3bee9f-b6ab-4d7f-8147-aa7075bba2f1.jpg)


	8.3: What is the second "Admin" name?
	


	Admin2
```
