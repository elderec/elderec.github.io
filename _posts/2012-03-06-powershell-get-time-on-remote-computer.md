---
title: 'Powershell &#8211; Get Time On Remote Computer'
author: Jonathan
layout: post
permalink: /2012/03/powershell-get-time-on-remote-computer/
categories:
  - Microsoft
  - Powershell
  - Scripting
  - Windows
tags:
  - Microsoft
  - Powershell
  - Scripting
  - Windows
---
Short but useful snippet to get the time on a remote computer using Powershell & WMI.

<pre class="brush: powershell; title: ; notranslate" title="">$rt = Get-WmiObject -Class Win32_OperatingSystem -ComputerName computer.consoto.com
$rt.ConvertToDateTime($rt.LocalDateTime)
</pre>