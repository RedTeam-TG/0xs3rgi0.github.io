---
layout: default
title : x0SerGio -Lame Writeup
---
## **Lame HackTheBox Machine**

Lame is A Linux machine create by ch4p on Hackthebox ,so far lame is very practical machine and  it's  easy-to-exploit it 

![Screenshot 2021-12-11 02:33:57](https://user-images.githubusercontent.com/93042298/145665443-754867cb-96b3-4a7e-81bd-27b5ff8d25fb.png)

frist time  we start with a nmap scan 
 
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

the scan results showing that these ports are open:

-Port 21: running File Transfer Protocol (FTP) version 2.3.4 and This allows anonymous login 

-Port 22: running OpenSSH version 4.7p1.

-Ports 139 and 445: are running Samba v3.0.20-Debian

First thing we want to do is verify whether version 2.3.4 of vsftpd is vulnerable to any known exploits, so we run searchsploit:

`searchsploit vsftpd 2.3.4`

![Screenshot 2021-12-11 02:10:14](https://user-images.githubusercontent.com/93042298/145666560-c33140d5-0c3a-4940-b98c-3e6aa2909d60.png)

Since there is already a Metasploit module targeting this specific version of vsftpd let’s launch Metasploit and search for it:

![Screenshot 2021-12-11 03:26:43](https://user-images.githubusercontent.com/93042298/145667131-5017a041-7c86-4d4c-ada6-9cec9a9ffc85.png)

Now we going to launching the exploit setting for sure 

![Screenshot 2021-12-11 03:34:40](https://user-images.githubusercontent.com/93042298/145667320-6fa5b06a-1428-4d62-ade4-961a752ad463.png)
![Screenshot 2021-12-11 03:35:59](https://user-images.githubusercontent.com/93042298/145667328-d0fe64d9-25d1-4b6b-b825-1d28073d9462.png)

Exploit completed, but no session was created wow seem like our exploit fail .so let back to enumerate a little bit more the port 445 With Nmap NSE called smb-enum-shares 
run just this command :

`nmap --script smb-enum-shares.nse -p445 10.10.10.3 -Pn`

![Screenshot 2021-12-11 04:40:17](https://user-images.githubusercontent.com/93042298/145668865-64206f83-f519-4c40-9738-d80c1f19daf2.png)

Now, we will be using searchsploit in order to find any known exploits that abuse this specific version of Samba:

`searchsploit Samba 3.0.20`

![Screenshot 2021-12-11 05:01:36](https://user-images.githubusercontent.com/93042298/145669360-d58da88f-6592-47f6-a9a8-661277d1e673.png)

We can clearly see that there’s a Metasploit module of this exploit by the name “Username map script”, so let’s attempt to look for it in msfconsole:

![Screenshot 2021-12-11 05:05:21](https://user-images.githubusercontent.com/93042298/145669506-796baf79-6066-4915-a423-2cf86ef254b7.png)

We choose module number 13 and set options:




