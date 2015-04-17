---
title: 'Powershell &#8211; Get Service Tags Remotely'
author: Jonathan
layout: post
permalink: /2012/11/powershell-get-service-tags-remotely/
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
Quick Powershell snippet to retrieve service tags from remote machines. Create a text file with one FQDN or IP per line. Adjust the Get-Content line in the snippet below and run.

<pre class="brush: powershell; title: ; notranslate" title="">$list = Get-Content -Path "c:\some\file.txt"

ForEach ($machine in $list) {
	$colItems = Get-WmiObject Win32_BIOS -Namespace “root\CIMV2" -Computername $machine
	ForEach($item in $col) { 
		Write-Host $machine "=" $item.SerialNumber
	}
}
</pre>