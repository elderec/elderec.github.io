---
title: 'Powershell &#8211; Get Network Adapter Connection Speed'
author: Jonathan
layout: post
permalink: /2012/03/powershell-get-network-adapter-connection-speed/
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
One-Liner to get network adapter speed in Powershell.

<pre class="brush: powershell; title: ; notranslate" title="">Get-WmiObject -ComputerName computer.contoso.com -Class Win32_NetworkAdapter | Where-Object { $_.Speed -ne $null -and $_.MACAddress -ne $null } | Format-Table -Property NetConnectionID,@{Label='Speed(MB)'; Expression = {$_.Speed/1MB}}
</pre>