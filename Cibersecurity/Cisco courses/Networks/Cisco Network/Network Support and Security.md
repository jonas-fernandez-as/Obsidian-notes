# 1 Network support #

Summary
### Diagnostics and trouble shooting methodologies ###
Troubleshooting is a process that should be applied systematically. One approach uses a seven-step process in which the technician defines the problem, gathers relevant information, analyzes the information, eliminates possible causes, proposes a hypothesis about the most likely cause of the problem, and then tests the hypothesis and solves the problem. Another approach is to follow the layers of the OSI model.

Structured troubleshooting can include the use of seven different methods, bottom–up, top-down, divide-and-conquer, follow-the-path, comparison, and the educated guess approach.

The choice of method sometimes depends on the type of issue that is being addressed and the experience of the technician. It is important to always document the issue according to company procedures, including providing information of the eventual resolution of the problem.
### Network documentation ###
Network documentation is essential to maintaining, securing, and troubleshooting networks. Documentation may consist of physical and logical network diagrams, written documents, and network performance baselines.

There are nine network topologies that may be documented. This includes personal area networks (PAN), local area networks (LAN), virtual LANs (VLANS), wireless LANs (WLAN), wireless mesh networks (WMN), campus area networks (CAN), metropolitan area networks (MAN), wide area networks (WAN), and virtual private networks (VPN).

Physical topology diagrams include the physical locations of devices and documents their connections. Logical topology diagrams include IP addresses and networking device details such as connected ports. Other information such as cloud services, routing policies, and security and remote access policies may appear on topology diagrams.

Cloud services can be Software as a Service (SaaS), Platform as a Service (PaaS), or Infrastructure as a Service (IaaS). XaaS means anything/everything as a service, including desktop as a service (DaaS), disaster recovery as a service (DRaaS), communications as a service (CaaS), and monitoring as a service (MaaS).

Wireless standards define the operating characteristics of wireless operations, including signaling specifications, data rates, and power efficiency. Wireless standards form the IEEE 802.11 wireless Ethernet family of standards, such as 802.11b, n, g, and ac. These standards exist in the unlicensed wireless spectrum. Licensed wireless frequencies are controlled by the Federal Communications Commission (FCC) and licenses are granted to radio stations, cellular companies, and television stations.

Device documentation differs depending on the type of devices. It will often include device operating system and software, licensing information, interface status, addressing, and routing protocols, etc.

Network baselines are a series of measurements of network performance taken during different types of network usage. The baselines help to understand the parameters of a properly working network so that network performance or security problems can be identified when performance deviates significantly from previous baseline measurements.

Cisco Discovery Protocol (CDP) is a Cisco protocol that runs on Cisco networking devices. It sends CDP advertisements to directly attached neighbor devices. Information sent in these advertisements include the configured device name, a port identifier, the hardware platform and software versions, and IP addresses. This information is displayed with the IOS commands **show cdp neighbors** and **show cdp neighbors detail**. CDP can be used to reveal information about network topologies.
### Help desks ###
Security policies specify what employees need to do to ensure that the network is secure. This includes policies regarding user identification and authentication, password length, complexity and refresh interval, acceptable behavior, and remote access requirements. Standard Operating Procedures (SOP) define procedures that must be followed for replacing network devices, installing or removing software applications, new employee onboarding, and employee termination. Guidelines are suggestions for proper procedure that exist when no SOPs are defined.

A help desk is a specialized team of IT professionals that are the central point of contact for employees and customers who need technical assistance. Help desks use communication tools such as chat, telephone, or email to receive issues from customers and facilitate the troubleshooting process. A ticketing system is used to manage “trouble tickets” that consist of details of the issues that users report. Users initiate the tickets, and technicians validate the issues, work with users to address the issues, and escalate the tickets if a higher degree of expertise is required to resolve the issues.

A support technician should always be considerate and should empathize with users, who may be under stress and anxious to resolve a problem quickly. Technicians should never belittle, insult, talk down to, or accuse users of causing the problem.

The know, relate, and understand skill set is a useful way to relate to customers. To know the customer, call them by their name or ask if there is another name that you can use. To better relate to the customer, attempt to create a one-on-one connection. And to understand the customer, determine their level of technical knowledge as a way to speak to them at an appropriate level. Questioning is important using either open-ended or closed-ended questions. Active listening entails using understanding responses as users talk and summarizing what they tell you to verify your understanding.

When addressing an issue with hosts, gather information about the device, operating system, network environment, and the results of connectivity tests, such as **ping** and **tracert**. Other sources of information are beep codes, event viewer logs, device manager settings, task manager data, and diagnostic tool results.

For Cisco device related tickets, use IOS commands, packet captures, and device logs to gather information. IOS commands for connectivity testing, such as **ping** and **traceroute** are useful. Secure Shell (SSH) is the preferred way to connect to the IOS CLI remotely because Telnet is not secure. IOS show commands, such as **show ip interface brief**, **show ip route**, and **show protocols** are useful also.

