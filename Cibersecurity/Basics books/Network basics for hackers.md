# Network basics for hackers #
## 1 Network Basics ##

All People Seems To Need Data Processing
Pease Do Not Throw Sausage Pizza Arround

![[osciseguridad.png]]

![[osciseguridad2.png]]
## 2 Subneting ##
## 3 Network analysis ##
#### Comands : ####

kali > ifconfig

kali > ping

kali > netstat –a (shows active connections)

To display all the TCP connections, you can use the to –t switch; for all the UDP connections, you can
use the –u switch and for all the listening connections, the –l switch, as seen below.

kali > netstat –a | grep http

kali > ss  (similar a netstat)

#### Network Sniffers ####

1. SolarWinds Deep Packet Inspection and Analysis Tool
2. Tcpdump
3. Windump
4. Wireshark
5. Network Miner
6. Capsa
7. tshark

#NSAs-EternalBlue-exploit (?)


#### TCP DUMP ####

kali > tcpdump


kali > tcpdump –w myoutput.cap (TAKE IT ON AN ARCHIVE)

*We can create that filter for the IP address by entering:*
kali > tcpdump host 192.168.0.114


kali > systemctl apache2 start
This starts your Apache webserver. Next, start tcpdump again on your Kali system.
tcpdump host 192.168.0.114


This filter displays traffic coming and going from our Windows 7 system. If we want to filter for just the
traffic coming FROM our Windows 7 system, we can create a filter like;
kali > tcpdump src host 192.168.0.114

*FILTERING BY PORT*

kali > tcpdump –vv dst port 80

Filter by TCP Flags

kali > tcpdump ‘tcp[tcpflags]==tcp-syn’

kali > tcpdump ‘tcp[tcpflags]==tcp-ack’

kali > tcpdump ‘tcp[tcpflags]==tcp-fin’

kali > tcpdump ‘tcp[tcpflags]==tcp-rst’

kali > tcpdump ‘tcp[tcpflags]==tcp-psh’

kali > tcpdump ‘tcp[tcpflags]==tcp-urg’

*Combining Filters*

kali > tcpdump host 192.168.0.114 and port 80

kali > tcpdump port 80 or port 443

*If we want to see all the traffic except that traveling from a particular IP address, we can use the negation
symbol (!) or not.*

kali > tcpdump not host 192.168.0.114

*Filtering for Passwords and Identifying Artifacts*

kali > tcpdump port 80 or port 21 or port 25 or port 110 or port 143 or port 23 –lA | egrep –i B5
‘pass=|pwd=|log=|login=|user=|username=|pw=|passw=|password=’

*Finally, if you want to filter for just the user agent (an identifying signature of the user and their browser)
we could create a filter such as:*

kali > tcpdump –vvAls | grep ‘User-Agent’

*Finally, to filter for just the browser cookies, we can create the following filter.*


kali > tcpdump –vvAls | grep ‘Set-Cookie|Host|Cookie:’

#### WIRESHARK ####

**Filtering by protocols **

tcp, udp , http etc
(it doesnt work with only a =)

**By adress **

ip.addr== < IP address >

**Filtering by ports**
tcp.dstport == 80

Note that this filter indicated the protocol (tcp), the direction (dst) and the port (80).

**Filter with keywords**

tcp contains facebook

If you want to see more options of filter , you can watch it on the top right on "EXPRESIONS"


All the operators of the filters
![[Captura de pantalla_2023-07-28_21-40-42.png]]

We can now create a filter by simply selecting a field in the left window, a relation in the upper right
window, and a value in the lower right window (values are very often 1 or 0, meaning they exist or do
not). For instance, if we want to find all tcp packets with the RST flag set, we would enter:


tcp. flags.rst == 1

**Following Streams**

Left click on a packet, and then search the option "follow"

**Statistics**



## 4 Firewals ##
Shows the tables
kali > sudo iptables -L

Help

kali > sudo iptables -h


-A this appends this rule to the chain

INPUT looks to match packets coming to the local system

-s sets the source address of the packets

-j sets the target in this case, DROP

sudo iptables -A INPUT -s 192.168.1.102 -j DROP

If we want to DROP packets destined for a particular port, we can use the -p option followed by
the protocol (tcp) and the --dport (destination port) followed by the port (ssh).

