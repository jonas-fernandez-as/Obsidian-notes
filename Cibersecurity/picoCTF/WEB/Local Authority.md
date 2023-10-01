
# Hints #

How is the password checked on this website?

# Process #
On this challenge you have to log in on a site and you have no credentials.

"Only letters and numbers allowed for username and password."

I turn on burpsuit and then I try to log in with any credentials, I have a login failed. but when I send the petition to the repeater I can see this:

```
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 27 Sep 2023 00:45:03 GMT
Content-Type: text/html; charset=UTF-8
Connection: close
Content-Length: 1901

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="style.css">
    <title>Login Page</title>
  </head>
  <body>
    <script src="secure.js"></script>
    
    <p id='msg'></p>
    
    <form hidden action="admin.php" method="post" id="hiddenAdminForm">
      <input type="text" name="hash" required id="adminFormHash">
    </form>
    
    <script type="text/javascript">
      function filter(string) {
        filterPassed = true;
        for (let i =0; i < string.length; i++){
          cc = string.charCodeAt(i);
          
          if ( (cc >= 48 && cc <= 57) ||
               (cc >= 65 && cc <= 90) ||
               (cc >= 97 && cc <= 122) )
          {
            filterPassed = true;     
          }
          else
          {
            return false;
          }
        }
        
        return true;
      }
    
      window.username = "admin";
      window.password = "strongPassword098765";
      
      usernameFilterPassed = filter(window.username);
      passwordFilterPassed = filter(window.password);
      
      if ( usernameFilterPassed && passwordFilterPassed ) {
      
        loggedIn = checkPassword(window.username, window.password);
        
        if(loggedIn)
        {
          document.getElementById('msg').innerHTML = "Log In Successful";
          document.getElementById('adminFormHash').value = "2196812e91c29df34f5e217cfd639881";
          document.getElementById('hiddenAdminForm').submit();
        }
        else
        {
          document.getElementById('msg').innerHTML = "Log In Failed";
        }
      }
      else {
        document.getElementById('msg').innerHTML = "Illegal character in username or password."
      }
    </script>
    
  </body>
</html>

```
That is not able to you when you have a login failed an inspect the source code, but its able to you when you use a proxy to intercept the petition. Inspecting the code you can see that is validating if you log in or no and appears a hash. And the source of this script is "secure.js"

you can go and see it on 
http://saturn.picoctf.net:49386/secure.js

```
function checkPassword(username, password)
{
  if( username === 'admin' && password === 'strongPassword098765' )
  {
    return true;
  }
  else
  {
    return false;
  }
}
```
And here is... in plain text the user , and the password... 

I receive this hash on the proxy
```
hash=2196812e91c29df34f5e217cfd639881
```
and then on this url
http://saturn.picoctf.net:49386/admin.php
I receive the flag:

`picoCTF{**********************}`