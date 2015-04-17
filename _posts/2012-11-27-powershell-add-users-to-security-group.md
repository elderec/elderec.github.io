---
title: 'Powershell &#8211; Add Users to Security Group'
author: Jonathan
layout: post
permalink: /2012/11/powershell-add-users-to-security-group/
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
Powershell to add users to groups with Get-ADGroup and Add-ADGroupMember cmdlets.

Add single user to security group:

<pre class="brush: powershell; title: ; notranslate" title="">Get-ADGroup -Identity &quot;SomeGroup&quot; | Add-ADGroupMember &quot;some.user&quot;
</pre>

Add multiple users from a text file to security group, one user per line.

<pre class="brush: powershell; title: ; notranslate" title="">$list = Get-Content -Path &quot;c:\tmp\list.txt&quot;
$group = Get-ADGroup &quot;SomeGroup&quot;
ForEach ($user in $list) { Add-ADGroupMember $group $user }
</pre>