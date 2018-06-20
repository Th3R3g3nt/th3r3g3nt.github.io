---
layout: post
title: "Dictionary-based Data Transfer (DDT) on GitHub"
date: 2018-06-19
excerpt_separator: <!--more-->
---

I've put my **DDT** client-server script on GitHub, so that everyone can benefit from it if someone choose to use it. DDT stands for **Dictionary-based Data Transfer**, a form of substitution based data exfiltration technique and a proof-of-concept tool. 

TL;DR
* Uses nslookup to exfiltrate (as of 6/19/2018, it still uses scapy, but it can be changed to nslookup - on the todo list)
* Default dictionary items are short, common, and human readable subdomain
* Data is obfuscated via in-place substitution, and without the directory, the data is non-descriptive
* Client-server communicates via provided infrastructure, no direct connection to the SOA is required
* Courtesy dictionary is included (dictionary generator is in the works)
* You still need your SOA & domain to be categorized as non-malicious

<!--more-->

The main goal is to use the victims’ network recursive DNS resolver to move a file from the source to the destination without raising suspicion based off the type of query-response traffic that we generate. The key is to use dictionary / substitution based subdomain lookups, any this could even work with the good-old nslookup on the compromised OS (Win or Linux or etc).  

While this is no silver bullet, depending on the quality of your dictionary, it may suit the needs where stealth is more important than speed or instant-volume of exfiltration.  

Get it at [https://github.com/Th3R3g3nt/DDT][ddt]


  
Example screenshot is below as:  

![Python Client Screenshot]({{ "/files/DDT-Client.jpg" | absolute_url }})  

![Python Server Screenshot]({{ "/files/DDT-Client.jpg" | absolute_url }})  


[ddt]:https://github.com/Th3R3g3nt/DDT