The next step in the troubleshooting process is to analyze the information that you have gathered and solve the problem. You can consult the ticket system software to locate similar issues, access vendor information resources and FAQs, and search the internet for relevant information. If you can’t solve the problem, then you should escalate it to a higher-level technician for resolution.
### Troubleshoot endpoit conectivility ###
To verify the network configuration of a Windows host, check the status of the connections in Network and Sharing center. You can also use **ipconfig /all** to display this information. Use ping and traceroute or tracert to test connectivity.

On a Linux host, you can view active connections in the GUI or use the **ifconfig** command in a terminal. In addition to ping and traceroute, other command line tools such as **speedtest** and **ncat** (nc) are available for network testing.

In MacOS, open **Network Preferences** > **Advanced** to get IP addressing information. The **ifconfig** command can issued from a terminal as well. Other useful commands are **networksetup -listallnetworkservices** and **networksetup -getinfo <**_network service_**>**. The Linux commands mentioned above are also available in MacOS. The MacOS wireless diagnostics tool can also help solve connectivity problems.

Apple iOS networking can be verified by accessing the Wi-Fi settings. In Android, information about the device addressing and connections can be accessed from the **About phone** > **Status** settings. Third party apps are available that enhance networks diagnostics for Android.
### Troubleshoot a network ###
To gather information to troubleshoot a network problem, Cisco IOS devices have many show commands that can provide detailed information. The Cisco IOS software separates management access into two privilege levels: User EXEC, which is lower-level, and Privileged EXEC, which has full privileges. Use the **enable** command to enter Cisco Privileged EXEC mode. IOS context sensitive help can be used to locate commands and get information about their usage. Context-sensitive help is available by entering a “**?**” at an empty prompt or after a command.

Packet capture and protocol analysis applications enable you to investigate packet content as it flows through the network. The software decodes the protocol layers housed within a packet. Wireshark is an example of a popular open source packet capture/protocol analysis application.

Bandwidth and throughput are characteristics of network data flow. Bandwidth is the theoretical amount of data that can be transmitted from one device to another in an amount of time. Bandwidth is typically measured in the number of bits per second. Throughput is the measurement of the actual number of bits per second that are being transmitted across the media. Throughput is always lower than bandwidth because of latency and delay. Online internet speed test tools and the **iPerf** Windows tool enable measurement of throughput.
### Troubleshoot conectivity remotely ###

When assisting remote users, it may be more efficient to use remote desktop applications. These applications allow a technician to take control of a remote desktop to investigate issues and make configuration changes. Remote desktop applications can create security vulnerabilities and many organizations have desktop sharing disabled on computers. Microsoft Remote Desktop is included in all pro versions of Windows. Apple Remote Desktop and TeamViewer are examples of other remote desktop software.

Telnet, SSH, and Remote Desktop Protocol (RDP) are protocols for remote access to systems. Telnet is an old virtual terminal application that is used to access the command line of a remote system. It uses TCP port 23. Telnet has no mechanism for encrypting transmitted data, and so should not be used. SSH, much like Telnet, enables virtual terminal sessions, however it includes encryption and should be used instead of Telnet. Virtual terminal clients such as PuTTY and Tera Term are available for connection to Telnet and SSH servers.

RDP was created by Microsoft. It also uses a client-server model in which the client accesses an operating system GUI on a remote computer. RDP software is available with Windows, OS X, Linux, and Unix via xrdp. For macOS, remote desktop functionality is provided by Virtual Network Computing (VNC) software.

Virtual Private Networks (VPN) enable secure remote network access over unsecured networks like the internet. A VPN uses dedicated secure connections that encrypt network traffic. Site-to-site VPNs connect entire remote facilities. Remote-access VPNs connect individual users to the corporate network. Remote access VPN users connect to a corporate network VPN gateway using a software client such as Cisco AnyConnect. Microsoft Windows has its own VPN client.

Network management refers to the process of configuring, monitoring, and managing the performance of a network. Modern network management platforms provide advanced analytics, machine learning, and intelligent automation to continually optimize network performance. Network management systems typically use Simple Network Management Protocol (SNMP) and Remote Network Monitoring (RMON) to gather information. Network management systems can be deployed in cloud-based or on-premises models. Cloud-based deployments are good for distributed environments that are geographically dispersed. On-premises systems require a lot of computing power and storage but are good for situations where compliance with data-sovereignty regulations is required. Cisco Meraki is a leading cloud-based network management platform that provides powerful network management capabilities without consuming user bandwidth.

Network automation is the process of automating the configuring, managing, testing, deploying, and operating of physical and virtual devices within a network. Common labor-intensive tasks can be automated using scripts and network programmability. Python is a popular scripting language for network automation.



## 2 Cybersecurity Threats, Vulnerabilities, and Attacks ##

