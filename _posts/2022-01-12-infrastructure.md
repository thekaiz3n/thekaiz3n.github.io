---
title: Infrastructure recon
excerpt: "This is a guide to scanning the host."
last_modified_at: 12-02-2022
categories:
- cheatsheet
tags:  
- pentest
- methodologies
- recon
- scanning
- infrastructure
---

# Scanning  


## Search Whois
whois is a tool to find who control the domain and where is register. Useful find the ip range.
* [IANA whois](iana.org/whois): Search in web.  

```bash
whois google.com
whois 172.16.0.100 #Return the ip range
```
## RDAP
Substitute of whois, your return is in json.
* [Client RDAP](https://client.rdap.org/): Web client RDAP.

## host
Command to find the ip of a domain.
```bash
host google.com #Return the ip
```  
## BGP/ASN
BGP Border gateway protocol is useful to find the ips ranges and asn.
* [bgpview](https://bgpview.io/): Web client to search bgp by IP.

ASN Autonomous System Number consists of blocks of IP addresses that are administered by a single organization, but cold has more.
```bash
amass intel -asn 28000 #Return the domains of the specific asn

amass intel -org paypal -max-dns-queries 2500 | awk -F, '{print $1}' ORS=',' | sed 's/,$//' | xargs -P3 -I@ -d ',' amass intel -asn @ -max-dns-queries 2500'' #Search all asn from paypal and search all domains of every asn

echo 'dod' | metabigor net --org -v | awk '{print $3}' | sed 's/[[0-9]]\+\.//g' | xargs -I@ sh -c 'prips @ | hakrevdns | anew' #Using metabigor OSINT tool
```  

## Shodan
Like a google to hackers.
Exemplos:
- Operators:
    - hostname: specific url
    - os: operational system
    - port: port
    - ip
    - net: by network
    - country
    - city
    - geo: geocalization
    - org: organization
    - " " : specific word
- Pesquisas:
    - webcam country:us
    - hostame:bgoogle.com
    - ip:37.59.174.225
    - net:37.59.174.224/28
    - port:10001 tanque country:us
    - port:3306 os:Windows country:us
    - port:445 country:br accountability
- API: Has a API to command line and integrations, required account.  

## Censys
Work like shodan.
Exemplos:
- Has operators and filters
- ip:[172.10.100.50 to 172.10.100.239]
- 172.10.100.224/28
- location.country_code:US AND metadata.os:UBUNTU AND 80.https.get.title:"index of"
- location.country_code:US AND metadata.os:UBUNTU AND 80.https.get.title:"Adminnistration"
- location.country_code:US AND metadata.os:UBUNTU AND ports:3306  

## CRT.SH
Log register of certifications, useful to search domains and subdomains:
```bash
curl -s "https://crt.sh/?q=%25.att.com&output=json" | jq -r '.[].name_value' | sed 's/\*\.//g' | httpx -title -silent | anew subdomains
```
## DNS
- Register types:
    - SOA: Main register
    - A: IPv4
    - AAAa: IPv6
    - NS: Name servers
    - CNAME: Canonical name(Alias/apelido)
    - MX: Mail exchange(email)
    - PTR: Poiter (A reverse / Translate IP to name)
    - HINFO: Host informations
    - TXT: Return information in text
- Reverse DNS
    - Ip to name
```bash
for ip in $(seq 1 239); do host -t ptr 172.20.1.$ip;done # Translate
```  

## DNS zone transfer
Has always at least 2 DNS server that contains the registers, and they have to stay synchronize.
* host -t ns google.com -> List the dns servers
* host -l -a google.com ns2.google.com -> Try transfer one to another  

## Passive recon
- [virustotal.com](http://virustotal.com)
- [dnsdumpster.com](http://dnsdumpster.com)
- [securitytrails.com](http://securitytrails.com)

## Trace route
Useful to identify the route, discover the OS based in ttl(TCP protocol) and border firewalls.
```bash
traceroute gooogle.com
```
Parameters:
- -w 3 : Waiting time
- -m 1 : TTL number
- -f 15 : Start of route
- -I : Change of UDP to ICMP.
- -T : USE TCP protocol.
- -p 53: Port.

## Active hosts/Ping Sweep
Useful to check the hosts that are alive.
```bash
fping -a -g 172.16.1.0/24

nmap -sn 172.100.100-239 -oG

nmap -v -sn 172.16.1.0/24 #Hosts of a range
```
## TCP ports
```bash
nmap -v -sS -p- -Pn 192.168.0.11 # -p- all ports, 
```
Types:
* TCP connect: Complete the three-way hanshake, consume more traffic and easily detectable.
    * Nmap: -T
* Half open/syn scan: Don't complete the twhs, after the syn/ack, is send RST to cancel the connection. More furtive.
    * Nmap: -S  
    
Nmap parameters:
```bash
-U : UDP protocol.
-v :Verbose
-p80,8080,22 :Select ports to scan
-g 80 :Origin port
-D RND:20 :Create randon IPS
-D 10.2.12.2.34,192.168.0.23 :Set the IP origin
-A :Agressive use the nse scripts and more.
-T1 to -T5 :Speed choice.
-V :Run scripts.
--top-ports:100 : Check the top 100 ports.
-oG output.file :Output and grepable
-iL : List of hosts
-p http* : Port that use http
-O : OS detect
```
## UDP ports
Apresent more diifult responses and not so reliable.
- Open port : No response
- Close port : Port unreachable
- Port with firewall drop : No response
- Port with firewall reject : Port unreachable
```bash
nmap -v -sUV 172.16.1.4
```  

## OS fingerprint

- TTL : Ping or another.
    - Linux: 64
    - WIndows: 128
    - FreeBSD: 65
    - Solaris: 255
    - Cisco: 254  

## Subdomains
Example with a few tools.
```bash
chaos -d $1 -o chaos1 -silent ; assetfinder -subs-only $1 >> assetfinder1 ; subfinder -d $1 -o subfinder1 -silent ; cat assetfinder1 subfinder1 chaos1 >> hosts ; cat hosts | anew clearDOMAIN ; httpx -l hosts -silent -threads 100 | anew http200 ; rm -rf chaos1 assetfinder1 subfinder1
```
Tools do find subdomains:

* [chaos](https://chaos.projectdiscovery.io/#/)
* [securitytrails](https://securitytrails.com/)
* [assetfinder](https://github.com/tomnomnom/assetfinder)
* [subfinder](https://github.com/projectdiscovery/subfinder)
* [crt.sh](https://crt.sh/)
* [github-domains.py](https://github.com/gwen001/github-search)


## SPF - Sender policy framework
Identify the servers allowed to send email with your domain.
- host - t txt google,com -> Find the active rule
    - Sem registro: It's vulnerable.
    - v=spfl include:google.com ?all -> vulnerable (neutral)
    - v=spfl include:google.com ~all -> vulnerable (identified as suspect)
    - v=spfl include:google.com -all -> Safe configuration.