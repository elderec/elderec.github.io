---
title: 'Powershell &#8211; Get Installed Microsoft Outlook Version with WMI'
author: Jonathan
layout: post
permalink: /2011/12/powershell-get-installed-microsoft-outlook-version-with-wmi/
categories:
  - Powershell
  - Scripting
tags:
  - Powershell
  - Scripting
  - Windows
---
This script is tailored to determine the current version of Outlook 2003, however you can adjust the version numbers to locate other versions of Outlook without much work.

<pre class="brush: powershell; title: ; notranslate" title="">$outlook11Versions = @("11.0.5608.5606","11.0.5608.5703","11.6359.6360","11.6568.6568")

$q = New-Object System.Management.ObjectQuery
$q.QueryString = "Select * from Win32_Product Where Caption Like '%Microsoft Office%'"
$s = New-Object System.Management.ManagementObjectSearcher($q)

$s.Get() | ForEach-Object {
	if ($_.Name -like "*Outlook*") {

		# check for outlook 2003
		for ($i=0; $i -lt $outlook11Versions.Length; $i++) {
			if ($_.Version -eq $outlook11Versions[$i]) {
				$old = $True
				$found = "Outlook 2003 " + $_.Version
			}
		}
	}
}

if ($old) {
	Write-Host $found -ForegroundColor Red
}
else {
	Write-Host "newer than 2003" -ForegroundColor Green
}
</pre>