---
title: 'Exchange 2010 &#8211; Find Mailboxes That Have Forwarding or Redirects Configured'
author: Jonathan
layout: post
permalink: /2013/04/exchange-2010-find-mailboxes-that-have-forwarding-or-redirects-configured/
dsq_thread_id:
  - 3684415470
categories:
  - Exchange
  - Microsoft
  - Powershell
  - Uncategorized
tags:
  - Exchange
  - Exchange 2010
  - Microsoft
  - Powershell
  - Scripting
---
Get list of users who have forwarding enabled on their accounts:

<pre class="brush: powershell; title: ; notranslate" title="">Get-Mailbox -Filter { ForwardingAddress -like "*" } | Where-Object { $_.ForwardingAddress -like "*" } | Select-Object Name,ForwardingAddress
</pre>

Get list of users who have forwarding rules configured in their mailboxes:

<pre class="brush: powershell; title: ; notranslate" title="">ForEach ($m in (Get-Mailbox -ResultSize Unlimited)) { Get-InboxRule -Mailbox $m.DistinguishedName | where { $_.ForwardTo } | fl MailboxOwnerID,Name,ForwardTo }
</pre>

Get list of users who have redirects configured on their mailboxes:

<pre class="brush: powershell; title: ; notranslate" title="">ForEach ($m in (Get-Mailbox -ResultSize Unlimited)) { Get-InboxRule -Mailbox $m.DistinguishedName | where {$_.ReDirectTo} | fl MailboxOwnerID,Name,RedirectTo }
</pre>