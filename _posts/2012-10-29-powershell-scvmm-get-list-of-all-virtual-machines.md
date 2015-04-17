---
title: 'Powershell &#8211; SCVMM Get List of All Virtual Machines'
author: Jonathan
layout: post
permalink: /2012/10/powershell-scvmm-get-list-of-all-virtual-machines/
fb_social_plugin_settings_box_like:
  - default
fb_author_message:
  - 
fb_status_messages:
  - 'a:0:{}'
categories:
  - Hyper-V
  - Microsoft
  - Powershell
  - System Center Virtual Machine Manager
  - Virtualization
  - Windows
tags:
  - Hyper-V
  - Powershell
  - Scripting
  - System Center Virtual Machine Manager
  - Virtualization
  - Windows
---
Quick one-liner to generate a CSV of virtual machines, sorted by their hosts. Report will include Host Name, VM Name, VM Hostname, Status, Action on host stop, and Action on host start.

<pre class="brush: powershell; title: ; notranslate" title="">Get-VM -VMMServer scvmm.contoso.com | Sort-Object Hostname | Select-Object HostName, Name, ComputerName, Status, StopAction, StartAction | Export-Csv .\vm-list.csv -NoTypeInformation
</pre>