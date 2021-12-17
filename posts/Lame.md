---
layout: default
title : x0SerGio -Lame Writeup
---
## **Lame HackTheBox Machine**

Lame est une machine Linux créée par ch4p sur Hackthebox, jusqu'à présent lame est une machine très pratique et il est facile de l'exploiter

![Screenshot 2021-12-11 02:33:57](https://user-images.githubusercontent.com/93042298/145665443-754867cb-96b3-4a7e-81bd-27b5ff8d25fb.png)

 nous commençons par un scan nmap
 
 ```nmap -sC -sV -oA scan <Target-IP> -Pn```

```
┌──(haveadream㉿kali)-[~]
└─$ nmap  -sV -sC -oA scan 10.10.10.3 -Pn
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-12-11 01:52 EST
Nmap scan report for 10.10.10.3
Host is up (0.16s latency).
Not shown: 996 filtered ports
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.14.4
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 33m09s, deviation: 3h32m09s, median: -1h56m51s
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name: 
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2021-12-10T23:58:20-05:00
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smb2-time: Protocol negotiation failed (SMB2)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 193.04 seconds

```

les résultats du scan montre que ces ports sont ouverts :

-Port 21 : exécutant le protocole de transfert de fichiers (FTP) version 2.3.4 et cela permet une connexion anonyme

-Port 22 : exécutant OpenSSH version 4.7p1.

-Ports 139 et 445 : exécutent Samba v3.0.20-Debian

vérifions si la version 2.3.4 de vsftpd est vulnérable à des exploits connus,lançons donc searchsploit :

`searchsploit vsftpd 2.3.4`

![Screenshot 2021-12-11 02:10:14](https://user-images.githubusercontent.com/93042298/145666560-c33140d5-0c3a-4940-b98c-3e6aa2909d60.png)

Sisi ,Nous avons deja un module Metasploit qui cible cette version spécifique de vsftpd,lançons Metasploit et recherchons-le :

![Screenshot 2021-12-11 03:26:43](https://user-images.githubusercontent.com/93042298/145667131-5017a041-7c86-4d4c-ada6-9cec9a9ffc85.png)

Maintenant, nous allons paramètrer l'exploit

![Screenshot 2021-12-11 03:34:40](https://user-images.githubusercontent.com/93042298/145667320-6fa5b06a-1428-4d62-ade4-961a752ad463.png)
![Screenshot 2021-12-11 03:35:59](https://user-images.githubusercontent.com/93042298/145667328-d0fe64d9-25d1-4b6b-b825-1d28073d9462.png)

Il semble que notre exploit a échoue.Nous allons plus énumérer le port 445 Avec Nmap NSE appelé smb-enum-shares en faisant:

`nmap --script smb-enum-shares.nse -p445 10.10.10.3 -Pn`

![Screenshot 2021-12-11 04:40:17](https://user-images.githubusercontent.com/93042298/145668865-64206f83-f519-4c40-9738-d80c1f19daf2.png)

Nous Allons utiliser searchsploit afin de trouver tous les exploits connus qui abusent de la version spécifique de notre Samba

`searchsploit Samba 3.0.20`

![Screenshot 2021-12-11 05:01:36](https://user-images.githubusercontent.com/93042298/145669360-d58da88f-6592-47f6-a9a8-661277d1e673.png)

Nous pouvons clairement voir qu'il existe un module Metasploit de cet exploit sous le nom de « Username map script », alors essayons de le rechercher dans msfconsole

![Screenshot 2021-12-11 05:05:21](https://user-images.githubusercontent.com/93042298/145669506-796baf79-6066-4915-a423-2cf86ef254b7.png)

choisissons le module numéro 13 et définissons les options

![Screenshot 2021-12-11 05:20:01](https://user-images.githubusercontent.com/93042298/145669847-3ce9fa2f-3b6a-4c3f-a0d4-23be7a38aaa5.png)

Apres L' exécutions de l'exploit, nous obtenons un shell,notre session est bien etablie

![Screenshot 2021-12-11 05:29:19](https://user-images.githubusercontent.com/93042298/145670158-061e1ef7-1b80-43c0-ac88-6a4d1c6a8d46.png)

![Screenshot 2021-12-11 05:30:36](https://user-images.githubusercontent.com/93042298/145670450-f67c682a-721f-453f-bcce-fd8e657cf1c8.png)

Maintemant trouvons les flags, naviguons d'abord vers `/home`. il y 2 répertoires User et makis , en verifiant ces derniers nous avons trouvé le flag de l'utilisateur sous le répertoire makis cool!

![Screenshot 2021-12-11 05:57:46](https://user-images.githubusercontent.com/93042298/145670929-747a57a9-08ab-4e8e-8a43-0a449872dd46.png)

Nous allons maintenant naviguer vers `/root` et obtenir le flag final

![Screenshot 2021-12-11 05:58:43](https://user-images.githubusercontent.com/93042298/145671044-5a39f94c-cc39-419b-a477-861b34da4f66.png)


Have Fun !!!

CC [Sergio](https://twitter.com/x0sergi)
<br> <br>
[Back To Home](../index.md)
<br>



