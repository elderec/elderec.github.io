---
title: 'Exchange 2010 &#8211; Add Calendar Permissions to Members of AD Group'
author: Jonathan
layout: post
permalink: /2012/05/exchange-2010-add-calendar-permissions-to-members-of-ad-group/
dsq_thread_id:
  - 3684415158
categories:
  - Exchange
  - Microsoft
  - Powershell
  - Scripting
tags:
  - Active Directory
  - Exchange
  - Exchange 2010 SP1
  - Microsoft
  - Powershell
  - Scripting
---
I was recently tasked with adding calendar permissions for the members of an AD group to a specific users mailbox. The following Powershell snippet will grant &#8220;Editor&#8221; permissions on the &#8220;someuser&#8221; calendar for the members of the AD group named &#8220;Some Group&#8221;.

This snippet makes use of the <a href="http://technet.microsoft.com/en-us/library/ee617193.aspx" alt="Get-ADGroupMember">Get-ADGroupMember</a> and the <a href="http://technet.microsoft.com/en-us/library/bb123685.aspx" alt="Get-Mailbox">Get-Mailbox</a> cmdlets.

<pre class="brush: powershell; title: ; notranslate" title="">Get-ADGroupMember -Identity "Some Group" | ForEach-Object { 
	$mb = Get-Mailbox -Identity $_.distinguishedName
	Add-MailboxFolderPermission -Identity "someuser:\Calendar" -User $mb.Alias -AccessRights Editor
}
</pre>