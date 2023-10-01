
https://medium.com/@hninja049/responder-hackthebox-walkthrough-f686dad57990

# Responder HackTheBox Walkthrough#

Responder is a free engine at the starting point of HackTheBox, it gives us a guide about NTLM and knowledge about LFI (local file inclusion).

**What is NTLM ?**

**Windows New Technology LAN Manager (NTLM)** is a suite of security protocols offered by Microsoft to authenticate users’ identity and protect the integrity and confidentiality of their activity. At its core, NTLM is a **single sign on (SSO)** tool that relies on **a challenge-response protocol** to confirm the user without requiring them to submit a password.

**What is LFI ?**

File Inclusion vulnerability allows an attacker to include a file, usually exploiting a “dynamic file inclusion” mechanisms implemented in the target application. The vulnerability occurs due to the use of user-supplied input without proper validation.

This can lead to something as outputting the contents of the file, but depending on the severity, it can also lead to:

- Code execution on the web server
- Code execution on the client-side such as JavaScript which can lead to other attacks such as cross site scripting (XSS)
- Denial of Service (DoS)
- Sensitive Information Disclosure

Local file inclusion (also known as LFI) is the process of including files, that are already locally present on the server, through the exploiting of vulnerable inclusion procedures implemented in the application. This vulnerability occurs, for example, when a page receives, as input, the path to the file that has to be included and this input is not properly sanitized, allowing directory traversal characters (such as dot-dot-slash) to be injected. Although most examples point to vulnerable PHP scripts, we should keep in mind that it is also common in other technologies such as JSP, ASP and others.

**Scanning**

let’s try to scan the machine using nmap.


now we will open the ip address in the browser.


add the ip address in /etc/hosts.


in this code there are parameter page, changing the language to another one and we get a page=french.html parameter in the URL.


this is case local file inclusion, now try to change value parameter page=../../../../../../../../../




now read the /etc/host our target using LFI



Now what we are going to do here is we are going to capture the NTLM (New Technology LAN Manager) hash of our administrator using a tool called **Responder.**

1. install responder
2. check network interface
3. use -I to specify network interface
4. run the responder -I tun0



after the responder start, now navigate to link.




we have out NTLM hash of Administrator let’s crack it using john the ripper, create new file.txt and paste the hash.




save the file and start crack using john the ripper.



next step start attack service running port 5985 WinRm.

Windows Remote Managment is a Microsoft protocol that **allows remote management of Windows machines** over HTTP(S) using SOAP.

The easiest way to detect whether WinRM is available is by seeing if the port is opened. WinRM will listen on one of two ports:

1. install evil-winrm via gem
2. running evil-winrm