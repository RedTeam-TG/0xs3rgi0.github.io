---
layout: default
title : 0xS3rGio - Comment masquer les informations de version d'un serveur Nginx
---

### Description

Dans cet article, nous verrons comment masquer les informations de version d'un serveur Nginx contre la technique du Banner Grabbing(capture de bannière)

### Le Banner Grabbing

Le`Banner Grabbing` est une technique utilisée pour obtenir des informations sur un système informatique sur un réseau et les services s'exécutant sur ses ports ouverts. Elle est souvent utilisée par les administrateurs système pour suivre les différents services au sein de leur organisation (source :`Wikipedia`).
C'est également l'une des techniques utiliser par des attaquants pour déterminer si un système est exploitable ou vulnerable.En cachant ces informations, vous leur rendez la vie plus difficile et votre serveur plus sécurisé.
 
 Maintenant, laissez-moi vous montrer comment faire cela avec un serveur Web Nginx
 
 Tout d'abord, lancez votre propre capture de bannière avec :

```
$ curl -I example.com
HTTP/1.1 301 Moved Permanently
Server: nginx *(REDACTED VERSION INFO)*
Date: Mon, 9 Nov 2021 23:43:54 GMT
Content-Type: text/html
Content-Length: 162
Connection: keep-alive
Location: https://example.com/
```
vous voyez que j'ai rédigé le numéro de la version au niveau des informations sur le serveur oui ,ce n'est donc qu'un exemple (je ne peux pas divulguer les informations de cette version du serveur nginx lol) j'espère que vous avez l'idée  

### Masquer les informations de version de Nginx

Pour masquer les informations, éditez le fichier `/etc/nginx/nginx.conf` :

```
sudo vim /etc/nginx/nginx.conf
```
`Le module server_tokens` activera ou désactivera la version nginx sur les pages d'erreur et dans le champ d'en-tête de réponse « Server »,pour plus d'info sur le module server_tokens référez vous a google.

l'étape suivante consiste donc à le désactiver en ajoutant « server_tokens off » ; dans la section `http` :

```
http {
	...
	server_tokens off;
	...
}
```
Maintenant Enregistrez et quittez l'éditeur de texte.

Alors passez à l'étape suivante, vérifiez que rien n'a été cassé avec :

```
sudo service nginx reload 	# Debian/Ubuntu
sudo systemctl restart nginx 	# RedHat/Centos
```
Enfin, confirmez les modifications apportées :

```
$ curl -I example.com
HTTP/1.1 301 Moved Permanently
Server: nginx 
Date: Mon, 9 Nov 2021 23:43:54 GMT
Content-Type: text/html
Content-Length: 162
Connection: keep-alive
Location: https://example.com/
```
Super, vous remarquez maintenant que la version serveur n'est plus présente ! Good job!
<br> <br>
[Back To Home](../index.md)
<br>