### Common threats  ###
A threat domain is an area of control, authority, or protection that attackers can exploit to gain access to a system. Attackers can exploit systems within a threat domain by gaining physical access to systems, breaking into wireless networks, compromising Bluetooth and NFC devices, sending malicious emails, scrutinizing social media accounts, spreading malicious software (malware) through removable media, or exploiting cloud computing environments.

Attacks can exploit software bugs or human error. Attacks threaten physical systems through sabotage or theft. In addition, equipment failures, utility interruptions, and natural disasters can impact the availability of systems and resources. Internal threats are usually from former or current employees, while external threats come from amateur or skilled attackers.

The user domain includes anyone with access to an organization’s information system, including employees, customers, and contract partners. Users are often considered to be the weakest link in information security systems. User threats come from a lack of security awareness, poorly enforced security policies, data theft, unauthorized downloads and media, visits to unauthorized websites, or intentional destructions of systems, applications, and data.

Threats to devices include unauthorized access to unattended systems, downloading of malware, and out of date software.

Threats to the LAN include unauthorized access to facilities and equipment, operating system vulnerabilities, rogue access points, interception of data in transit, and inefficient management practices. Misconfigured security devices, such as firewalls, can also be exploited.

Threats to the private cloud include unauthorized network probing and port scanning, unauthorized access to resources, vulnerabilities in device software, configuration errors, and unauthorized access to internal resources through the cloud.

The application domain includes all critical systems, applications, and data used by an organization to support operations. Threats to the application domain include unauthorized access, server downtime or hardware failure, network operating system vulnerabilities, data loss, and vulnerabilities in web applications or client-server software.

Complex threats take the form of advanced persistent threats (APT) or algorithm attacks. APTs take place over an extended period and use elaborate tactics and malware. Algorithm attacks exploit software processes to generate behaviors that were not intended by the software developers.

Backdoors, such as Netbus or Back Orifice, are used to gain ongoing unauthorized access to systems by bypassing normal authentication procedures. They typically involve the use of remote administrative tools (RAT) to gain access to systems. Rootkits are a type of malware that exploits vulnerabilities to gain unauthorized access (privilege escalation). Rootkits can modify system files and interfere with system forensics and monitoring tools. They are very difficult to detect and remove.

The United States Computer Emergency Readiness Team (US-CERT) and the U.S. Department of Homeland Security sponsor a database of common vulnerabilities and exposures (CVEs). CVE identifiers are a standard way to refer to known security vulnerabilities. The Dark Web is used by hackers to exchange vulnerability and threat information and stolen data. Security professionals use CVEs and Dark Web resources to research security threats. Indicators of Compromise (IOCs) are the characteristics of attacks that can be used to identify exploits. Automated Indicator Sharing (AIS) provides a standard way for security professionals to exchange exploit information using the Structured Threat Information Expression (STIX) and Trusted Automated Exchange of Intelligence Information (TAXII) are standards.
### Deception ###
Social engineering is a non-technical strategy that attempts to manipulate individuals into performing risky actions or divulging confidential information. Pretexting is a social engineering attack in which someone lies to gain access to confidential data. A something-for-something attack uses the offer of a gift for confidential information. Identify fraud is the use of a person’s stolen confidential information to acquire goods or services.

Social engineering uses a number of tactics to gain cooperation from victims. Attackers may pretend to be persons of authority or use intimidation to compel people to act in ways that compromise security. They may also use tactics such as consensus, scarcity, urgency, and familiarity. Attackers will even develop a relationship of trust with a victim in order to eventually violate the victim’s security.

Shoulder surfing refers to looking over someone’s shoulder in order to obtain credentials like passwords, PINs, or credit card numbers. Dumpster diving means literally going through someone’s trash to find confidential personal information. Piggybacking and tailgating are ways to gain unauthorized physical access to restricted areas.

Other means of deception are sending fake invoices to get money or credentials, watering hole attacks in which popular websites are infected with malware, typo squatting by creating URLs that look very close to popular websites, prepending by removing email external site warnings, and concerted influence campaigns.

Organizations can defend against deception by teaching employees never to provide confidential information to unknown parties, detect suspicious emails and resist clicking links, avoid or terminate uninitiated or automatic downloads, and resist pressure by unknown individuals.
### Cyber attacks ###
Malware is software that can steal data, bypass access controls, or cause harm to or compromise a system. Viruses are a type of malware that replicates itself when executed. They can be harmless or destructive. Worms are programs that replicate independently across networks. Trojan horses are malware that masquerade as other software applications or are distributed with legitimate applications. Logic bombs are triggered to act by date and time or other system events. They can damage system hardware and software. Ransomware is a common attack that uses malicious software to encrypt a system hardware drive. Sometimes, but not always, paying a ransom will reverse the damage.

Denial of service (DoS) attacks are a type of network attack that affects the availability of resources. In one type of DoS attack a network or application is overwhelmed with an enormous amount of data. This can make systems slow or crash. In another DoS attack, maliciously formatted packets are sent to disrupt system operation.

