---
layout: post
title: "pChart 2.1.3 and earlier Directory Traversal, Reflected XSS"
date: 2014-01-24
excerpt_separator: <!--more-->
---

So I stumbled upon a webapp called pChart as part of one of my pentest. Turned out that there are some injection and XSS problems in the Example folder, that is present by default. I contacted the author, worked out the details of notification and publication; then here we are.

TL;DR
* Directory Traversal
* Cross-Site Scripting (XSS)
* Requires hxxp://victimhost.com/examples/ to be accessible
* http://www.exploit-db.com/exploits/31173/
* OSVDB-ID: 102595
<!--more-->

**Timeline**

2014 January 16 - Vulnerability confirmed, vendor contacted  
2014 January 17 - Vendor replied, responsible disclosure was orchestrated  
2014 January 24 - Vendor was inquired about progress, vendor replied
and noted that the official patch is released.  

**Exploit-DB submission**

\# Exploit Title: pChart 2.1.3 Directory Traversal and Reflected XSS  
\# Date: 2014-01-24  
\# Exploit Author: Balazs Makany  
\# Vendor Homepage: www.pchart.net  
\# Software Link: www.pchart.net/download  
\# Google Dork: intitle:"pChart 2.x - examples" intext:"2.1.3"  
\# Version: 2.1.3  
\# Tested on: N/A (Web Application. Tested on FreeBSD and Apache)  
\# CVE : N/A  

**Summary**  
PHP library pChart 2.1.3 (and possibly previous versions) by default contains an examples folder, where the application is vulnerable to Directory Traversal and Cross-Site Scripting (XSS). It is plausible that custom built production code contains similar problems if the usage of the library was copied from the examples. The exploit author engaged the vendor before publicly disclosing the vulnerability and consequently the vendor released an official fix before the vulnerability was published.

**[1] Directory Traversal**  
"hxxp://localhost/examples/index.php?Action=View&amp;Script=%2f..%2f..%2fetc/passwd"  
The traversal is executed with the web server's privilege and leads to sensitive file disclosure (passwd, siteconf.inc.php or similar), access to source codes, hardcoded passwords or other high impact consequences, depending on the web server's configuration. This problem may exists in the production code if the example code was copied into the production environment.

Directory Traversal remediation  
1. Update to the latest version of the software  
2. Remove public access to the examples folder where applicable  
3. Use a Web Application Firewall or similar technology to filter malicious input attempts  


**[2] Cross-Site Scripting (XSS)**  
"hxxp://localhost/examples/sandbox/script/session.php?&lt;script&gt;alert('XSS')&lt;/script&gt;"  
This file uses multiple variables throughout the session, and most of them are vulnerable to XSS attacks. Certain parameters are persistent throughout the session and therefore persists until the user session is active. The parameters are unfiltered.

Cross-Site Scripting remediation  
1. Update to the latest version of the software  
2. Remove public access to the examples folder where applicable  
3. Use a Web Application Firewall or similar technology to filter malicious input attempts  
