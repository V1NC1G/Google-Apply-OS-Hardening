# Google Cybersecurity Professional Certificate - Apply OS Hardening Techniques

## Table of Contents

- [Objectives](#objectives)
- [Scenario](#scenario)
- [tcpdump Traffic Log](#tcpdump-traffic-log)
- [Incident Report](#incident-report)
- [Learning Experience](#learning-experience)

## Objectives

The objective of this activity is to investigate, identify, document and recommend a solution to a security problem encountered by visitors when visiting a company website called yummyrecipesforme.com when loading the main webpage.

The investigation will consist of reading `tcpdump` logs, and identifying network protocols used. This would determine the kind of attack a malicious actor is doing.

## Scenario

You are a cybersecurity analyst for yummyrecipesforme.com, a website that sells recipes and cookbooks. A former employee has decided to lure users to a fake website with malware.

The baker executed a brute force attack to gain access to the web host. They repeatedly entered several known default passwords for the administrative account until they correctly guessed the right one. After they obtained the login credentials, they were able to access the admin panel and change the website’s source code. They embedded a javascript function in the source code that prompted visitors to download and run a file upon visiting the website. After embedding the malware, the baker changed the password to the administrative account. When customers download the file, they are redirected to a fake version of the website that contains the malware.

Several hours after the attack, multiple customers emailed yummyrecipesforme’s helpdesk. They complained that the company’s website had prompted them to download a file to access free recipes. The customers claimed that, after running the file, the address of the website changed and their personal computers began running more slowly.

In response to this incident, the website owner tries to log in to the admin panel but is unable to, so they reach out to the website hosting provider. You and other cybersecurity analysts are tasked with investigating this security event.

To address the incident, you create a sandbox environment to observe the suspicious website behavior. You run the network protocol analyzer tcpdump, then type in the URL for the website, yummyrecipesforme.com. As soon as the website loads, you are prompted to download an executable file to update your browser. You accept the download and allow the file to run. You then observe that your browser redirects you to a different URL, greatrecipesforme.com, which contains the malware.

The logs show the following process:

1. The browser initiates a DNS request: It requests the IP address of the yummyrecipesforme.com URL from the DNS server.
2. The DNS replies with the correct IP address.
3. The browser initiates an HTTP request: It requests the yummyrecipesforme.com webpage using the IP address sent by the DNS server.
4. The browser initiates the download of the malware.
5. The browser initiates a DNS request for greatrecipesforme.com.
6. The DNS server responds with the IP address for greatrecipesforme.com.
7. The browser initiates an HTTP request to the IP address for greatrecipesforme.com.

A senior analyst confirms that the website was compromised. The analyst checks the source code for the website. They notice that javascript code had been added to prompt website visitors to download an executable file. Analysis of the downloaded file found a script that redirects the visitors’ browsers from yummyrecipesforme.com to greatrecipesforme.com.

The cybersecurity team reports that the web server was impacted by a brute force attack. The disgruntled baker was able to guess the password easily because the admin password was still set to the default password. Additionally, there were no controls in place to prevent a brute force attack.

Your job is to document the incident in detail, including identifying the network protocols used to establish the connection between the user and the website. You should also recommend a security action to take to prevent brute force attacks in the future.

## tcpdump Traffic Log

```
14:18:32.192571 IP your.machine.52444 > dns.google.domain: 35084+ A? yummyrecipesforme.com. (24)
14:18:32.204388 IP dns.google.domain > your.machine.52444: 35084 1/0/0 A 203.0.113.22 (40)


14:18:36.786501 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [S], seq 2873951608, win 65495, options [mss 65495,sackOK,TS val 3302576859 ecr 0,nop,wscale 7], length 0
14:18:36.786517 IP yummyrecipesforme.com.http > your.machine.36086: Flags [S.], seq 3984334959, ack 2873951609, win 65483, options [mss 65495,sackOK,TS val 3302576859 ecr 3302576859,nop,wscale 7], length 0
14:18:36.786529 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [.], ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 0
14:18:36.786589 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 73: HTTP: GET / HTTP/1.1
14:18:36.786595 IP yummyrecipesforme.com.http > your.machine.36086: Flags [.], ack 74, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 0


14:20:32.192571 IP your.machine.52444 > dns.google.domain: 21899+ A? greatrecipesforme.com. (24)
14:20:32.204388 IP dns.google.domain > your.machine.52444: 21899 1/0/0 A 192.0.2.17 (40)

14:25:29.576493 IP your.machine.56378 > greatrecipesforme.com.http: Flags [S], seq 1020702883, win 65495, options [mss 65495,sackOK,TS val 3302989649 ecr 0,nop,wscale 7], length 0
14:25:29.576510 IP greatrecipesforme.com.http > your.machine.56378: Flags [S.], seq 1993648018, ack 1020702884, win 65483, options [mss 65495,sackOK,TS val 3302989649 ecr 3302989649,nop,wscale 7], length 0
14:25:29.576524 IP your.machine.56378 > greatrecipesforme.com.http: Flags [.], ack 1, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 0
14:25:29.576590 IP your.machine.56378 > greatrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 73: HTTP: GET / HTTP/1.1
14:25:29.576597 IP greatrecipesforme.com.http > your.machine.56378: Flags [.], ack 74, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 0
```

## Incident Report

The protocol involved with this incident is the **HTTP** protocol. It can be further confirmed that the HTTP protocol was used by reading the logs generated from running tcpdump, indicating that the HTTP protocol was used to access the web server.

Customers raised an issue with the helpdesk team that when accessing the website (yummyrecipesforme.com), they were prompted to download and run a file that contains free recipes. Their machines have been running slowly ever since after downloading the file. The website owner as well was not able to login to their admin account.

tcpdump was ran on a sandbox enviroment by analysts to determine what was causing the problem by reading the logs generated. Upon opening the website, they were also prompted to download a file which redirected the analysts to a spoofed website called _greatrecipesforme.com_.

The tcpdump logs showed that the browser initially requested the IP address of the _yummyrecipesforme.com_ website. After accepting the prompt to download the file, the logs showed a sudden change in network traffic as the browser requested a new IP address for the spoofed website _greatrecipesforme.com_. The network traffic is then rerouted to the new IP address for the spoofed website.

Analyzing the source code for the website and the file showed that the attacker manipulated the code to prompted the users to download a file disguised as a browser update. Since the website owner has been locked out of their admin account, it might have been that the attacker used brute force attack to access the account and change the administrator credentials. Running the downloaded malicious file compromised the end users' computers.

It is recommended to have a stronger and better password policy to be better protected from brute force attacks and not allow previous passwords from being used. Since the vulnerability of this attack came from the ability of the attacker to use the default password to log in, it is also recommended to prevent the use of old passwords such as the default to be used when updating a password. The use of multi factor authentication (MFA) is also recommended to further increase the security against brute force attacks. Any attacker that attempts a brute force attack will not be able to gain access to the system because of the additional authentication needed unless the attacker also gets hold of the user's device used for MFA.

## Learning Experience

This activity taught me about reading tcpdump logs and determine what network protocols are used. I also learned about TCP flag codes which increased my understanding of reading logs generated by tcpdump.

I also learned about the process of reading and analyzing tcpdump logs to determine how an attack is being conducted and be able to come up with a solution that could stop the attack and prevent it from happening in the future.

In this case, the attack is from a brute force attack wherin administrator credential is compromised due to bad password policy and the solution is to improve password policy by not repeating old passwords during a password update and to add authentication procedures like MFA to further improve authentication security.
