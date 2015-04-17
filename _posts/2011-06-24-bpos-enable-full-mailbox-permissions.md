---
title: 'BPOS: Enable Full Mailbox Permissions'
author: Jonathan
layout: post
permalink: /2011/06/bpos-enable-full-mailbox-permissions/
categories:
  - Microsoft
  - Microsoft Online
tags:
  - BPOS
  - Exchange
  - Microsoft
  - Microsoft Online
---
Enable Mailbox Sharing and Delegation using the Add-MSOnlineMailPermission powershell cmdlet. I went over configuring powershell for BPOS in my <a href="http://elderec.org/2011/06/email-forwarding-in-exchange-online-bpos/" alt="Email forwarding in Exchange Online BPOS">BPOS in my Email forwarding in Exchange Online BPOS</a> post.

This command grants admin@example.com (TrustedUser) full access and send as permissions on user@example.com’s (Identity) mailbox using the $cred credentials to perform the action.

$cred will be your BPOS administration user credentials

<pre>$cred = get-credential
Add-MSOnlineMailPermission -Identity user@example.com -Credential $cred -TrustedUser admin@example.com –GrantFullAccess True –GrantSendAs True
</pre>