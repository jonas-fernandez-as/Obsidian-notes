

  Type juggling vulnerability is a security vulnerability that arises when a PHP script does not properly validate user input, and instead relies on type juggling to automatically change the data type of a variable based on the input.

Savitar has created a server on apache 2 with a form that aparently is on php... or something like that.
![[Screenshot_2023-08-30_16_21_36.png]]

![[Screenshot_2023-08-30_16_21_39.png]]

So, is on the index.php and now he did a validations on the user and password on php, this is suposed that nobody can see it (if they investigate on the font)
![[Screenshot_2023-08-30_16_25_32.png]]



Now he did some validations, comparing if the 'usuario' is a valid user, and comparing with strcmp the password too.. if they are ok the status code== 0 is success, so everything would be ok
![[Screenshot_2023-08-30_16_29_23.png]]

Now , everything would be ok with the final validation , a message is sended to the users 
![[Screenshot_2023-08-30_16_32_37.png]]


Ok.. now enters the concept of type juggling... 

He now will make petitions with curl about the data

```
curl -X POST --data ' usuario=admin&password=pepe' http://127.0.0.1 | html2text
```

Strangely if we make another petition with this syntax the petition is successfull.

```
curl -X POST --data ' usuario=admin&password[]=pepe' http://127.0.0.1 | html2text
```
