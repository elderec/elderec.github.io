---
title: 'Powershell &#8211; List Printer Names, Ports, and Drivers on Print Server'
author: Jonathan
layout: post
permalink: /2012/11/powershell-list-printer-names-ports-and-drivers-on-print-server/
dsq_thread_id:
  - 3684415416
categories:
  - Active Directory
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
Quick one-liner to pull printer names, drivers, and ports, from a print server.

<pre class="brush: powershell; title: ; notranslate" title="">Get-WMIObject -Class Win32_Printer -Computer server.contoso.com | Select Name,DriverName,PortName,Shared,ShareName | ft -auto
</pre>