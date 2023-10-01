
*sudo nmap -p- -sV -sC -sS -vvv -n -Pn --min-rate=5000 192.168.100.24 -oN scan*

-p- (scans all ports) , you can also specify some or  a range ex: 1-1023
-sV (detects the version of the services)
-sC(charges the scripts (exploits availables (??)))
-sS(TCP SYN scan , envia paquetes y espera respuesta, no abre conexion TCP completa, si no hay respuesta significa que el puerto esta filtrado)
-Pn(Omites the host discovery , we already know that)
--min-rate=5000()
-vvv (verbose, shows the results while discovers the ports)
-oN scan (creates an archive called scan and put the discovers on a text archive)

SEGUN EL PINGUINO DE MARIO 
-p-  --open escanea puertos abiertos
-sV LA VERSION DE LOS SERVICIOS
-sS SYN SCAN mas sigiloso.
-min-rate 5000 para que valla mas rapido
-vvv para que reporte lo que va sacando
-n para evitar que haga resolucion DNS
-Pn evitar que haga PING en el ip

**BUSCA UN SCRIPT EN NMAP **

**ls /usr/share/nmap/scripts/http-en* **
and the route is in *usr/share/nmap/scripts/http-enum.nse*
![[Cibersecurity/IMAGES/scriptsnmap1.png]]

**Y LO ENVIAS ASI;**
**nmap --script=http-enum.nse -p 80 192.168.100.28 -oN nmap-http-enum.txt**
![[SCRIPTNMAP2.png]]

-T4-  Its a speed of the attack
-p 80,22,121,433,10000      select which port we  want to investigate

-oG  (en formate greepeable)