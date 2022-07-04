---
description: >-
  https://www.crest-approved.org/examination/practitioner-security-analyst/index.html
---

# Crest CPSA

CREST has a good reputation and market in the UK Penetration Testing industry, and in order to become CREST Registered Tester (CRT) also known as "CREST Registered Penetration Tester", you are first required to pass the theory part of the test which is the CPSA exam.

CPSA covers 10 domains, for each domain I will provide a link and/or notes of the additional resources I have used for each domain, make sure to go through all resources

### Software Skills and Assessment Management

This is basically a common-sense topic, non-technical, and you are expected to understand how to scope a Penetration Testing, the UK Law & Compliance that impact Penetration Testing activities and the concept of infrastructure testing, application testing, black box, grey box and white box formats of testing.

Resource: [Penetration Testing Tutorial](https://www.tutorialspoint.com/penetration\_testing/penetration\_testing\_introduction.htm)

### Core Technical Skills

A very important and comprehensive section, here you are expected to understand common IP Protocols such as IPv4, TCP, UDP and ICMP, understand output of network sniffers and scanners such as Nmap, Wireshark and TCPDump.

This section also covers Cryptography such as the difference between encryption and encoding, symmetric / asymmetric, encryption algorithms DES, 3DES, AES, RSA, RC4, hashes SHA1 and MD5, Message Integrity Codes HMAC and the applications of Cryptography on SSL, IPSec, SSH, PGP, common wireless encryption WEP, WPA and TKIP.

To finalise this section also covers Linux and Windows file system permissions.

Go through each section in a very slow pace, there are no need to rush, make sure to clearly understand each topic described on the videos.

Resources: Go through [Section 1 - Networking Concepts](https://www.professormesser.com/network-plus/n10-007/n10-007-training-course/), [Section 6 - Cryptography and PKI](https://www.professormesser.com/security-plus/sy0-501/sy0-501-training-course/), [Linux File Permissions](https://www.youtube.com/watch?v=BmVmJi5dR9c) and [Windows File Permissions](https://www.youtube.com/watch?v=XQNYkUwmV5E)

### Background Information Gathering & Open Source

A section that pretty much talks about WHOIS and DNS, I would say this isn't a hard section and you should be able to master it in very few hours if you practise "hands-on", make sure to have an up and running Kali box where you would be able to test all the commands.

Resource: Sign-up for [Cybrary](https://www.cybrary.it/), go to the "Advanced Penetration Testing" course, then go to Module 4 "Information Gathering (part 2) Domain Name Services".

### Networking Equipment

For this section the [3rd Edition - Network Security Assessment Know Your Network](https://www.amazon.co.uk/Network-Security-Assessment-Know-Your/dp/149191095X/ref=dp\_ob\_title\_bk) will pretty much give all that you need.

I recommend that you concentrate your attention and efforts on how to fingerprint and enumerate common vulnerabilities within network management protocol such as Telnet, SSH, SNMP, TFTP and NTP using Nmap.

Furthermore, you should know common security issues relating to ARP, DHCP, CDP and STP.

To finalise, make sure to do understand the basics of how IPSec and SIP works.

Resources: Go through the [IPsec from Professor Messer](https://www.professormesser.com/network-plus/n10-005/ipsec-2/), [Management Protocols](https://www.professormesser.com/network-plus/n10-005/application-protocols/), [Networking Protocols](https://www.professormesser.com/network-plus/n10-005/networking-protocols/) and [Application Protocols](https://www.professormesser.com/network-plus/n10-005/application-protocols/)

### Microsoft Windows Security Assessment

After covering a lot of network, it is time to tackle the basics of Windows enumeration and exploitation.

This domain will cover the basic on how to identify and enumerate Windows environment, including Active Directory and SMB shares.

Also, you are expected to understand how Windows store and encrypt password.

Finally, you need to understand common Windows vulnerabilities which exploit code exists in the public domain.

Resources: Go through [A Little Guide to SMB Enumeration from Hacking Articles](https://www.hackingarticles.in/a-little-guide-to-smb-enumeration/), learn all commands mentioned on Section 3 of [Windows Command List](http://www.networkpentest.net/p/windows-command-list.html), read about [NT LAN Manager](https://en.wikipedia.org/wiki/NT\_LAN\_Manager) and [Security Account Manager](https://en.wikipedia.org/wiki/Security\_Account\_Manager)

### Unix Security Assessment

Here you are expected to identify, enumerate and understand common Unix services and vulnerabilities.

This domain will cover some very old technologies such as R\* services, X11 and finger.

Resources: Go through [4 ways to SMTP Enumeration](https://www.hackingarticles.in/4-ways-smtp-enumeration/), [FTP Penetration Testing on Ubuntu](https://www.hackingarticles.in/ftp-penetration-testing-on-ubuntu-port-21/), [Linux Privilege Escalation using Misconfigured NFS](https://www.hackingarticles.in/linux-privilege-escalation-using-misconfigured-nfs/), [R\* services](https://en.wikipedia.org/wiki/Berkeley\_r-commands) and [Penetration Testing on X11 Server](https://www.hackingarticles.in/penetration-testing-on-x11-server/)

### Web Technologies, Web Testing Methodologies and Web Testing Techniques

A HUGE section that covers a lot about web application technologies and testing techniques.

Here you will need to understand common web application vulnerabilities and testing techniques to identify Command Injection, XSS and CSRF within your Web App.

For our very luck, all the additional resources are provided by Cybrary.

Resources: Sign-up for [Cybrary](https://www.cybrary.it/), go to the "Advanced Penetration Testing" course, then go to Module 11 "WebApps", also go to course named "Penetration Testing and Ethical Hacking" and go through Module 10 "Web Servers and Apps" and finally Module 11 "SQL Injection".

### Databases

For this domain you are expected to understand how to identify, enumerate and perform common attack against Microsoft SQL Server and Oracle RDBMS.

This is a very tiny domain, but do not take it less seriously!

Resources: Go through [Pentesters Guide to Oracle Hacking](https://medium.com/@netscylla/pentesters-guide-to-oracle-hacking-1dcf7068d573) and [MSSQL Penetration Testing with Metasploit](https://www.hackingarticles.in/mssql-penetration-testing-metasploit/) "make sure to research and understand each exploit outside of Metasploit"

### Generic Comments and Studying Tips

* Learn acronyms e.g. what DNS stands for? It is very important that you try to remember **all** acronyms mentioned in the [3rd Edition - Network Security Assessment Know Your Network](https://www.amazon.co.uk/Network-Security-Assessment-Know-Your/dp/149191095X/ref=dp\_ob\_title\_bk)
* Do not **skip** old technology, learn about old technologies such as X11, Finger and R\* Services
* Do not rush through the domains and additional resources! Take your time, read, listen and make sure you understand each topic before proceeding to the next domain

### Study resources

#### Flashcards

{% embed url="https://quizlet.com/search?query=cpsa&type=sets" %}

{% embed url="https://quizlet.com/355066589/crest-cpsa-flash-cards" %}

#### Mock Exams

{% embed url="https://www.udemy.com/course/crest-practitioner-security-analyst-cpsa-practice-tests" %}
