
# Recognizement #
## Nmap ##
(Soft)

##### Typical NMAP scann that he run#####
nmap -A -p- -Pn 

`-A` (Aggressive scan options)

This option enables additional advanced and aggressive options. Presently this enables OS detection (`-O`), version scanning (`-sV`), script scanning (`-sC`) and traceroute (`--traceroute`). More features may be added in the future. The point is to enable a comprehensive set of scan options without people having to remember a large set of flags. However, because script scanning with the default set is considered intrusive, you should not use `-A` against target networks without permission. This option only enables features, and not timing options (such as `-T4`) or verbosity options (`-v`) that you might want as well. Options which require privileges (e.g. root access) such as OS detection and traceroute will only be enabled if those privileges are available.


##### Soft scan #####

nmap -A -F T1(orT2) 10.10.10.223 -v

T1,T2 slow scann

`-F` (Fast (limited port) scan)

Specifies that you wish to scan fewer ports than the default. Normally Nmap scans the most common 1,000 ports for each scanned protocol. With `-F`, this is reduced to 100.


## Directory search with ffuf ##

ffuf -w /usr/share/wordlists/drib/common.txt -u http://url.htb/FUZZ -fc  403 -p 2

(fc= filter codes http)
(-p= seconds delay)

## SHODAN ##

use it from the console with an api key

See devices using some app

```
shodan count wordpress
```
See specificl version
```
shodan count wordpress 1.4.7
```
Get a file with the servers with this vuln or version
```
shodan download wordpressfile "wordpress 1.4.7"
```
then watch the content of the json file

json beautify


Shodan retrieves a lot of info from a particular host
```
shodan host 98.137.11.168
```

Shodan scan -h ( options for scan)

```
shodan scan submit 98.137.11.168
```



Alternatives to Shodan include Censys and Project Sonar.

##### EXCELENT WEBSITE #####

This site retrieves a lot of information of different sites

![[Screenshot_2023-09-22_21_50_58.png]]

https://bgp.he.net/


ASN que es ?

