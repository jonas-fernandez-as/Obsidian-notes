
Traducción del inglés-En criptografía, un ataque de oráculo de relleno es un ataque que utiliza la validación de relleno de un mensaje criptográfico para descifrar el texto cifrado.

¿Sabes qué es un _padding oracle attack_? Se trata de un ciberataque criptográfico cuyo concepto es bastante simple, pero a la vez muy poderoso y demuestra que **es posible romper un criptosistema a partir de una cantidad mínima de información**.


The encoding aparently is on block, the text plain is transformed to blocks, sometimes two , ten or I dont know how many.




Padbuster does everything automatically . Add first the IP , the cookie and 8 bytes before, now the name of the cookie with the cookie again and finally put the encoding 0 (youre saying that you want a base64 encoding)


8bytes mean that you will go decoding by 8 bytes of the hash I think , when it finish with 8, follows to the next 8 bytes until the end.

```
padbuster http://10.10.10.189/login.php (session cookie) 8 -cookie "auth=(cookie)" -encoding 0
```

Now if everything goes well, you will see a table. You must select a number to specify which row you want to select.

He will try to get the "keys" or "password" for each block.


This app tries to give us the cookie on plain text

This permites us to create a cookie session for another user, like an "admin" user. Like this

This command computes a new cookie
```
padbuster http://10.10.10.189/login.php (session cookie) 8 -cookie "auth=(cookie)" -encoding 0 -plaintext "user=admin"
```

When you got the cookie you put on the platform of the navigator that you use like "modify this cookie" or something like that. You modify the curent cookie for the cookie of admin that you did.





Another way to do this on padding oracle attack is put on the register zone

```
admin= (or the user that you want)
```

and then you clone the environment of that user, and you log as that user... just on the registration. 


I was trying to clone the id of the administrator and i get a error

![[Screenshot_2023-09-17_11_47_13.png]]

![[Screenshot_2023-09-17_11_47_19.png]]





Here i see something strange... 

```c
      │ File: koulis.js
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ // just a meme - not a part of the challenge
   2   │ (() => {
   3   │     const gif = `${window.location.origin}/static/koulis.gif`;
   4   │     const canvas = document.createElement('canvas');
   5   │     const ctx = canvas.getContext('2d');
   6   │     const image = new Image();
   7   │ 
   8   │     image.addEventListener('load', () => {
   9   │         canvas.width = image.width;
  10   │         canvas.height = image.height;
  11   │ 
  12   │         ctx.font = 'normal 16.6px Consolas';
  13   │         ctx.fillStyle = '#fff';
  14   │         ctx.textAlign = 'left';
  15   │         ctx.textBaseline = 'middle';
  16   │         ctx.lineWidth = 3;
  17   │         ctx.strokeStyle = '#000';
  18   │ 
  19   │         ctx.strokeText('Welcome to a place where your dreams dont matter', 90, 160);
  20   │         ctx.fillText('Welcome to a place where your dreams dont matter', 90, 160);
  21   │ 
  22   │         ctx.strokeText('Makelarides are performing black magic 🧙', 113, 190);
  23   │         ctx.fillText('Makelarides are performing black magic 🧙', 113, 190);
  24   │ 
  25   │         ctx.font = 'normal 11px Consolas';
  26   │ 
  27   │         console.log('%c+', `font-size: 1px; padding: ${image.height/2}px ${image.width/2}px; background: url(${canvas.toDataURL()}), url(${gif}); backgro
       │ und-repeat: no-repeat; background-size: ${image.width}px ${image.height}px; color: transparent;`);
  28   │     });
  29   │     image.src = gif;
  30   │ })();


```


This is strange for me , it's written just a meme - not part of the challenge and the gif is very suspicious hahaha... and there is a mage and speak about black magic ...
http://0.0.0.0:1337/static/koulis.gif

look at this face hahaha
![[Screenshot_2023-09-17_13_32_51.png]]

