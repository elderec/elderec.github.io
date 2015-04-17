---
title: 'Exchange 2010 &#8211; Moving the Discovery Search Mailbox'
author: Jonathan
layout: post
permalink: /2011/10/exchange-2010-moving-the-discovery-search-mailbox/
dsq_thread_id:
  - 3684414971
categories:
  - Exchange
  - Microsoft
  - Powershell
tags:
  - Exchange
  - Exchange 2010
  - Powershell
  - Windows
---
Moving the Discovery Search Mailbox in Exchange 2010 is a relatively simple process. Chances are you are trying to remove the default database created by Exchange 2010 setup and it won&#8217;t let you because of this mailbox. Finding and moving the arbitration mailbox is outlined below.

**Get a list of arbitration mailboxes:**

<pre class="brush: powershell; title: ; notranslate" title="">Get-Mailbox -Arbitration</pre>

**Move the arbitration mailboxes:**

<pre class="brush: powershell; title: ; notranslate" title="">Get-Mailbox -Arbitration -Database db1 | New-MoveRequest -TargetDatabase db2</pre>

![Exchange 2010 - Moving Discovery Search Mailboxes][1]

 [1]: http://img.elderec.org/ss/2011/10/sst-2011-10-06_12.24.44.png