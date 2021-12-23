---
layout: default
title : x0SerGio -Blue Writeup
---
## **HacktheBox Blue**
 
 Blue est l'une des machines tres simple  de hackthebox creer par  ch4p . Elle démontre l'impact de l'exploit EternalBlue, qui a été utilisé pour compromettre les entreprises par le biais d'attaques de ransomware et de crypto-mining à grande échelle.
 
`Recon` :
 
 Nmap scan 
 
 ```nmap -sC -sV -oA scan <Target-IP>```
 
 ```
 ┌──(sergi㉿kali)-[~/HTB/Blue]
└─$ nmap -sV -sC -oA scan 10.10.10.40 
Starting Nmap 7.92 ( https://nmap.org ) at 2021-12-22 16:17 EST
Nmap scan report for 10.10.10.40
Host is up (0.12s latency).
Not shown: 991 closed tcp ports (conn-refused)
PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
49156/tcp open  msrpc        Microsoft Windows RPC
49157/tcp open  msrpc        Microsoft Windows RPC
Service Info: Host: HARIS-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2021-12-22T21:18:29
|_  start_date: 2021-12-22T19:30:22
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: haris-PC
|   NetBIOS computer name: HARIS-PC\x00                                                                
|   Workgroup: WORKGROUP\x00                                                                           
|_  System time: 2021-12-22T21:18:31+00:00                                                             
|_clock-skew: mean: 3s, deviation: 2s, median: 1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 85.92 seconds
 ```   
 
 Le resultat du scan Nous revelent Des ports ouverts sur notre machine evidemment des ports en RPC (135), NetBios (139), SMB (445) et plein d'autres ports. Nous allons avancez avec le SMB en executant un script nmap pour vérifier si ce dernier est vulnerable:

`nmap --script vuln -p 445 10.10.10.40`

![Screenshot_2021-12-23_06-50-28](https://user-images.githubusercontent.com/93042298/147236657-37d60c30-84c6-42db-a849-187b2a52bf32.png)

 Notre box est vulnerable et pour info la vulnerabilite est liee au  RCE dans les serveurs Microsoft SMBv1 (ms17-010).Ce qui reference directment a l' EternalBlue qui avait permis au hackers d'exécuter à distance du code arbitraire pour accéder à des réseaux suite a cette  vulnérabilité dans le protocole Windows OS SMB.

Nous allons donc utilisez Searchsploit pour vérifier notre exploit dans Exploit Database.

![Screenshot_2021-12-23_07-30-04](https://user-images.githubusercontent.com/93042298/147240856-076e5064-fcb2-4353-a46f-fce89bb169d8.png)

 
 