If we wanted to accept connections to the website www.amazon.com, we could build a rule that 
ACCEPTs outgoing connection (OUTPUT) over the TCP protocol (-p tcp) to amazon.com (-d amazon.com)

kali > sudo iptables -A OUTPUT -p tcp -d amazon.com -j ACCEPT

If we wanted to block access to any other websites, we could create the following two rules;

kali > sudo iptables -A OUTPUT -p tcp --dport 80 -j DROP
kali > sudo iptables -A OUTPUT -p tcp --dport 443 -j DROP

DELETE TABLES

kali > sudo iptables -F

## 5 Wi-Fi Networks (802.11) ##
### Airodump ng mac spoofing ###
It isnt necesary to know the bssid ... if we know the mac of the router wifi we can acces on it.

Ver las macs de usuarios en una ssid oculta (si ha filtrado las macs de los usuarios autorizados )

kali > airodump-ng –c 11 –a –bssid < mac >

changing your mac

kali> ifconfig wlan0 down

Then, use macchanger to spoof the MAC address making it the same as the connected client’s MAC.

kali > macchanger –m < mac > wlan0

kali > ifconfig wlan0 up

WEP insecure
wpa -wpa2 more secure
### ATTACK WITH AERODUMP (THE CLASSICAL) ###


### HACKING WPS ###

(finding wifis with wps)
kali > wash –i wlan0mon

we can brute force the PIN with either

(DIFFERENT PROGRAMS THAT DO THE SAME APARENTLY)
To use bully, enter:

kali > bully wlan0mon –b 00:11:22:33:44:55 –e NTGR_VMB_1462061001–c 11

To use reaver, enter the following:

kali > reaver –i wlan0mon –b 00:11:22:33:44:55 –vv

Make certain that you replace the MAC address with the actual MAC address of the target AP, the actual
SSID of the target AP, and the actual channel the AP is broadcasting on.
### Build our Evil Twin ###

To do so, we need another tool from the aircrack-ng suite, airbase-
ng.

kali > airbase-ng –a aa:bb:cc:dd:ee:ff --essid hackers-arise –c 6
wlan0mon

Where:
aa:bb:cc:dd:ee:ff is the MAC address of the new Evil Twin AP

--essid hackers-arise is the name of the Evil Twin AP

-c 6 is the channel we want it to broadcast on

wlan0mon is the interface we want to use as an AP


Now that we have our wireless card up as an AP let’s check our system again for wireless extensions with
iwconfig. (WE CREATE ANOTHER FAKE WIFI)

![[Captura de pantalla_2023-07-30_17-29-10.png]]

We now need to set up a DHCP server (it assigns IP addresses to those who connect) to the tunnel we
created.

kali > dhclient ha &



To get the clients to connect to our new Evil Twin AP, we need to knock them off the legitimate AP. We
can do this the same way we did above in our WPA2 attack. We use the aireplay-ng command and
send de-authentication frames into the AP (sometimes, this can DoS some of the older AP hardware).
This will make the legitimate AP unavailable to the clients, and they will connect to the Evil Twin
instead!

kali > aireplay-ng –deauth 1000 aa:bb:cc:dd:ee:ff wlan0mon –ignore-
negative-one

Now you can see all the unencrypted trafic of the client on the wifi
### Denial of Service (DoS) Attack ###

(DROPS ALL THE USERS OF AN AP, AND SOMETIMES FORCE THE ADMINISTRATOR TO FORCE THE AP)
kali > aireplay-ng –deauth 100 –a < BSSID > wlan0mon
Script for doing until the end ..
![[deau.png]]

### PMKID Attack ###

The tools we need for this attack are not built into Kali by default, so we will need to download them
from github and build them.
First, we need the hcxdumptool. Using git clone, we can download it from www. github.com by
entering;
kali > git clone https://github.com/ZerBea/hcxdumptool.git

Then, navigate to the new hcxdumptool directory;

kali > cd hcxdumptool

..and make and install this tool.

kali >make
kali >make install



Next, we need the hcxtools. Just like the hcxdumptool above, we can download and install it by
entering;

kali >git clone https://github.com/ZerBea/hcxtools.git
kali >cd hcxtools
kali >make
kali >make install

We now need to place our wireless adapter into monitor mode again.
kali >airmon-ng start wlan0
With the wireless adapter in monitor mode, we can now probe the available APs for their PMKID.
kali >hcxdumptool –I wlan0mon –o HackersArisePMKID –enable_status=1

