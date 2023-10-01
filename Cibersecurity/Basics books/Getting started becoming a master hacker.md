# Getting started becoming a master hacker #
## 2 Story of hacking##
## 3 The hacker process ##

You may need the following information to successfully exploit a system:

1. The operating system;
2. The service pack of the operating system;
3. What ports are open on the target system;
4. What services are running on the target system;
5. What applications are running on the target system; and
6. What language is used on the target system.

#### Steps ####
##### Fingerprinting #####

Fingerprinting is the process of enumerating the following attributes of a target:
1. Users
2. Hosts
3. Network Topology
4. Operating Systems
5. Services

This can be made in two ways:

- Passive Reconnaissance (OSINT)
- Active Reconnaissance (NMAP,ETC, MORE RISKS)

##### Next step #####

- Password Cracking (ONLINE MORE DIFFICULT THAN OFFLINE)
- Exploitation (If we have failed with password-cracking)

##### Third step #####
- Post-Exploitation

##### Fourth step #####

- Covering Tracks
This can mean deleting or altering log files, deleting bash commands,
changing timestamps on files, and others.









## 4 Mounting a lab ##
## 5 Passive reconnaissance ##

## 6 Active reconnaissance ##
NMAP
HPING3
## 7 Searching for exploits ##


Types of web vunerabilities
1. Application Error Disclosure
2. X-Frame-Options Header Not Set
3. Cookie No HttpOnly Flag
4. Cross Domain Javascript Source File Inclusion
5. Web Browser XSS Protection Not Enabled
6. X-Content-Type-Options Header Missing



## 8 Cracking passwords ##

The hash of the password on linux are stored on the:
- /etc/shadow
On windows :
- C:\Windows\System32\config\SAM

With modern systems, the password cracking process is to (1) generate a potential password; (2) encrypt
it with the same algorithm the system used to generate the hash; and then (3) compare that hash to the one
recovered from the system.



I prefer to think
of just two of them(between the biggest methods that are disponible). The first approach is to use a list of potential passwords. These might include:
1. Dictionary;
2. Dictionary with special characters and numbers;
3. List of commonly used passwords;
4. Custom wordlist developed by the hacker.

Tools

John (craks passwords from different hashes, by bruteforce with dictionaries)

Custom password list

ceWL , crunch, cupp

ceWL = takes passwords form a website internet (ex, specialized words)
crunch= adds special characters to the passwords

cupp = Common User Password Profiler. Is not on kali

./cupp -i (interactive mode)

Hashcat= you know that

To use hashcat to crack passwords, we will need:
1. The type of hash we are cracking;
2. The type of attack;
3. The output file for the cracked passwords;
4. The file containing our hashes;
5. The file containing our wordlist.

To then crack the password hashes from a Windows system, we could create a hashcat command as
such:
kali > hashcat –m 0 –a o –o passwords hashlist.txt /root/top10000passwords


hashcat is the command
-m 0 designates the type of hash we are attempting to crack (MD5 in this case)
-a 0 designates a dictionary attack
-o passwords is the output file for the passwords

hashlist.txt is the input file of the hashes
/root/top10000passwords is the absolute path and file name of the wordlist

pwdump. (when we dont have acces to DLL dynamic linked library, and root privileges to acces to c:\Windows\System32\config\SAM.) at https://www.openwall.com/passwords/windows-pwdump.

Remote Password Cracking

medusa

hydra

### 9 Metasploit ###
