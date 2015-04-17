---
title: 'Powershell &#8211; Change Exchange Alias to Match SamAccountName'
author: Jonathan
layout: post
permalink: /2013/06/powershell-change-exchange-alias-to-match-samaccountname/
dsq_thread_id:
  - 3684415649
categories:
  - Active Directory
  - Exchange
  - Powershell
  - Scripting
tags:
  - Active Directory
  - Exchange
  - Powershell
  - Scripting
---
Quick Powershell snippet to modify a users Exchange Alias to match the the Active Directory SamAccountName. This came in handy when deploying Airwatch using the {EmailUserName} variables to configure Exchange properties in profiles.

<pre class="brush: powershell; title: ; notranslate" title="">$mboxes = Get-Mailbox -OrganizationalUnit contoso.com/users/sales

foreach ($m in $mboxes) {
	$m | Set-Mailbox -Alias $m.SamAccountName
}
</pre>