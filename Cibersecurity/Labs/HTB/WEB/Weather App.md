
On this challenge you must download files first and you have a link too where you can see the application working.

You need docker to run it, if you don't know how to install it on kali follow this steps

```
kali@kali:~$ sudo apt update kali@kali:~$ kali@kali:~$ sudo apt install -y docker.io kali@kali:~$ kali@kali:~$ sudo systemctl enable docker --now kali@kali:~$ kali@kali:~$ docker kali@kali:~$
```


Now run it 
```
./build-docker.sh
```


Now I can see the docker container running on http://0.0.0.0:1337/

If you're no sure where they are. You can see the docker containers working on with

```
docker ps -a
```
The output:
```
┌──(cds㉿kali)-[~/Documents/htb/weather lab/web_weather_app]
└─$ sudo docker ps -a
[sudo] password for cds: 
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS          PORTS                                   NAMES
0896e52853bc   weather_app   "/usr/bin/supervisor…"   11 minutes ago   Up 11 minutes   0.0.0.0:1337->80/tcp, :::1337->80/tcp   weather_app

```
At some point I think maybe it is not necessary that you start the docker container, the app works perfectly online. http://134.209.188.158:30650


This is an app that gives you the weather 

![[Screenshot_2023-09-17_11_33_30.png]]

If you investigate on on the files that you downloaded you can see that you have a login site and register site.

![[Screenshot_2023-09-17_11_33_11.png]]

I was trying to log in with the classic 'or 1=1 -- - and when I log in i recive a message "you're not admin" 

and when I put just a colon (') I get this error

```
ReferenceError: re is not defined  
    at router.post (/app/routes/index.js:52:2)  
    at Layer.handle [as handle_request] (/app/node_modules/express/lib/router/layer.js:95:5)  
    at next (/app/node_modules/express/lib/router/route.js:137:13)  
    at Route.dispatch (/app/node_modules/express/lib/router/route.js:112:3)  
    at Layer.handle [as handle_request] (/app/node_modules/express/lib/router/layer.js:95:5)  
    at /app/node_modules/express/lib/router/index.js:281:22  
    at Function.process_params (/app/node_modules/express/lib/router/index.js:335:12)  
    at next (/app/node_modules/express/lib/router/index.js:275:10)  
    at Function.handle (/app/node_modules/express/lib/router/index.js:174:3)  
    at router (/app/node_modules/express/lib/router/index.js:47:12)
```

When I put anithing I can log in, but im not admin. I can log putting whatever on the input