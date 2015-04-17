---
title: 'Powershell &#8211; Find All Users Who Report To Specific Manager'
author: Jonathan
layout: post
permalink: /2012/11/powershell-find-all-users-who-report-to-specific-manager/
categories:
  - Active Directory
  - Microsoft
  - Powershell
  - Scripting
  - Windows
tags:
  - Active Directory
  - Powershell
  - Scripting
  - Windows
---
Quick Powershell One-Liner to find users who report to a specific person.

<pre class="brush: powershell; title: ; notranslate" title="">Get-ADUser -Filter { Manager -eq "CN=Some Manager,OU=Users,DC=contoso,DC=com" } -Properties telephoneNumber | ft Name, telephoneNumber
</pre>