The Domain Name System (DNS) is essential to network operations. Attackers can damage the reputation of a domain by creating bogus similar domains or through false news. In domain spoofing, attackers exploit weaknesses in DNS to map legitimate domain names to the IP addresses of malicious websites. If attackers gain access to a target’s DNS registration information, they can hijack the domain name by changing the domain name to IP address mappings.

Two common types of Layer 2 attacks are spoofing and MAC flooding. MAC address spoofing occurs when an attacker disguises their device as a valid one on the network and can therefore bypass the authentication process. ARP spoofing sends spoofed ARP messages across a LAN to link an attacker’s MAC address to the IP address of an authorized device on the network. IP spoofing sends IP packets from a spoofed source address in order to disguise the packet origin. In MAC flooding, an attacker floods the network with fake MAC addresses, compromising the security of the network switch.

Man-in-the-Middle (MitM), or on-path attacks, happen when a cybercriminal takes control of an intermediate device in the network, or puts their own device on a path to intercept user data. The attacker can steal information, manipulate data, or relay false information. A Man-in-the-Mobile (MitMo) attack is a variation of an MitM attack in which a mobile device is infected with malware that steals data from the device.

Zero-day attacks exploit software vulnerabilities before they become widely known to the public. A sophisticated and holistic view of the security infrastructure is required to defend against these attacks.

Keyboard loggers are types of malware that record every keystroke made on a computer. This can reveal confidential information and account credentials.

Several guidelines for defending against attacks are to configure firewalls to filter packets incoming packets that appear to have originated internally, ensure all software has the most recent updates and patches, distribute workloads between multiple server systems, and block ICMP packets at the network edge.
### Wireless and mobile devices attacks ###
Grayware is an unwanted application that behaves in an annoying or undesirable manner. SMiShing is the use of fake SMS messages to lure the user to visit a malicious website or call a fraudulent phone number.

Rogue Access Points are installed on networks without authorization. They can masquerade as legitimate access points to trick users into associating with them. They can be used to conduct MitM attacks by deauthenticating users or posing as legitimate access points with more desirable connections in evil twin attacks.

Wireless signals are susceptible to interference and jamming. Attackers can deny wireless service by jamming Wi-Fi signals. Bluetooth can be used to send unauthorized messages through Bluejacking. Bluesnarfing occurs when an attacker copies information from a mobile device through a malicious Bluetooth connection.

Wired equivalent privacy (WEP) and Wi-Fi protected access (WPA) are security protocols that were designed to secure wireless networks. WEP had no provision for key management and so it was vulnerable to attack. To address this and replace WEP, WPA and then WPA2 were developed as improved wireless security protocols.

To enhance wireless security, it is important to use at least WPA2 encryption. Access points should be placed outside of the network perimeter, if possible. Use tools like NetStumbler to detect rogue access points. Permit only secure Wi-Fi guest access. Finally, employees should always use remote access VPN when connecting to the organization’s network over public Wi-Fi networks.
### Appllication attacks ###
Cross-site scripting (XSS) is a common web application attack in which malicious code is inserted into a legitimate website. The victim’s browser executes the malicious code which downloads malware, redirects to a malicious website, or steals information.

Injection attacks involve exploiting systems by inserting malformed data or commands in user input fields. They are especially common against databases. XML and SQL injection attacks corrupt databases or cause sensitive information, such as user credentials, to be revealed. Dynamic link libraries (DLL) are software modules that are used by applications to interact with Windows. Attackers can inject malicious code into DLLs that will then execute when the DLL is used. LDAP injection attacks exploit input validation to execute queries on LDAP servers, potentially giving attackers access to sensitive account information.

Remote code execution allows a cybercriminal to take advantage of application vulnerabilities to execute commands with the privileges of the user running the application on the target device. Other application attacks are cross-site request forgeries, race condition attacks, improper user input attacks, error handling attacks, and application programming interface (API) attacks. Additional attacks are replay attacks, directory traversal attacks, and resource exhaustion attacks.

To defend against application attacks, the first line of defense is to write solid code. All user input should be validated. Security testing tools should be used to evaluate code as it is developed and prior to deployment. Finally, all software, including operating systems, should be kept up to date.

Spam, also known as junk mail, is simply unsolicited email. Spam is usually a nuisance, but it can be malicious. Although spam filters are widely used, it is important that users know how to identify spam.

Phishing and spear phishing are attacks that appear to come from legitimate sources but want you to download files or submit confidential information. Spear phishing attacks are directly targeted at specific individuals. Vishing uses voice messages to attack. Pharming directs users to fake versions of legitimate websites. Whaling is phishing directed at high-profile users like executives, politicians, or celebrities.

To defend against email and browser attacks, organizations should use spam filters, deploy antivirus software, and educate users about network security.


# 2 Cybersecurity threats, vulnerabilities and attacks #

