---
title: Baby interdimensional internet - Hack The Box
excerpt: "Baby is a easy machine, created by the Synack Red Team Track, to read the source code, manipulate post requestions and in inject commands, in flask application.Using content-type and python exec(command injection)"
categories:
  - writeups
tags:  
  - htb
  - burpsuite
  - flask
last_modified_at: 29-12-2021
---
![center-aligned-image](/assets/images/ctf/htb-writeup-baby-interdimensional-internet/synack.png){: .align-center}  

Baby is a easy machine, created by the Synack Red Team Track, to read the source code, manipulate post requestions and in inject commands, in flask application

## Web Page

Initialy we see a web page where has a number.

![](/assets/images/ctf/htb-writeup-baby-interdimensional-internet/1.png)

Search in source code, a directory /debug, in commented.

![](/assets/images/ctf/htb-writeup-baby-interdimensional-internet/2.png)

In this directory has a source code from a flask application. Has two accepted methods, POST and GET, with two parameters "accepted" and "accepted".

![](/assets/images/ctf/htb-writeup-baby-interdimensional-internet/3.png)

Now we can in burp, using repeater, send a post requestion to application. The application is vulnerable to a command injection, we can sen a exec command to python that will run the command and return the code.

![](/assets/images/ctf/htb-writeup-baby-interdimensional-internet/4.png)

## Observations

**HTTP HEADER**  
Have to add the content-type, to work the request.  
Font: https://www.geeksforgeeks.org/http-headers-keep-alive/
```

content-type: application/x-www-form-urlencoded

```
**Python exec Payload**  
Font: https://sethsec.blogspot.com/2016/11/exploiting-python-code-injection-in-web.html 
```
eval('__import__(\'os\').popen(\'ls\').read()')

```
