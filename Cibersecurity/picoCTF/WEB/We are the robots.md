First step, we have a website telling , where are the robots?

http://jupiter.challenges.picoctf.org:60915/

Second step, you must try to find for the robots file on /robots.txt


http://jupiter.challenges.picoctf.org:60915/robots.txt

And we see this : 

```
|User-agent: *|
||Disallow: /8028f.html|
```

and now when you go to  http://jupiter.callenges.picoctf.org/8028f.html you will be able to see the flag

```
Guess you found the robots  
picoCTF{**************************}
```