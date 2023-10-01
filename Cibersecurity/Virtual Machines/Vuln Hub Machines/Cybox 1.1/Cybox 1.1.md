The first step is always create a folder where we are going to work, scan with arp-scan and then send the scan with nmap

sudo nmap -sS -sC -sV -n -p- -Pn -min-rate 5000 -vvv 192.168.0.26 -oN cybox1.1 

this is the folder of the scan /Documentos/Workspace/Virtual machines/pentesting/Cybox1.1/cybox1.1

Aparently allways we start searching on the servers , like apache and all this stuffs 

In some ocasions when its open the port in an apache server for example (port 80 or any other with http) and other with ssl open port (445 for example an smb with https) they can give us a different results , two pages completly different

The  source code of the page is not interesting

When we have a static page the best way is make a search of directories 

![[Cibersecurity/Virtual Machines/Vuln Hub Machines/Cybox 1.1/IMAGES/a.png]]
we search a word list called 2.3-m 
and then we use the tool called #dirsearch 
```
python3 dirsearch.py -u http://192.168.0.33 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 150 -e php,txt,html -f
```
**LOOK THAT IS DIFFERENT , IT IS NOT ONLY DIRSEARCH LIKE THE PICTURE**

Ok, now its discovering things. In the website of apache we can see that there is a interesting information about an e mail , it uses cybox.company
![[MAIL.png]]
we are going to put this in our etc/host folder
We are telling our computer that when it goes to the DNS cybox.company it resolves it with the ip 192.168.100.33
![[nanohosts.png]]

It discovered a 192.168.100.33/phpmyadmin (BUT IT ONLY WORKS ON LOCAL NETWORK AND WE ARE NOT ON THE NETWORK OF CYBOX APARENTLY)

While its working dirsearch we are going to search subdomains of cybox.company with #dirbuster 
```
	gobuster vhost --append-domain -u cybox.company -w /usr/share/dnsrecon/subdomains-top1mil-5000.txt | tee subdomines.txt
```
With this command i get this sub domains :
![[append-domain.png]]

cat subdomines.txt | cut -d" " -f2 . WITH THIS WAY THE ARCHIVE IS EDITED
and we aded it to the /etc/hosts
![[subdominios.png]]
We need to investigate this sub domains with HTTP AND HTTPS

we find on the webmail.cybox an exploit for squirrelmail , the version used is 1.4.22
```
searchsploit squirrelmail
```
![[searchsploit.png]]
unfortunately the machine cannot be exploited by this way (that is what the guy said)

We created a user on this page *register.cybox.company*
![[user.png]]
Now we can log in the squirrelmail
![[pepe.png]]
We can log on the ftp server to

![[ftpserver.png]]
we are loged on the monitor.cybox.company
its a control of in and out of employees.
The guy said, that it is not so important because on this web app we cannot do much things. When you investigate the petitions there are only "GET" so we cant manipulate much on this.
![[monitor.png]]

We can actualize our password on the site, aparently this is relevant
![[reset password.png]]
![[Cibersecurity/Virtual Machines/Vuln Hub Machines/Cybox 1.1/IMAGES/password.png]]

we can modify the link , and change the administrator passowrd, because the URL its very explicit on the content 
![[adminpassword.png]]

As administrators when we log on the app we have the additional option of "admin panel"

![[adminpanel.png]]
There are no much interesting things except  the "style as this guy said" because its importing it with php, wit the option "include"
![[adminpanel2.png]]
We see that is importing a style but we really dont know where it is being imported
![[styles.png]]
We will use dirsearch and maybe we are going to find some interesting directory
![[dirsearchh.png]]
Dirsearch send us styles
![[styyyyl.png]]
http://monitor.cybox.company/admin/styles/general.css 
THe guy says that in the first is specifying the extension.css he said, "la estara añadiendo en el backend ?"

if we want to see another folder with another extension how do we do it ?

http://monitor.cybox.company/admin/styles.php?style=general
HE said that we have the nullbite.. and he started to search directorys in the search bar
http://monitor.cybox.company/admin/styles.php?style=%00
![[nullbite.png]]

He said in this way it isnt work =../../../etc/passwd
we need to add the nullbite =../../../etc/passwd%00 "TO elude the extension"
"ESTA LEYENDO TODO LO QUE ESTA A LA DERECHA, PERO SI PONEMOS EL NULLBITE TODO LO QUE ESTA A LA DERECHA SE LO VA A SALETEAR"

AND NOW IT WORKED
![[asdqwd.png]]


FIRST FLAG ON THE /home/cybox/user.txt

![[first flag.png]]

but we are going to go for a SHELL

php page phpinfo.php
![[dev.png]]

there is a vulnerability LFI on this php

![[lfi..png]]

its configured in LAMP 
Linux Apache MySQL and PHP
/opt/bitnami/php/etc/php.ini   <-----bitnami is configured on this
/bitnami/lampstack-linux/output/php/lib

IF IT IS CONFIGURED IN BITNAMY IT MUST HAVE A FOLDER OF LOGS
 on de route /var/log/apache2/access.log is not in this route, is on the route bitnami
 the guy say the route of bitnami folder is on google 
( WE ARE GOING TO DO A "POISON LOG ")
 ![[bitnami.png]]
 
 LA ABANDONE POR AHORA
 