## Common threats ##
A threat domain is an area of control, authority, or protection that attackers can exploit to gain access to a system. Attackers can exploit systems within a threat domain by gaining physical access to systems, breaking into wireless networks, compromising Bluetooth and NFC devices, sending malicious emails, scrutinizing social media accounts, spreading malicious software (malware) through removable media, or exploiting cloud computing environments.

Attacks can exploit software bugs or human error. Attacks threaten physical systems through sabotage or theft. In addition, equipment failures, utility interruptions, and natural disasters can impact the availability of systems and resources. Internal threats are usually from former or current employees, while external threats come from amateur or skilled attackers.

The user domain includes anyone with access to an organization’s information system, including employees, customers, and contract partners. Users are often considered to be the weakest link in information security systems. User threats come from a lack of security awareness, poorly enforced security policies, data theft, unauthorized downloads and media, visits to unauthorized websites, or intentional destructions of systems, applications, and data.

Threats to devices include unauthorized access to unattended systems, downloading of malware, and out of date software.

Threats to the LAN include unauthorized access to facilities and equipment, operating system vulnerabilities, rogue access points, interception of data in transit, and inefficient management practices. Misconfigured security devices, such as firewalls, can also be exploited.

Threats to the private cloud include unauthorized network probing and port scanning, unauthorized access to resources, vulnerabilities in device software, configuration errors, and unauthorized access to internal resources through the cloud.

The application domain includes all critical systems, applications, and data used by an organization to support operations. Threats to the application domain include unauthorized access, server downtime or hardware failure, network operating system vulnerabilities, data loss, and vulnerabilities in web applications or client-server software.

Complex threats take the form of advanced persistent threats (APT) or algorithm attacks. APTs take place over an extended period and use elaborate tactics and malware. Algorithm attacks exploit software processes to generate behaviors that were not intended by the software developers.

Backdoors, such as Netbus or Back Orifice, are used to gain ongoing unauthorized access to systems by bypassing normal authentication procedures. They typically involve the use of remote administrative tools (RAT) to gain access to systems. Rootkits are a type of malware that exploits vulnerabilities to gain unauthorized access (privilege escalation). Rootkits can modify system files and interfere with system forensics and monitoring tools. They are very difficult to detect and remove.

The United States Computer Emergency Readiness Team (US-CERT) and the U.S. Department of Homeland Security sponsor a database of common vulnerabilities and exposures (CVEs). CVE identifiers are a standard way to refer to known security vulnerabilities. The Dark Web is used by hackers to exchange vulnerability and threat information and stolen data. Security professionals use CVEs and Dark Web resources to research security threats. Indicators of Compromise (IOCs) are the characteristics of attacks that can be used to identify exploits. Automated Indicator Sharing (AIS) provides a standard way for security professionals to exchange exploit information using the Structured Threat Information Expression (STIX) and Trusted Automated Exchange of Intelligence Information (TAXII) are standards.
## Deception ##
Social engineering is a non-technical strategy that attempts to manipulate individuals into performing risky actions or divulging confidential information. Pretexting is a social engineering attack in which someone lies to gain access to confidential data. A something-for-something attack uses the offer of a gift for confidential information. Identify fraud is the use of a person’s stolen confidential information to acquire goods or services.

Social engineering uses a number of tactics to gain cooperation from victims. Attackers may pretend to be persons of authority or use intimidation to compel people to act in ways that compromise security. They may also use tactics such as consensus, scarcity, urgency, and familiarity. Attackers will even develop a relationship of trust with a victim in order to eventually violate the victim’s security.

Shoulder surfing refers to looking over someone’s shoulder in order to obtain credentials like passwords, PINs, or credit card numbers. Dumpster diving means literally going through someone’s trash to find confidential personal information. Piggybacking and tailgating are ways to gain unauthorized physical access to restricted areas.

Other means of deception are sending fake invoices to get money or credentials, watering hole attacks in which popular websites are infected with malware, typo squatting by creating URLs that look very close to popular websites, prepending by removing email external site warnings, and concerted influence campaigns.

Organizations can defend against deception by teaching employees never to provide confidential information to unknown parties, detect suspicious emails and resist clicking links, avoid or terminate uninitiated or automatic downloads, and resist pressure by unknown individuals.
## Cyber attacks ##
Malware is software that can steal data, bypass access controls, or cause harm to or compromise a system. Viruses are a type of malware that replicates itself when executed. They can be harmless or destructive. Worms are programs that replicate independently across networks. Trojan horses are malware that masquerade as other software applications or are distributed with legitimate applications. Logic bombs are triggered to act by date and time or other system events. They can damage system hardware and software. Ransomware is a common attack that uses malicious software to encrypt a system hardware drive. Sometimes, but not always, paying a ransom will reverse the damage.

Denial of service (DoS) attacks are a type of network attack that affects the availability of resources. In one type of DoS attack a network or application is overwhelmed with an enormous amount of data. This can make systems slow or crash. In another DoS attack, maliciously formatted packets are sent to disrupt system operation.

