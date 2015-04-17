---
title: 'Exchange 2010 &#8211; Determine Mailbox Sizes With Powershell'
author: Jonathan
layout: post
permalink: /2012/03/exchange-2010-determine-mailbox-sizes-with-powershell/
categories:
  - Exchange
  - Microsoft
  - Powershell
  - Scripting
tags:
  - Exchange 2010
  - Exchange 2010 SP1
  - Microsoft
  - Powershell
  - Scripting
---
Below is a quick snippet for retrieving mailboxes sizes for all mailboxes in a specific mailbox database:

<pre class="brush: powershell; title: ; notranslate" title="">Get-MailboxDatabase -Identity "EXAMPLE" | Get-MailboxStatistics | where {$_.ObjectClass –eq “Mailbox”} | Sort-Object TotalItemSize –Descending | ft @{label=”User”;expression={$_.DisplayName}},@{label=”Total Size (MB)”;expression={$_.TotalItemSize.Value.ToMB()}},@{label=”Items”;expression={$_.ItemCount}} -auto</pre>