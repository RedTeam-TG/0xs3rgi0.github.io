---
layout: default
title : 0xS3rGio - How to Hide An Nginx server Version Information
---
### Description

in this article we will see how to hide of an Nginx server version information against banner grabbing technique

### Banner Grabbing

`Banner Grabbing` is a technique used to gain information about a computer system on a network and the services running on its open ports.It is often used by system admins to keep track of the different services across their organization(source:`Wikipedia`)
It's also one of the very techniques that attackers can use to determine if a system can be exploited, however by hiding this information you make life more difficult for them and your server more secure.
 
 Now let me show you how to do this with an Nginx web server
 
 Frist at all,run your own banner grab with:

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
you see that i have redacted the version number in server information side so this is just an example(can't leak the info this nginx server version lol) i hope you get the idea

### Hide Nginx Version Information

To hide the info , edit the `/etc/nginx/nginx.conf` file:

```
sudo vim /etc/nginx/nginx.conf
```
`The server_tokens` module will either enable or disable the nginx version on error pages and in the “Server” response header field.

so the next step is to turn it off by adding `server_tokens off`; under the `http` section:

```
http {
	...
	server_tokens off;
	...
}
```
now Save and exit the text editor.

So jump on the next step, check that nothinng was broken with:

```
sudo service nginx reload 	# Debian/Ubuntu
sudo systemctl restart nginx 	# RedHat/Centos
```
Finally, confirm the changes worked:

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
Great,you notice now that The server version is no longer present! GooD JoB!

Hope you have learned and enjoyed this article

<br> <br>
[Back To Home](../index.md)
<br>



