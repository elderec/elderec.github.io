---
title: 'Powershell &#8211; Install SNMP Services Remotely on Windows Server 2008R2'
author: Jonathan
layout: post
permalink: /2013/09/powershell-install-snmp-services-remotely-on-windows-server-2008r2/
dsq_thread_id:
  - 3684415569
categories:
  - Active Directory
  - Microsoft
  - Powershell
  - Scripting
  - Windows
tags:
  - Active Directory
  - Microsoft
  - Powershell
  - Scripting
  - Windows
---
The script below assumes you have an active directory group with all the servers as members. 

<pre class="brush: powershell; title: ; notranslate" title=""># import the powershell active direcory module
Import-Module ActiveDirectory

# get the group members
$servers = Get-ADGroupMember -Identity GroupWithServersInIt

# install SNMP on the servers
foreach ($server in $servers) {
	invoke-command -computername $server.name -ScriptBlock {import-module ServerManager; Add-WindowsFeature SNMP-Services}
}
</pre>