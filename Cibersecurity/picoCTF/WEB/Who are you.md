#### Description

Let me in. Let me iiiiiiinnnnnnnnnnnnnnnnnnnn


#### Hints

It ain't much, but it's an RFC [https://tools.ietf.org/html/rfc2616](https://tools.ietf.org/html/rfc2616)

### Process

We're in a website where we see a message 

```
Only people who use the official PicoBrowser are allowed on this site!
```

I had to intercept the request with burpsuit and edit the user agent with "PicoBrowser"

Then I recive another message from the website


`I don't trust users visiting from another site`

Now we must add another header "referer" and put on the parameter this
mercury.picoctf.net:52362 so, the result

`Referer: mercury.picoctf.net:523`


Now we have another message :

Sorry, this site only worked in 2018

On this opportunity we must add the "Date" header 

`Date: Fri, 26 Jan 2018 07:28:00 GMT`

We have another message once we send the request.

`Ì don't trust users who can be tracked`

So I put the header `DNT:1`


Now I can see another message:

`This website is ony for people from Sweden`

So I just search on google "lookup ip sweden" and I add one ip from sweden and I add the X-Forwarded-For header

`X-Forwarded-For: 192.44.242.19`

And the last message:

`You're in Sweden but you don't speak Sedish?`

And the last thing that here we need to do is change the Accept-Language header.

`Accept-Language: sv,en;q=0.9`

Finally you will be able to see the flag, if you want to learn more about the HTTP Headers , here you have a link

https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers

Thank you !!!!