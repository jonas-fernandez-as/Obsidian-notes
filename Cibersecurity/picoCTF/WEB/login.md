#### Description

My dog-sitter's brother made this website but I can't get in; can you help?

## Process ##


On this challenge we have a log in. I cant intercept the pettition with burpsuit but when you inpect the source code you can see that there is a js file with an obfuscate code. If you beautify the code you can see this.

```
(async () => {
    await new Promise((e => window.addEventListener("load", e))), document.querySelector("form").addEventListener("submit", (e => {
        e.preventDefault();
        const r = {
                u: "input[name=username]",
                p: "input[name=password]"
            },
            t = {};
        for (const e in r) t[e] = btoa(document.querySelector(r[e]).value).replace(/=/g, "");
        return "YWRtaW4" !== t.u ? alert("Incorrect Username") : "cGljb0NURns1M3J2M3JfNTNydjNyXzUzcnYzcl81M3J2M3JfNTNydjNyfQ" !== t.p ? alert("Incorrect Password") : void alert(`Correct Password! Your flag is ${atob(t.p)}.`)
    }))
})();
```

While I was trying to log in I use the credential admin, and the message from the website was "incorrect password" if I write another username I receive "incorrect username".

So if you watch the code is validating two inputs , one from the username and other from the password, but they are encrypted 

I tried this on my console
```
┌──(cds㉿kali)-[~/Downloads]
└─$ echo "YWRtaW4" | base64 -d ; echo
adminbase64: invalid input

                                                                                                                                                                      
┌──(cds㉿kali)-[~/Downloads]
└─$ echo "cGljb0NURns1M3J2M3JfNTNydjNyXzUzcnYzcl81M3J2M3JfNTNydjNyfQ" | base64 -d ; echo
picoCTF{*******************************}base64: invalid input

```

So, there is the flag... the flag is the password