The Domain Name System (DNS) is essential to network operations. Attackers can damage the reputation of a domain by creating bogus similar domains or through false news. In domain spoofing, attackers exploit weaknesses in DNS to map legitimate domain names to the IP addresses of malicious websites. If attackers gain access to a target’s DNS registration information, they can hijack the domain name by changing the domain name to IP address mappings.

Two common types of Layer 2 attacks are spoofing and MAC flooding. MAC address spoofing occurs when an attacker disguises their device as a valid one on the network and can therefore bypass the authentication process. ARP spoofing sends spoofed ARP messages across a LAN to link an attacker’s MAC address to the IP address of an authorized device on the network. IP spoofing sends IP packets from a spoofed source address in order to disguise the packet origin. In MAC flooding, an attacker floods the network with fake MAC addresses, compromising the security of the network switch.

Man-in-the-Middle (MitM), or on-path attacks, happen when a cybercriminal takes control of an intermediate device in the network, or puts their own device on a path to intercept user data. The attacker can steal information, manipulate data, or relay false information. A Man-in-the-Mobile (MitMo) attack is a variation of an MitM attack in which a mobile device is infected with malware that steals data from the device.

Zero-day attacks exploit software vulnerabilities before they become widely known to the public. A sophisticated and holistic view of the security infrastructure is required to defend against these attacks.

Keyboard loggers are types of malware that record every keystroke made on a computer. This can reveal confidential information and account credentials.

Several guidelines for defending against attacks are to configure firewalls to filter packets incoming packets that appear to have originated internally, ensure all software has the most recent updates and patches, distribute workloads between multiple server systems, and block ICMP packets at the network edge.
## Wireless and mobile devices ##
Grayware is an unwanted application that behaves in an annoying or undesirable manner. SMiShing is the use of fake SMS messages to lure the user to visit a malicious website or call a fraudulent phone number.

Rogue Access Points are installed on networks without authorization. They can masquerade as legitimate access points to trick users into associating with them. They can be used to conduct MitM attacks by deauthenticating users or posing as legitimate access points with more desirable connections in evil twin attacks.

Wireless signals are susceptible to interference and jamming. Attackers can deny wireless service by jamming Wi-Fi signals. Bluetooth can be used to send unauthorized messages through Bluejacking. Bluesnarfing occurs when an attacker copies information from a mobile device through a malicious Bluetooth connection.

Wired equivalent privacy (WEP) and Wi-Fi protected access (WPA) are security protocols that were designed to secure wireless networks. WEP had no provision for key management and so it was vulnerable to attack. To address this and replace WEP, WPA and then WPA2 were developed as improved wireless security protocols.

To enhance wireless security, it is important to use at least WPA2 encryption. Access points should be placed outside of the network perimeter, if possible. Use tools like NetStumbler to detect rogue access points. Permit only secure Wi-Fi guest access. Finally, employees should always use remote access VPN when connecting to the organization’s network over public Wi-Fi networks.
## Application attacks ##
Cross-site scripting (XSS) is a common web application attack in which malicious code is inserted into a legitimate website. The victim’s browser executes the malicious code which downloads malware, redirects to a malicious website, or steals information.

Injection attacks involve exploiting systems by inserting malformed data or commands in user input fields. They are especially common against databases. XML and SQL injection attacks corrupt databases or cause sensitive information, such as user credentials, to be revealed. Dynamic link libraries (DLL) are software modules that are used by applications to interact with Windows. Attackers can inject malicious code into DLLs that will then execute when the DLL is used. LDAP injection attacks exploit input validation to execute queries on LDAP servers, potentially giving attackers access to sensitive account information.

Remote code execution allows a cybercriminal to take advantage of application vulnerabilities to execute commands with the privileges of the user running the application on the target device. Other application attacks are cross-site request forgeries, race condition attacks, improper user input attacks, error handling attacks, and application programming interface (API) attacks. Additional attacks are replay attacks, directory traversal attacks, and resource exhaustion attacks.

To defend against application attacks, the first line of defense is to write solid code. All user input should be validated. Security testing tools should be used to evaluate code as it is developed and prior to deployment. Finally, all software, including operating systems, should be kept up to date.

Spam, also known as junk mail, is simply unsolicited email. Spam is usually a nuisance, but it can be malicious. Although spam filters are widely used, it is important that users know how to identify spam.

Phishing and spear phishing are attacks that appear to come from legitimate sources but want you to download files or submit confidential information. Spear phishing attacks are directly targeted at specific individuals. Vishing uses voice messages to attack. Pharming directs users to fake versions of legitimate websites. Whaling is phishing directed at high-profile users like executives, politicians, or celebrities.

To defend against email and browser attacks, organizations should use spam filters, deploy antivirus software, and educate users about network security.
# 3 Network Security Summary #
## Security foundations ##
The cybersecurity cube provides a useful way to think about protecting data. The first dimension of the cube identifies the goals of confidentiality, integrity, and availability (CIA). Confidentiality concerns preventing disclosure of information to unauthorized persons. Integrity refers to the accuracy, consistency, and trustworthiness of data. Data states are in transit, at rest in storage, or in process. The pillars of defense are people, technology, and policies and practices. Confidentiality, integrity, and availability are also referred to as the CIA triad.

