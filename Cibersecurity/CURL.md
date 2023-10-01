

http -https

### basic (returns the content of the site)###
```
curl https://sitio.com 
```

### OUTPUT FILE ###

Saves the output of a query to a file in a particular path (It goes just to a particular resource) 

Es importante especificar el protocolo del sitio(ej:http, etc)

```
curl -o /home/cds/desktop/sitio.html http://sitio.com
```


![[Screenshot_2023-09-17_15_36_44.png]]

The bar at the top of the output 

TOTAL : total amount of the downloaded
RECEIVED: the amount recived
AVERAGE SPEED : the average download speed
TIME : total , spent , left
CURRENT SPEED.


### DOWNLOADING FILES  curl -o###

The file will be downloaded on the folder that we are currently working.


Here we are giving a custom name to the file 
```
kali/desktop> 
curl -o ubuntuIso.iso http://pathtotheubuntufile.com
```

capital 0 downloads it with the default name from the server
```
kali/desktop> 
curl -O http://pathtotheubuntufile.com
```

## REDIRECTS  curl -L##

Specify the redirect of an url (from http to https)

```
curl -L http://sitio.com
```
If I leave http this doesn't retrieves me nothing

### SEE HEADERS  CURL -I ###
-I
#### this analyzes the headers of a response of the web server ####

```
curl -I http://sitio.com
```

### TLS HANDSHAKE CURL -v ###

TLS Handshake **es un protocolo que sirve para que dos servidores se verifiquen entre sí y puedan establecer un tráfico cifrado e intercambiar claves**. Si todo va bien y se verifican correctamente se realiza el intercambio de datos.

```
curl -v http://sitio.com
```

## PETITIONS HTML ##

```
curl --data "log=admin&pwd=password" https://wordpress.com/wp-login.php
```



# API REQUESTS #


-X  or --request -HTTP method to use
-i or --include -include the response headers
-d or --data -The data to be sent to the API
-H or --header -Any aditional headers to be sent


GET normal
```
curl https://jsonplaceholder.typicode.com/posts
```
GET with params
```
curl https://jsonplaceholder.typicode.com/posts?userID=5
```


POST
Normal
```
  curl -X POST "userId=5&title=PostTitle&body=Post content."  https://jsonplaceholder.typicode.com/posts
```
POST
authentication
```
curl -X POST \
https://some-web-url/api/v1/users \
-H 'Accept: application/json' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer {ACCESS_TOKEN}' \
-H 'cache-control: no-cache'
-d '{
"username": "myusername",
"email":"myusername@gmail.com"
"password": "Passw0rd123!"
}'
```
PUT

```
curl -X PUT -H 'Content-Type: application/json' \
-d'{
"userId":5,
"title":"New Post title",
"body":"new post content"
}' \
https://site/api/v1.com/posts/5
```

PATCH

```
curl -X PUT -H 'Content-Type: application/json' \
-d'{
"userId":5,
"title":"New Post title",
"body":"new post content"
}' \
https://site/api/v1.com/posts/5
```
DELETE
```
curl -X DELETE  https://jsonplaceholder.typicode.com/posts?userID=5
```