FILTER ONLY THE TARGET ID THAT WE WANT TO ATTACK
kali > cat > targetBSSID <the target AP’s BSSID>

kali > hcxdumptool –I wlan0mon –o HackersArisePMKID –enable_status=1 –
filterlist_ap=targetBSSID –filtermode=2


Convert Dump to Hashcat Format
To convert the HackersArisePMKID file into a format that hashcat can work with, we need to use
the hcxcaptool. Make certain you are in the same directory as the HackersArisePMKID file and
enter:
kali > hcxcaptool –z hashoutput.txt HackersArisePMKID
Now that we have stripped out all the superfluous information, we can send this hashoutput.txt file
to hashcat and crack it! Note the –m 16800 in this command represents the appropriate hash
algorithm for this hash.
kali > hashcat –m 16800 hashoutput.txt top10000passwords.txt
## 6 Bluetooth Networks ##

### The BlueBourne Attack ###

The exploit attacks the SDP protocol of the BlueTooth
## 7 Address Resolution Protocol(ARP) ##

kali > sudo arp -v (more info,flags,etc)


Tool

kali > sudo netdiscover -h.
kali > netdiscover -r 192.168.100.0/24 (r=range)

arpspoofing. (this is the name of this kind of attacks)

Tools that we use 

Ettercap is an easy-to-use arp spoofing tool for MiTM attacks. To learn more about Ettercap,
click here (https://www.hackers-arise.com/post/2017/08/28/MiTM-Attack-with-Ettercap).
Other tools that utilize ARP for MitM attacks include;

1. arpspoof (https://www.hackers-arise.com/post/2017/07/25/man-the-middle-mitm-attack-
with-arpspoofing)

2. driftnet (https://www.hackers-arise.com/post/2017/09/27/MitM-Using-driftnet-to-View-the-
Targets-Graphics-Files)

**Leveraging ARP in the Metasploit Meterpreter**

The Address Resolution Protocol (ARP) can also be leveraged by the Metasploit Meterpreter

## 8 DNS##
### DNS vulnerabilities ###

Some attacks
Abusing dns reconnnaissance

Spoof dns (LAN)

DoSS
DoS


BIND ( linux implementation for dns ) -> this is not an attack

CHANGING DNS SERVER SETTINGS

.---------------------------------

Building a DNS server in linux



TO DO.. CREATE YOUR OWN DNS SERVER.

## 9 SMB (server message block) ##
### Create a SMB server ###

## 10 Simple Message Transfer Protocol ##

Scan the  SMTP server
kali > nmap -sT -A 192.168.56.103 -p25

-A switch
to attempt to determine the service running on that port,

if it is open this search scriopts for the version

nmap --script=smtp-* 192.168.56.103 -p 25

To determine any potential vulnerabilities on that SMTP server, we might use nmap scripts. To
run all the nmap scripts for SMTP, we can use the --script=smtp-* option where the wildcard
(*) means to run all the scripts in the smtp category.


(RETURN AGAIN TO THIS SECTION ,MAKE A SMTP SERVER, AND INFORMATE ABOUT THE POSSIBLE EXPLOITS THAT YOU ACN USE)
## 11 SNMP Simple Network Manage Protocol ##
If an attacker can breach the SNMP, they may be able
to unmask your encrypted VPN communication (see NSA's ExtraBacon exploit here) as well as see and possibly control every device connected to
your network.

UDP ports 161 and 162

Network devices use this protocol to communicate with each other and
can be used by administrators to manage the devices.


The management data exposed by the agents on each of the managed machines are stored in a
hierarchical database called the Management Information Base or MIB.

The SNMP protocol communicates on UDP port 161. The communication takes place with
protocol data units or PDU's. These PDU's are of seven (7) types.
 GetRequest
 SetRequest
 GetNextRequest
 GetBulkRequest
 Response
 Trap
 InformRequest

SNMPv1 is not encrypted and still in use in many networks

Abusing SNMP for Information Gathering


SNMP can be a rich source of information on the target network if we can access it. snmpcheck
will pull the information from the MIB, and onesixtyone helps us crack the SNMP "passwords."
Both can be critical in exploiting SNMP for reconnaissance.

## 12 HTTP ##
The basic syntax of the URL is:
protocol://hostname[:port]/ [/path/] file [?param=value]

HTTP Headers
There are numerous types of HEADERS in HTTP. Some can be used for both requests and
responses, and others are specific to the message types.
These are some of the common header types;
General Headers
* Connection - tells the other end whether the connection should closed after HTTP transmission
* Content-Encoding - specifies the type of encoding
* Content-Length - specifies the content length
* Content-Type - specifies the content type
* Transfer-Encoding - specifies the encoding on the message body
Request Headers
* Accept - specifies to the server what type of content it will accept
* Accept-Encoding - specifies to the server what type of message encoding it will accept
* Authorization - submits credentials
* Cookie - submits cookies to the server
* Host - specifies the host name
* If-Modified-Since - specifies WHEN the browser last received the resource. If not modified,
the server instructs the client to use the cached copy
* If-None-Match - specifies entity tag
* Origin - specifies the domain where the request originated
* Referrer - specifies the URL of the requestor
* User-Agent - specifies the browser that generated the request
Response Headers

* Access-Control-Allow-Origin - specifies whether the resource can be retrieved via cross-
domain

* Cache-Control - passes caching directive to the browse
* Etag - specifies an entity tag (notifies the server of the version in the cache)
* Expires - specifies how long the contents of the message body are valid
* Location - used in redirect responses (3xx)
* Pragma - passes caching directives to the browser
* Server - specifies the web server software
* Set-Cookie - issues cookies
* WWW-Authenticate - provides details of the type of authentication supported
* X-Frame-Options - whether and how the response may be loaded within the browser frame

Cookies
Cookies are a critical part of HTTP. Cookies enable the server to send items of data to the client,
and the client stores this data and resubmits it to the server the next time a request is made to the
server.
The server issues a cookie to the client using the SET-COOKIE response header.
SetCookie: Tracking=wdr66gyU34pli89
When the user makes a subsequent request to the server, the cookie is added to the header.


Cookies are used to identify the user of the server and other key information about the server.
These cookies are usually a name/value pair and do not contain a space.

STATUS ( STATUS OF THE RESPONSES ARE IMPORTANT TO .. SO YOU MUST LEARN SOME OF THEM)
## 13 Automobile Networks  ##

The CAN Protocol.
sudo apt install can-utils

1. Basic tools to display, record, generate and play can traffic
2. CAN access via IP sockets
3. CAN in-kernel gateway configuration
4. Can Bus measurement
5. ISO-TP tools
6. Log file converters
7. Serial line discipline (slc) configuration

1. Basic tools to display, record, generate and replay CAN traffic

 candump : display, filter and log CAN data to files
 canplayer : replay CAN logfiles
 cansend : send a single frame
 cangen : generate (random) CAN traffic
 cansniffer : display CAN data content differences (just 11bit CAN IDs)

2. CAN access via IP sockets
 canlogserver : log CAN frames from a remote/local host
 bcmserver : interactive BCM configuration (remote/local)
 socketcand : use RAW/BCM/ISO-TP sockets via TCP/IP sockets
3. CAN in-kernel gateway configuration
 cangw : CAN gateway userpace tool for netlink configuration
4. CAN bus measurement and testing
 canbusload : calculate and display the CAN busload
 can-calc-bit-timing : userspace version of in-kernel bitrate calculation
 canfdtest : Full-duplex test program (DUT and host part)
5. ISO-TP tools ISO15765-2:2016 for Linux
 isotpsend : send a single ISO-TP PDU
 isotprecv : receive ISO-TP PDU(s)
 isotpsniffer : 'wiretap' ISO-TP PDU(s)
 isotpdump : 'wiretap' and interpret CAN messages (CAN_RAW)
 isotpserver : IP server for simple TCP/IP <-> ISO 15765-2 bridging (ASCII HEX)
 isotpperf : ISO15765-2 protocol performance visualisation
 isotptun : create a bi-directional IP tunnel on CAN via ISO-TP
6. Log file converters
 asc2log : convert ASC logfile to compact CAN frame logfile
 log2asc : convert compact CAN frame logfile to ASC logfile
 log2long : convert compact CAN frame representation into user readable
7. Serial Line Discipline configuration (for slcan driver)
 slcan_attach : userspace tool for serial line CAN interface configuration
 slcand : daemon for serial line CAN interface configuration
 slcanpty : creates a pty for applications using the slcan ASCII protocol

