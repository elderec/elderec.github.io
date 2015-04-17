---
title: 'Exchange 2010 &#8211; Move All Mailboxes From OU To Specific Database'
author: Jonathan
layout: post
permalink: /2012/02/exchange-2010-move-all-mailboxes-from-ou-to-specific-database/
categories:
  - Exchange
  - Microsoft
  - Powershell
tags:
  - Exchange
  - Exchange 2010
  - Exchange 2010 SP1
  - Microsoft
  - Powershell
  - Windows
---
I needed to move all mailboxes for accounts in a specific OU to a specific Mailbox Database. Below are the powershell commands I used to accomplish this.

To create the necessary move requests:

<pre class="brush: powershell; title: ; notranslate" title="">$ou = "constoso.com/account purgatory"
Get-Mailbox -OrganizationalUnit $ou | New-MoveRequest -TargetDatabase "Purgatory"
</pre>

To clean up the move requests when complete:

<pre class="brush: powershell; title: ; notranslate" title="">Get-MoveRequest | Where-Object { $_.Identity -like "$ou*" -and $_.Status -eq "Completed" } | Remove-MoveRequest -Confirm:$false
</pre>