---
title: 'Powershell &#8211; Find Serial Numbers on MININT Computers in Active Directory'
author: Jonathan
layout: post
permalink: /2012/11/powershell-find-serial-numbers-on-minint-computers-in-active-directory/
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
Find MININT computers and retrieve their service tag / serial numbers.

<pre class="brush: powershell; title: ; notranslate" title=""># get list of MININT* computers
$Computers = Get-ADComputer -Filter "Name -like 'MININT*'"

# get serial numbers for each computer
ForEach ($x in $Computers) {
	$colItems = Get-WmiObject Win32_BIOS -Namespace “root\CIMV2" -computername $x.DNSHostName -ErrorAction SilentlyContinue

	if ($colItems -ne $null) {
		ForEach($objItem in $colItems) { 
			Write-Host $x.Name "=" $objItem.SerialNumber
		}
	}
}
</pre>