Data integrity ensures that data is unaltered by unauthorized entities while it is captured, stored, retrieved, updated, and transferred. Methods used to ensure data integrity include hashing, data validation checks, data consistency checks, and access controls.

Availability refers to the need to make data accessible to all authorized users whenever they need it. Cyberattacks and system failures can disconnect users from the data they need. Availability can be ensured by properly maintaining equipment, keeping software and systems up to date, testing backups and fallbacks, implementing new technologies, monitoring network activity, and analyzing vulnerabilities to detect threats.
## Access control ##
Physical access controls prevent unauthorized users from physically accessing networks, data, and equipment. Physical access controls determine who, where, and when people can enter or exit a facility. Physical access controls include guards, perimeter fences, motion detectors, devices locks, and locked doors that can only be accessed with swipe cards or combinations. Additional physical security measures are guard dogs, video cameras, and alarms.

Logical access controls are the hardware and software solutions used to manage access to resources and systems. These technology-based solutions include tools and protocols that computer systems use for identification, authentication, authorization, and accounting (AAA). Examples of these controls are encryption, smart cards with embedded chips, passwords, biometrics, access control lists (ACLs), firewalls and intrusion detection systems.

Administrative access controls are the policies and procedures defined by organizations to implement and enforce all aspects of controlling unauthorized access. Examples are approved policies, defined procedures, background checks, and data classification.

Administrative access controls involve three security services: authentication, authorization, and accounting (AAA). Authentication is the verification of the identity of each user, to prevent unauthorized access. Authorization services determine which resources users can access, along with the operations that users can perform, and even when they can perform them. Accounting keeps track of what users do on the network, such as what they access, when they access it, and what they do with it. This information is compiled in logs.

Identification enforces the rules established by the authorization policy. Unique identifiers are usernames and passwords, personal identification numbers, or biometrics such as fingerprints, retina scans, or voice recognition.

Federated identity management (FIM) refers to multiple enterprises that let their users use the same identification credentials to gain access to the networks of all enterprises in the group. While FIM provides convenience to users and administrators, if the system is exploited by hackers, they will have access to many systems or applications instead of just one.

Password policies help ensure that passwords meet length and complexity requirements. Passwords should be at least 8 to 10 characters. Passwords should include a mix of upper and lower case characters, numbers, and symbols.

Combining other means of identity with passwords, such as multi-factor authentication, is increasingly popular.

Accounting traces an action back to a person or process. Accounting then collects this information and reports the usage data. The organization can use this data for such purposes as auditing or billing.
## Defending systems and devices ##
An organization needs a good administrator to configure operating systems to protect against outside threats. A systematic approach is required to establish security monitoring procedures, evaluate software updates, and install updates using a documented plan. Baselines help to indicate system compromise when performance deviates significantly from the baseline.

Fileless malware attacks are difficult to detect and leave no footprint. They can exploit scriptable command shells. Python, Bash, and Visual Basic for Applications (VBA) scripts can be malicious.

To stay ahead of cybercriminals, software should be proactively patched to eliminate vulnerabilities. Operating systems regularly check for patches, but administrators should evaluate patches before they are installed. Automated patch management systems provide administrators with control over date and time of updates and reporting about the status of systems and patches.

Host-based endpoint security includes host-based firewalls that can block incoming and outgoing traffic, host intrusion detection systems (HIDS) that monitor systems and login security and system events. Host intrusion prevention systems (HIPS) detect malicious activity and can send you an alarm, log the malicious activity, reset the connection, and/or drop the packets. Endpoint detection and response (EDR) is an integrated security solution that continuously monitors and collects data from endpoint devices. Data loss prevention (DLP) tools provide a centralized way to ensure that sensitive data is not lost, misused, or accessed by unauthorized users. Next-generation firewalls (NGFW) combine traditional firewalls with other network-device-filtering functions.

Data can be protected through host encryption by Windows Encrypting Files System (EFS) that can encrypt files or entire drives (full-disk encryption – FDE) with BitLocker. BitLocker requires a Trusted Platform Module (TPM) in BIOS. BitLocker To Go is a tool that encrypts removable drives.

Boot integrity ensures that the system can be trusted and has not been altered while the operating system loads. Secure Boot is a security standard to ensure that a device boots using trusted software.

Apple provides system hardware and macOS security features that offer robust endpoint protection. The Mac hardware platform has enhanced security features such as a special security processor, boot integrity, and a dedicated AES encryption engine. Apple Data Protection and FileVault data storage encryption are supported by the hardware-based AES encryption engine. Biometric data is processed in security hardware, isolating it from the operating system. Apple also includes a find my device feature, XProtect antimalware technology, a malware removal tool (MRT), and Gatekeeper, which ensures that only authentic, digitally-signed Apple software can be installed.

