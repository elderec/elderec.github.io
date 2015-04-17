---
title: 'Windows &#8211; Reset Local Administrator Password Remotely'
author: Jonathan
layout: post
permalink: /2013/01/windows-reset-local-administrator-password-remotely/
categories:
  - Active Directory
  - Microsoft
  - Windows
tags:
  - Active Directory
  - Microsoft
  - Windows
---
The <a title="Pspasswd" href="http://technet.microsoft.com/en-us/sysinternals/bb897543.aspx" target="_blank">Pspasswd </a>utlitiy, which comes as part of the <a href="http://technet.microsoft.com/en-us/sysinternals/bb896649.aspx" title="Sysinternals PsTools" target="_blank">Sysinternals PsTools</a> kit, can be used to reset the local administrator password on machines locally or remotely. Obviously, this comes in handy when you&#8217;re not sure of the local administrator password on a domain joined machine.

Resetting the local administrator account on a single machine:

<pre class="brush: plain; title: ; notranslate" title="">pspasswd \\server.contoso.com administrator "passwordGoesHere"
</pre>

You can also specify a list of computer names in a text file to change the password on multiple machines. The example below assumes computerlist.txt contains a list of computer names, one per line:

<pre class="brush: plain; title: ; notranslate" title="">pspasswd \\@computerlist.txt administrator "passwordGoesHere"
</pre>