Un sistema autónomo se define como “un grupo de redes IP que poseen una política de rutas propia e independiente”. Esta definición hace referencia a la característica fundamental de un Sistema Autónomo: realiza su propia gestión del tráfico que fluye entre él y los restantes Sistemas Autónomos que forman Internet. [Wikipedia](https://es.wikipedia.org/wiki/Sistema_aut%C3%B3nomo)

##### Cherry tree  #####
Good and sintetic
##### Bounty hunter checklist #####
Bug bunty checklist
https://github.com/sehno/Bug-bounty/blob/master/bugbounty_checklist.md

----- ----- ----- 
Pentesting checklist
https://github.com/harshinsecurity/web-pentesting-checklist


##### URL parts #####

![[Screenshot_2023-09-22_22_24_30.png]]

WFUZZ good for guzz subdomains 

### SEARCHING FOR SUBDOMAINS ###


Here he says something about "DNS zone transfer  attack"

##### Using dig tool#####

This is interesting to use wen we see a port number 53 open


```
dig axfr @ip friendzone.red
```
You can save the output in a file and watch another domain
```
dig axfr @ip friendzoneportal.red > zone
```
Look the file in an order most "friendly"
```
cat zone | grep friendzone | grep IN | awk '{print $1}' | sort -u
```

###### Sort######
El comando sort (ordenar) es un comando para **ordenar líneas de archivos de texto**. Permite ordenar alfabéticamente, en orden inverso, por número, por mes y también puede eliminar duplicados.

sort -u (only displays the unique ones)
awk '{print $1}' = displays the first column
###### awk ######
**Nota:** En términos generales el comando awk permite procesar y modificar el texto según nuestras necesidades.

https://geekland.eu/uso-del-comando-awk-en-linux-y-unix-con-ejemplos/

### NSLOOKUP  && WHOIS###


Nslookup
Use:
```
┌──(cds㉿kali)-[~]
└─$ nslookup google.com
Server:         172.16.0.1
Address:        172.16.0.1#53

Non-authoritative answer:
Name:   google.com
Address: 142.251.134.46
Name:   google.com
Address: 2800:3f0:4002:809::200e

```

whois use:
```
┌──(cds㉿kali)-[~]
└─$ whois google.com                      
   Domain Name: GOOGLE.COM
   Registry Domain ID: 2138514_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.markmonitor.com
   Registrar URL: http://www.markmonitor.com
   Updated Date: 2019-09-09T15:39:04Z
   Creation Date: 1997-09-15T04:00:00Z
   Registry Expiry Date: 2028-09-14T04:00:00Z
   Registrar: MarkMonitor Inc.
   Registrar IANA ID: 292
   Registrar Abuse Contact Email: abusecomplaints@markmonitor.com
   Registrar Abuse Contact Phone: +1.2086851750
   Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
   Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
   Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited
   Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited
   Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited
   Name Server: NS1.GOOGLE.COM
   Name Server: NS2.GOOGLE.COM
   Name Server: NS3.GOOGLE.COM
   Name Server: NS4.GOOGLE.COM
   DNSSEC: unsigned
   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
>>> Last update of whois database: 2023-09-23T18:28:31Z <<<

For more information on Whois status codes, please visit https://icann.org/epp

NOTICE: The expiration date displayed in this record is the date the
registrar's sponsorship of the domain name registration in the registry is
currently set to expire. This date does not necessarily reflect the expiration
date of the domain name registrant's agreement with the sponsoring
registrar.  Users may consult the sponsoring registrar's Whois database to
view the registrar's reported date of expiration for this registration.

TERMS OF USE: You are not authorized to access or query our Whois
database through the use of electronic processes that are high-volume and
automated except as reasonably necessary to register domain names or
modify existing registrations; the Data in VeriSign Global Registry
Services' ("VeriSign") Whois database is provided by VeriSign for
information purposes only, and to assist persons in obtaining information
about or related to a domain name registration record. VeriSign does not
guarantee its accuracy. By submitting a Whois query, you agree to abide
by the following terms of use: You agree that you may use this Data only
for lawful purposes and that under no circumstances will you use this Data
to: (1) allow, enable, or otherwise support the transmission of mass
unsolicited, commercial advertising or solicitations via e-mail, telephone,
or facsimile; or (2) enable high volume, automated, electronic processes
that apply to VeriSign (or its computer systems). The compilation,
repackaging, dissemination or other use of this Data is expressly
prohibited without the prior written consent of VeriSign. You agree not to
use electronic processes that are automated and high-volume to access or
query the Whois database except as reasonably necessary to register
domain names or modify existing registrations. VeriSign reserves the right
to restrict your access to the Whois database in its sole discretion to ensure
operational stability.  VeriSign may restrict or terminate your access to the
Whois database for failure to abide by these terms of use. VeriSign
reserves the right to modify these terms at any time.

The Registry database contains ONLY .COM, .NET, .EDU domains and
Registrars.
Domain Name: google.com
Registry Domain ID: 2138514_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.markmonitor.com
Registrar URL: http://www.markmonitor.com
Updated Date: 2019-09-09T15:39:04+0000
Creation Date: 1997-09-15T07:00:00+0000
Registrar Registration Expiration Date: 2028-09-13T07:00:00+0000
Registrar: MarkMonitor, Inc.
Registrar IANA ID: 292
Registrar Abuse Contact Email: abusecomplaints@markmonitor.com
Registrar Abuse Contact Phone: +1.2086851750
Domain Status: clientUpdateProhibited (https://www.icann.org/epp#clientUpdateProhibited)
Domain Status: clientTransferProhibited (https://www.icann.org/epp#clientTransferProhibited)
Domain Status: clientDeleteProhibited (https://www.icann.org/epp#clientDeleteProhibited)
Domain Status: serverUpdateProhibited (https://www.icann.org/epp#serverUpdateProhibited)
Domain Status: serverTransferProhibited (https://www.icann.org/epp#serverTransferProhibited)
Domain Status: serverDeleteProhibited (https://www.icann.org/epp#serverDeleteProhibited)
Registrant Organization: Google LLC
Registrant State/Province: CA
Registrant Country: US
Registrant Email: Select Request Email Form at https://domains.markmonitor.com/whois/google.com
Admin Organization: Google LLC
Admin State/Province: CA
Admin Country: US
Admin Email: Select Request Email Form at https://domains.markmonitor.com/whois/google.com
Tech Organization: Google LLC
Tech State/Province: CA
Tech Country: US
Tech Email: Select Request Email Form at https://domains.markmonitor.com/whois/google.com
Name Server: ns4.google.com
Name Server: ns1.google.com
Name Server: ns2.google.com
Name Server: ns3.google.com
DNSSEC: unsigned
URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/
>>> Last update of WHOIS database: 2023-09-23T18:21:05+0000 <<<

For more information on WHOIS status codes, please visit:
  https://www.icann.org/resources/pages/epp-status-codes

If you wish to contact this domain’s Registrant, Administrative, or Technical
contact, and such email address is not visible above, you may do so via our web
form, pursuant to ICANN’s Temporary Specification. To verify that you are not a
robot, please enter your email address to receive a link to a page that
facilitates email communication with the relevant contact(s).

Web-based WHOIS:
  https://domains.markmonitor.com/whois

If you have a legitimate interest in viewing the non-public WHOIS details, send
your request and the reasons for your request to whoisrequest@markmonitor.com
and specify the domain name in the subject line. We will review that request and
may ask for supporting documentation and explanation.

The data in MarkMonitor’s WHOIS database is provided for information purposes,
and to assist persons in obtaining information about or related to a domain
name’s registration record. While MarkMonitor believes the data to be accurate,
the data is provided "as is" with no guarantee or warranties regarding its
accuracy.

By submitting a WHOIS query, you agree that you will use this data only for
lawful purposes and that, under no circumstances will you use this data to:
  (1) allow, enable, or otherwise support the transmission by email, telephone,
or facsimile of mass, unsolicited, commercial advertising, or spam; or
  (2) enable high volume, automated, or electronic processes that send queries,
data, or email to MarkMonitor (or its systems) or the domain name contacts (or
its systems).

MarkMonitor reserves the right to modify these terms at any time.

By submitting this query, you agree to abide by this policy.

MarkMonitor Domain Management(TM)
Protecting companies and consumers in a digital world.

Visit MarkMonitor at https://www.markmonitor.com
Contact us at +1.8007459229
In Europe, at +44.02032062220
```

##### Reverse whois #####

https://viewdns.info/reversewhois/

REVERSE IP searches 
```
Once you’ve found the IP address of the known domain, perform a
reverse IP lookup. Reverse IP searches look for domains hosted on the same
server, given an IP or domain. You can also use ViewDNS (https://viewdns.info/) .info for this.

Also run the whois command on an IP address, and then see if the tar-
get has a dedicated IP range by checking the NetRange field. An IP range is a

block of IP addresses that all belong to the same organization. If the orga-
nization has a dedicated IP range, any IP you find in that range belongs to
```

The -h flag in the whois command sets the WHOIS server to retrieve
information from, and whois.cymru.com is a database that translates IPs to
ASNs.
```
whois -h whois.cymru.com [ip]
```

If the company has a dedicated IP range and doesn’t mark those
addresses as out of scope, you could plan to attack every IP in that range.

### theHarvester ###

This tool search for domains, mails, etc. Powerful recon tool.

more info: theHarvester -h

Example:

```
kali >theHarvester -d domain.com  -b google.com
```


### crt.sh ###

Searchs for certificates online ( Secure Sockets Layer
(SSL) certificates)

(domains)

Watch the  domains they have a lot of info before to be relased to the site.

https://crt.sh/ (there are others like Censys, and Cert Spotter)

```
The crt.sh website also has a useful utility that lets you retrieve the
information in JSON format on the url put:

https://crt.sh/?q=facebook.com&output=json.
```


### Wayback machine ###

ES IMPORTANTE PORQUE HAY VULNERABILIDADES LA GENTE SE OLVIDA DE ACTUALIZAR LOS DOMINIOS Y QUEDAN SUBDOMINIOS VULNERABLES A ATAQUES!!

Old sites from the internet
https://archive.org/web/

##### WAYBACK URLS #####

-h help
```
┌──(cds㉿kali)-[~/go/bin]
└─$ ./waybackurls -h
Usage of ./waybackurls:
  -dates
        show date of fetch in the first column
  -get-versions
        list URLs for crawled versions of input URL(s)
  -no-subs
        don't include subdomains of the target domain
```

nano yahoo.txt
```
yahoo.com
```
Use the tool
```
cat yahoo.txt | ./waybackurls > yahoo.urls
```

##### amass enum #####

Amass gives you a lot of subdomines from a determinate url
```
kali> amass enum -passive -d yahoo.com
```

You will retrieve a lot of domines from yahoo.urls  buy you need just a couple of them

you need this
```
sudo apt install httpprobe
```

##### httprobe #####

use
(YOU CAN DO IT WITOUT THE OUTPUT FILE..)
```
cat yahoo.urls | httprobe -t > valid.urls
```

### Sublist3r (DO NOT WORK) (subdomaines)###
```
sudo apt install sublist3r
```
sublist3r -h

```
kali> sublist3r -d yahoo.com
```

Add this extension (OPEN MULTIPLE URLS On the browser)

ALTERNATIVE :
SUBFINDER

https://github.com/projectdiscovery/subfinder

go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest

```
┌──(cds㉿kali)-[~]
└─$ subfinder -d domain.com.ar   
```

### Amass (subdomaines) ###
Amass gives us the most quantity of subdomains possible, but we need to see if they still working

This tool have a option of IP fuzz very usefull
amass -h 
```
kali>amass enum -d yahoo.com

kali> amass enum -passive -d yahoo.com
```

httprobe can help 

use
(YOU CAN DO IT WITOUT THE OUTPUT FILE..)
```
cat yahoo.urls | httprobe -t > valid.urls
```

### W3TECHS - WAPPALIZER - REACT DEV TOOLS ###


### NNMAP ###

Bug bounty  way, to specific ports so we do not touch the alarms of the server:
```
nmap -A -p35,445,135,139,80,443 10.10.10.215 -v
```

### FFUF ###

```
ffuf -u https://FUZz.yahoo.com -w /usr/share/wordlist/dirb/common.txt -p 1
```
-p (slows 1 second)
-fc (omites some results)

Good for apis

```
ffuf -u https://api.yahoo.com/FUZZ -w /usr/share/wordlist/dirb/common.txt -p 1
```

### DIRB ###

Very easy to use
```
dirb https://yahoo.com
```


### WPSCAN ###


TIP: BE SURE READ ALL THE INFO OF THIS SCAN.
Wordpress scan 

```
wpascan -h 
```

Typical use (for pluggins)
```
kali >wpscan  --url http:url.com -e ap --pluggins-tetection agressive
```


### GEDIT 

Gedit es un editor de textos compatible con UTF-8 para Solaris, GNU/Linux, macOS y Microsoft Windows. Diseñado como un editor de textos de propósito general, gedit enfatiza la simplicidad y facilidad de uso. Incluye herramientas para la edición de código fuente y textos estructurados, como lenguajes de marcado.

```
sudo apt install gedit
```

## GOOGLE DORKS ##


#### site ####
Tells Google to show you results from a certain site only.
```
site:python.org.
```
#### inurl ####
Searches for pages with a URL that match the search string.
```
inurl:"/course/jumpto.php" site:example.com.
```
#### intitle ####

Finds specific strings in a page’s title.
```
intitle:"index of" site:example.com.
```

#### link ####

Searches for web pages that contain links to a specified URL.
```
link:"https://
en.wikipedia.org/wiki/ReDoS".
```
#### filetype ####

Searches for pages with a specific file extension.
```
site: filetype:log site:example.com.
```

#### Wildcard ####

```
wildcard (*)
```
You can use the wildcard operator within searches to mean any character or series of characters.

```
"how to hack * using Google".
```
#### Quotes ####
```
Quotes (" ")
```
Adding quotation marks around your search terms forces an exact match.
```
"how to hack" 
```
#### Or ####
```
Or (|)
```
The or operator is denoted with the pipe character (|) and can be used
to search for one search term or the other, or both at the same time.

```
"how to hack" site:(reddit.com | stackoverflow.com).
```

#### Minus ####
```
Minus (-)
```
The minus operator (-) excludes certain search results.

```
"how to hack websites" -php.
```

#### Look a lot of subdomines ####
```
*.site:.example.com
```
#### Special endpoints or vulnerabilities ####

Kibana exposition
```
site:example.com inurl:app/kibana
```
s3 amazon
```
site:s3.amazonaws.com COMPANY_NAME
```
#### Files from websites ####
```
site:example.com ext:php
site:example.com ext:log
```
And you can add extensions with words inside of them

```
site:example.com ext:txt password
```





# OSINT techniques #

Pastebin (https://pastebin.com/)

Job searches

Stack overflow questions of employees

Google calendars

# Tech Stack Fingerprinting#
Watching the version of the applications or technologies of the server or the target.

-Nmap -sV

-Watching the headers (HTTP headers like Server and X-Powered-By) with curl or a proxy like burpsuit. 

-Source code

-Wappalizer

https://buitlwith.com

https://stackshare.io

Retire.js (tool that searchs for outdated js and nodejs libraries )


### Writing Your Own Recon Scripts ###

TASK FOR THE FUTURE

# OWASP JUICE SHOP

SOurce foge .net 9.3.1 node 12 linux.tgz
sudo apt get install node.js
sudo apt get install npm
cd juiceshopfolder
npm start
	about:config
	network.proxy_allow_hijacking_locahost = true




# IDOR 
## Insecure direct object reference 

-Log as other person
-Write other persons blog or posts
-etc (most comon bugs that we encounter)

Creating two accounts and test your account

-Number of cookies

Changing passwords:
www.example.com/changepassword=username+password=password

(here you can change the password from another account)

#### Exercises
POSTBOOK hacker 101












