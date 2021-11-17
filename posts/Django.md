---
layout: default
title : x0SerGio -Django Writeup
---

## **PwnTillDawn Online Battlefield**

PwnTillDawn Online Battlefield is a penetration testing lab created by wizlynx group where participants can test their offensive security skills in a safe and legal environment, but also having fun! The goal is simple, break into as many machines as possible using a succession of weaknesses and vulnerabilities and collect flags to prove the successful exploitation. Each target machine that can be compromised contains at least one “FLAG” (most of the times a file and typically located in the user’s Desktop, or the user’s root directory), which you must retrieve, and submit in the application. The flag is in the majority of the cases in a SHA1 format but not always

![Django](https://user-images.githubusercontent.com/93042298/142042252-a174da6f-9f29-42f1-becb-3c6f487896ee.png)

 frist time  we start with a nmap scan 
 
 ```nmap -sC -sV -oA scan <Target-IP>```

```
 Starting Nmap 7.91 ( https://nmap.org ) at 2021-11-16 19:15 EST
Stats: 0:00:23 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 76.77% done; ETC: 19:16 (0:00:07 remaining)
Nmap scan report for 10.150.150.212
Host is up (0.17s latency).
Not shown: 928 closed ports, 58 filtered ports
PORT      STATE SERVICE      VERSION
21/tcp    open  ftp
| fingerprint-strings: 
|   GenericLines: 
|     220-Wellcome to Home Ftp Server!
|     Server ready.
|     command not understood.
|     command not understood.
|   Help: 
|     220-Wellcome to Home Ftp Server!
|     Server ready.
|     'HELP': command not understood.
|   NULL, SMBProgNeg: 
|     220-Wellcome to Home Ftp Server!
|     Server ready.
|   SSLSessionReq: 
|     220-Wellcome to Home Ftp Server!
|     Server ready.
|_    command not understood.
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drw-rw-rw-   1 ftp      ftp            0 Mar 26  2019 . [NSE: writeable]
| drw-rw-rw-   1 ftp      ftp            0 Mar 26  2019 .. [NSE: writeable]
| drw-rw-rw-   1 ftp      ftp            0 Mar 13  2019 FLAG [NSE: writeable]
| -rw-rw-rw-   1 ftp      ftp        34419 Mar 26  2019 xampp-control.log [NSE: writeable]
|_-rw-rw-rw-   1 ftp      ftp          881 Nov 13  2018 zen.txt [NSE: writeable]
|_ftp-bounce: bounce working!
| ftp-syst: 
|_  SYST: Internet Component Suite
80/tcp    open  http         Apache httpd 2.4.34 ((Win32) OpenSSL/1.0.2o PHP/5.6.38)
|_http-server-header: Apache/2.4.34 (Win32) OpenSSL/1.0.2o PHP/5.6.38
| http-title: Welcome to XAMPP
|_Requested resource was http://10.150.150.212/dashboard/
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
443/tcp   open  ssl/http     Apache httpd 2.4.34 ((Win32) OpenSSL/1.0.2o PHP/5.6.38)
|_http-server-header: Apache/2.4.34 (Win32) OpenSSL/1.0.2o PHP/5.6.38
| http-title: Welcome to XAMPP
|_Requested resource was https://10.150.150.212/dashboard/
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
445/tcp   open  microsoft-ds Windows 7 Home Basic 7601 Service Pack 1 microsoft-ds (workgroup: PWNTILLDAWN)
3306/tcp  open  mysql        MariaDB (unauthorized)
8089/tcp  open  ssl/http     Splunkd httpd
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: splunkd
| ssl-cert: Subject: commonName=SplunkServerDefaultCert/organizationName=SplunkUser
| Not valid before: 2019-10-29T14:31:26
|_Not valid after:  2022-10-28T14:31:26
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
49157/tcp open  msrpc        Microsoft Windows RPC
49158/tcp open  msrpc        Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port21-TCP:V=7.91%I=7%D=11/16%Time=61944A09%P=x86_64-pc-linux-gnu%r(NUL
SF:L,35,"220-Wellcome\x20to\x20Home\x20Ftp\x20Server!\r\n220\x20Server\x20
SF:ready\.\r\n")%r(GenericLines,79,"220-Wellcome\x20to\x20Home\x20Ftp\x20S
SF:erver!\r\n220\x20Server\x20ready\.\r\n500\x20'\r':\x20command\x20not\x2
SF:0understood\.\r\n500\x20'\r':\x20command\x20not\x20understood\.\r\n")%r
SF:(Help,5A,"220-Wellcome\x20to\x20Home\x20Ftp\x20Server!\r\n220\x20Server
SF:\x20ready\.\r\n500\x20'HELP':\x20command\x20not\x20understood\.\r\n")%r
SF:(SSLSessionReq,89,"220-Wellcome\x20to\x20Home\x20Ftp\x20Server!\r\n220\
SF:x20Server\x20ready\.\r\n500\x20'\x16\x03\0\0S\x01\0\0O\x03\0\?G\xd7\xf7
SF:\xba,\xee\xea\xb2`~\xf3\0\xfd\x82{\xb9\xd5\x96\xc8w\x9b\xe6\xc4\xdb<=\x
SF:dbo\xef\x10n\0\0\(\0\x16\0\x13\0':\x20command\x20not\x20understood\.\r\
SF:n")%r(SMBProgNeg,35,"220-Wellcome\x20to\x20Home\x20Ftp\x20Server!\r\n22
SF:0\x20Server\x20ready\.\r\n");
Service Info: Hosts: Wellcome, DJANGO; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -1h09m19s, deviation: 2s, median: -1h09m21s
| smb-os-discovery: 
|   OS: Windows 7 Home Basic 7601 Service Pack 1 (Windows 7 Home Basic 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1
|   Computer name: Django
|   NetBIOS computer name: DJANGO\x00
|   Workgroup: PWNTILLDAWN\x00
|_  System time: 2021-11-16T23:08:58+00:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-11-16T23:08:59
|_  start_date: 2020-04-02T14:41:43

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 163.30 seconds

```
The Scan Results reveals that we have open ports on our box obviously FTP, HTTP, SMB and other ports too but I decided to focus on these ports which leads us to Ports Enumeration.

So i decided to check to enumerate the SMB first even if we already have anonymous access to the FTP

![Screenshot 2021-11-17 03:13:30](https://user-images.githubusercontent.com/93042298/142147098-ae0f0ae5-77f7-4a4a-af21-ca118e216715.png)

Nothing Much with the SMB seem like there is no shares. Now let check the port 80 

![Screenshot 2021-11-17 08:49:01](https://user-images.githubusercontent.com/93042298/142195625-80a6f413-34a6-4c04-b224-870c2ccc7455.png)

Nice dashboard with some info on the web server.so far just by doing some polls on the page i have then access the `phpinfo.php` page. nice stuff ! now let's try to check the `phpmyadmin` page

![Screenshot 2021-11-17 11:07:35](https://user-images.githubusercontent.com/93042298/142216188-1d980635-3099-4efe-aca0-1e042a948a4e.png)

Trying default credentials such as (admin , admin),(admin , password) but no luck nothing much lol. so let get a look on the FTP 

![Screenshot 2021-11-17 11:26:08](https://user-images.githubusercontent.com/93042298/142219420-284a6045-948e-4954-b041-e625f0656d0c.png)

Boommm! haha we have our first flag which is `FLAG19.txt` cool! Trying to go ahead with `FTP Directory Traversal` using `ls ../../../` and `cd /..` I was able to see all the directories

![Screenshot 2021-11-17 11:49:43](https://user-images.githubusercontent.com/93042298/142226040-dbb277bf-9251-4251-9467-4631bd6ca663.png)
![Screenshot 2021-11-17 11:51:54](https://user-images.githubusercontent.com/93042298/142226120-30b7f98e-f7d0-43bb-a7d0-4424edf53a13.png)