Physical protection of devices includes controlling access to equipment and facilities, using cable locks, keyed or cipher door locks, and device inventory and tracking with radio frequency identification systems (RFID).
## Antimalware protection ##
Various network security devices are required to protect the network perimeter from outside access. These devices could include a hardened router that is providing VPN services, a next generation firewall, an IPS appliance, and a AAA services server. However, securing an internal LAN is nearly as important as securing the outside network perimeter. Endpoints and the network infrastructure require protection.

There are three types of antimalware programs; signature-based, heruistics-based, and behavior-based. Host-based antivirus protection is also known as agent-based. Agent-based antivirus runs on every protected machine. Agentless antivirus protection performs scans on hosts from a centralized system. Host-based firewalls restrict incoming and outgoing connections to connections initiated by that host only. Examples are Windows Defender Firewall with Advanced Security and iptables and TCP Wrappers on Linux.

Protecting endpoints in a borderless network can be accomplished using network-based, as well as host-based techniques. Devices and techniques that implement host protections at the network level include Cisco Secure Endpoint, Cisco Secure Email, Cisco Umbrella, and Network Admission (NAC) and Control systems. These technologies work together with host-based systems to secure the enterprise.
## Firewalls and Host Based intrusion protection ##
Firewalls resist network attacks, serve as the only point between internal and external networks, and enforce access control policies. They protect hosts from exposure, sanitize protocol flow, and block malicious data from servers and clients. Firewalls are ineffective if misconfigured or out of date. They can slow networks and some data cannot be passed over them.

There are various types of firewalls. Packet filtering (stateless) firewalls are usually part of a router firewall. They permit or deny traffic based on Layer 3 and Layer 4 information. Stateful firewalls are the most versatile and the most common firewall technologies in use. Stateful filtering is a firewall architecture that is classified at the network layer. It also analyzes traffic at OSI Layer 4 and Layer 5. An application gateway firewall (proxy firewall) filters information at Layers 3, 4, 5, and 7 of the OSI reference model. Next-generation firewalls (NGFW) go beyond stateful firewalls. Transparent firewalls filter traffic between two bridged interfaces. Hybrid firewalls combine attributes of the other firewall types.

Packet filtering firewalls are usually part of a router firewall. They use simple permit or deny rules, have low impact on network performance, are easy to implement and provide initial security at the network layer. They are susceptible to IP spoofing, may not be effective against fragmented packets, and can use complex ACLs that are difficult to use and maintain. Stateful firewalls are often the primary means of defense by filtering unwanted, unnecessary, and undesirable traffic. They are generally more effective than stateless firewalls. However, they cannot prevent application layer attacks, are less effective against stateless protocols, have difficulty tracking dynamic port negotiation, and do not use authentication.

Host-based personal firewalls are standalone software programs that control traffic entering or leaving a computer. Host-based firewalls may use a set of predefined policies, or profiles, to control packets entering and leaving a computer. They also may have rules that can be directly modified or created to control access based on addresses, protocols, and ports. Examples include Windows Defender Firewalls, iptables, nftables and TCP Wrappers.

Antimalware protection consists of antivirus, adware, phishing, and spyware protection. Some antimalware software combines features of the different types.
## Secure wireless access ##
Wireless networks are susceptible to a number of threats, including: interception of data, wireless intruders, DoS attacks, and rogue APs. DoS attacks can result from improperly configured devices, malicious user interference, and accidental interference. Rogue APs can be used by an attacker to capture MAC addresses, capture data packets, gain access to network resources, or launch a man-in-the-middle (MitM) attack. In an MitM, attack, the hacker is positioned between two legitimate entities in order to read or modify the data that passes between the two parties.

In SSID cloaking, the SSID beacon frame is disabled. For MAC address filtering, an administrator can manually permit or deny clients wireless access based on their physical MAC hardware address.

Open system authentication should only be used in situations where security is of no concern. Shared key authentication provides mechanisms such as WEP, WPA, WPA2, and WPA3 to authenticate and encrypt data between a wireless client and AP. WEP and WPA authentication are outdated and insecure. WPA2 is recommended at a minimum with WPA3 preferred when it becomes available.

Personal authentication requires configuration of a username and pre-shared password. Enterprise authentication requires the use of a RADIUS authentication server using 802.1x with Extensible Authentication Protocol (EAP).

Encryption protects data by making it unreadable if intercepted. WPA2 uses Temporal Key Integrity Protocol (TKIP) or Advanced Encryption Standard (AES).

WPA3, when available, is the recommended 802.11 authentication method. It includes WPA3-Personal, WPA3-Enterprise, Open Networks, and IoT onboarding. WPA3 open or public Wi-Fi networks still do not use any authentication. However, they do use Opportunistic Wireless Encryption (OWE) to encrypt all wireless traffic. For IoT onboarding, WPA3 uses Device Provisioning Protocol (DPP) to securely onboard IoT devices.