  

Insecure deserialization is **a vulnerability in which untrusted or unknown data is used to inflict a denial-of-service attack, execute code, bypass authentication or otherwise abuse the logic behind an application**.


Some tools: 
curl -sL https://deb.nodesource.com/setup_18.x | sudo -F bash -

apt install nodejs npm -y


He installed node and now is watching on a machine, that the message is 404 when he enters.

http://10.10.10.85:3000 

he did CTRL +R and the 404 changed to 
"hey dummy 2 + 2 is 22"


He intercepted in burpsuit a cookie profile on base 64

now he decodes it ![[Screenshot_2023-08-30_16_48_53.png]]
Now he send it to the decoder of burpsuit and changed the user from Dummy to savitar and encoded it again to base64

![[Screenshot_2023-08-30_16_49_51.png]]

Now he is sending again the cookie on burpsuit changed... but why ????


Now he creates a "serialize function"

This executes the command that we want to execute

![[Screenshot_2023-08-30_17_09_58.png]] 


If we want to execute this with node, we put 
"npm install node-serialize"


If i use :

```
node serialize.js
```

Node doesnt execute me this

BUT WITH IIFE we can execute commands before the data will be serialized and  before it is deserealized
***
IIFE = EXPRESION DE FUNCION EJECUTADA INMEDIATAMENTE
***
now, he created another file called "unserialize.js"

So now he did this code where he required the serialized data and the "serialize.js"

![[Screenshot_2023-08-30_17_16_01.png]]


Now he put node unserialize.js and nothing happens


(HE added  this slash \ to some of the ' '  )


HE ADDED () TO A FILE  serialize.js
![[Screenshot_2023-08-30_17_18_56.png]]

Agrega los  () y todo esto del ife al "serialize.js" .... y tambien le agrega otro al "unserialize.js"

he said, this executes inmediately because of this .. (IIFE) (executes the command before serialize  the data)

So he said that the syntax of the petition of the cookie is "serialized" so for this reason is vulnerable to this attack. So when the server recives this petition he unserialize the data


To add the command he goes to "nodejs shell exploit" on google.. I mean the command to add before or after the data become serialized or unserialized

He downloaded a script on python

So , now on the "unserialize.js" he removed all the cookie data, and he just put the reverse shell on node (all encoded on base64).


This command put all on the same line, on base 64 format and the "echo" of the end , removes a aditional  hashtag "#" on the end of the base64 encoding
 ```
cat data | base64 -w 0; echo
```

The key is that IIFE () I MEAN THE IIFE IS THAT PARENTHESES THAT EXECUTES THE ACCTION WHEN THE DATA IS UNSERIALIZED ON THE SERVER.

while he is listening on the 443 with netcat he sent now the new cookie. And then he have connection to the machine.
