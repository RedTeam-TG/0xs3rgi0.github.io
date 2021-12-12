---
layout: default
title : x0SerGio -Django Writeup
---

## **PwnTillDawn Online Battlefield**

PwnTillDawn Online Battlefield est un lab de tests d'intrusion créé par le groupe wizlynx où les participants peuvent tester leurs compétences en sécurité offensive dans un environnement sûr et légal, mais aussi en s'amusant ! Le but est simple, s'introduire dans autant de machines que possible en utilisant une succession de faiblesses et de vulnérabilités et collecter des flags pour prouver la réussite de l'exploitation. Chaque machine cible qui peut être compromise contient au moins un « FLAG » (la plupart du temps un fichier et généralement situé sur le bureau de l'utilisateur ou dans le répertoire racine de l'utilisateur), que vous devez récupérer et soumettre dans l'application. Le flag est dans la majorité des cas au format SHA1 mais pas toujours

![Django](https://user-images.githubusercontent.com/93042298/142042252-a174da6f-9f29-42f1-becb-3c6f487896ee.png)

 nous commençons par un scan nmap comme dab!
 
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
Les résultats du Scan révèlent que nous avons des ports ouverts sur notre machine , évidemment  FTP, HTTP, SMB et d'autres ports aussi.J'ai donc décidé d énumérer le SMB en premier même si nous avons déjà un accès anonyme au FTP 

![Screenshot 2021-11-17 03:13:30](https://user-images.githubusercontent.com/93042298/142147098-ae0f0ae5-77f7-4a4a-af21-ca118e216715.png)

aucun partage au niveau du SMB bahh. Maintenant,vérifions le port 80

![Screenshot 2021-11-17 08:49:01](https://user-images.githubusercontent.com/93042298/142195625-80a6f413-34a6-4c04-b224-870c2ccc7455.png)

 joli dashboard j'ai puis accéder à la page `phpinfo.php` mais nous avons juste quelques informations sur le serveur Web .Maintenant essayons de vérifier la page  
![Screenshot 2021-11-17 11:07:35](https://user-images.githubusercontent.com/93042298/142216188-1d980635-3099-4efe-aca0-1e042a948a4e.png)

 J'ai essayé de me loger avec les informations d'identification par défaut mais pas de chance.un retour sur le FTP Serais cool 

![Screenshot 2021-11-17 11:26:08](https://user-images.githubusercontent.com/93042298/142219420-284a6045-948e-4954-b041-e625f0656d0c.png)

Boummm ! haha nous avons notre premier flag qui est `FLAG19.txt` cool ! En essayant d'aller plus loin avec `FTP Directory Traversal`(`ls ../../../` et `cd /..`) j'ai pu voir tous les répertoires  

![Screenshot 2021-11-17 11:49:43](https://user-images.githubusercontent.com/93042298/142226040-dbb277bf-9251-4251-9467-4631bd6ca663.png)
![Screenshot 2021-11-17 11:51:54](https://user-images.githubusercontent.com/93042298/142226120-30b7f98e-f7d0-43bb-a7d0-4424edf53a13.png)

MDR! en vérifiant le répertoire `xampp` et nous avons notre deuxième flag `FLAG20.txt` et également un fichier `passwords.txt` contenant les détails d'identification pour Phpmyadmin

![Screenshot 2021-11-17 12:52:07](https://user-images.githubusercontent.com/93042298/142234854-42af1922-9308-4c3b-b7e7-a59d8734ed30.png)

Maintenant, connectez-nous à Phpmyadmin avec les informations d'identification :

`User: root`
`Password:thebarrierbetween`

![Screenshot 2021-11-17 13:15:00](https://user-images.githubusercontent.com/93042298/142239047-c733c7c0-58be-47a9-9d73-6f78dc66661a.png)

Notre FLAG 18 obtenu .Trouvons maintemant  le dernier FLAG juste en revenant sur le serveur FTP

![Screenshot 2021-11-17 13:38:09](https://user-images.githubusercontent.com/93042298/142243172-5f47420a-c8a1-4329-aa3b-8cde7824bb93.png)

Good job ! dernier flag obtenu, vous pouvez essayer aussi un autre moyen pour otbenir le dernier flag via un shell reverse au terminal.xD

Have Fun !!!

Greeting From [Sergio](https://twitter.com/x0sergi)
<br> <br>
[Back To Home](../index.md)
<br>
















