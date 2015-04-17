---
title: 'Exchange &#8211; Export Email Addresses to Excel'
author: Jonathan
layout: post
permalink: /2013/04/exchange-export-email-addresses-to-excel/
categories:
  - Exchange
  - Microsoft
  - Powershell
  - Windows
tags:
  - Exchange
  - Exchange 2010
  - Exchange 2010 SP1
  - Microsoft
  - Powershell
  - Scripting
  - Windows
---
Powershell command to export all user email addresses to an Excel sheet. Below uses OrganizationalUnit, you could change the Get-Mailbox criteria to Server, Database, or whatever your requirements.

<pre class="brush: powershell; title: ; notranslate" title="">Get-Mailbox -OrganizationalUnit 'contoso.com/users' -ResultSize Unlimited | Select-Object DisplayName,ServerName,PrimarySmtpAddress, @{Name=“EmailAddresses”;Expression={$_.EmailAddresses | Where-Object {$_.PrefixString -ceq 'smtp'} | ForEach-Object {$_.SmtpAddress}}} | Export-Csv -Path some-file.csv
</pre>