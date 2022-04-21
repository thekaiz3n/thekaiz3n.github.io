---
title: Breaking grad - Hack The Box
excerpt: "Breaking grad is a medium/hard machine thats need knowledgment of prototype pollution, where we can abuse of a js funtion that has a bad validation of objetc parameters."
categories:
  - writeups
tags:  
  - htb
  - prototype_polluition
  - POST
  - js
last_modified_at: 30-12-2021
---
![center-aligned-image](/assets/images/ctf/hackthebox.webp){: .align-center}  

Breaking grad is a medium/hard machine thats need knowledgment of prototype pollution, where we can abuse of a js funtion that has a bad validation of objetc parameters.

## Web Page

Initialy we see a web page where has a choose of a person e return, and say if has pass or not. But don't seem to have anything. 

![](/assets/images/ctf/htb-writeup-breaking-grad/1.png)

So we can check the files, seems to be a js, using express. Checking the routes, has the /api/calculate that check and return that message to screen. And have a **/debug/:action**

![](/assets/images/ctf/htb-writeup-breaking-grad/2.png)

Debug seems to be interesting, let's check the DebugHelper.js. The function receive a command that execute and returning something.

![](/assets/images/ctf/htb-writeup-breaking-grad/3.png)

Let's check the fork function.

fork function: https://nodejs.org/dist/latest-v6.x/docs/api/child_process.html#child_process_child_process_fork_modulepath_args_options

This function receive a object.

```
execPath <string> Executable used to create the child process.
execArgv <Array> List of string arguments passed to the executable. Default: process.execArgv

```

The function receive this parameters that can execute in the server. But the **__proto__** is filtered, in objectHelper.js.js, but  the **constructor.prototype** not, and do the same thing.

fucntion __proto__ : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto

![](/assets/images/ctf/htb-writeup-breaking-grad/5.png)

Send a object, with the parameters accepted by fork, will execute the command.

That will be write in /debug/version.
![](/assets/images/ctf/htb-writeup-breaking-grad/6.png)

Heavily inspired by https://y3a.github.io/2021/06/15/htb-breaking-grad/