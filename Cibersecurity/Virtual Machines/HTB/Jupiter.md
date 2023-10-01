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

