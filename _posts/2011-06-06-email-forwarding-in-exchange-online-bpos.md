---
title: Email forwarding in Exchange Online BPOS
author: Jonathan
layout: post
permalink: /2011/06/email-forwarding-in-exchange-online-bpos/
categories:
  - Microsoft Online
tags:
  - Email
  - Exchange
  - Microsoft
  - Microsoft Online
---
  * Download & Install .NET Framework 2.0 &#8211; <http://bit.ly/kKD3ne>
  * Download & Install Powershell 1.0 &#8211; [http://bit.ly/kkc3aH][1]
  * Download & Install Microsoft Online Migration Tools &#8211; <http://bit.ly/l0T3He>

Open the &#8220;Migration Command Shell&#8221; and follow the steps outlined below.

**Provide Microsoft Online administration credentials:**  
`$AdminCreds = get-credential`

**Setup forwarding:**  
`Set-MSOnlineAlternateRecipient -DeliverToBoth $True -Identity username@emaildomain.com -AlternateRecipient username@emaildomain.com -Credential $AdminCreds`

 [1]: http://bit.ly/kKD3ne