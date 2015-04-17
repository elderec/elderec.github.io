---
title: Setup Anonymous Relay on Exchange 2010 Receive Connector
author: Jonathan
layout: post
permalink: /2011/06/setup-anonymous-relay-on-exchange-2010-receive-connector/
categories:
  - Exchange
  - Microsoft
tags:
  - Exchange
  - Exchange 2010
  - Microsoft
---
I&#8217;ve come across several situations were granting anonymous relay on a Exchange 2010 receive connector can be beneficial. In my environment we use a connector of this type for our multi-function printers/scanners that scan to email. This way we don&#8217;t have to setup mailboxes just for printers/scanners.

**Create your connector:**

<pre class="brush: powershell; title: ; notranslate" title="">New-ReceiveConnector -Name "Anon Relay" -Usage Custom -PermissionGroups AnonymousUsers -Bindings your.exchange.ip.address:25 -RemoteIpRanges allow.from.ip.address
</pre>

**Grant anonymous relay on the new connector:**

<pre class="brush: powershell; title: ; notranslate" title="">Get-ReceiveConnector "Anon Relay" | Add-ADPermission -User "NT AUTHORITY\ANONYMOUS LOGON" -ExtendedRights "Ms-Exch-SMTP-Accept-Any-Recipient"
</pre>

**Add single additional IP to connector:**

<pre class="brush: powershell; title: ; notranslate" title="">$rec = Get-ReceiveConnector "Anon Relay"
$rec.RemoteIPRanges += "new.ip.address"
Set-ReceiveConnector "Anon Relay" -RemoteIPRanges $rec.RemoteIPRanges
</pre>

**Add multiple IP addresses to the connector:**

<pre class="brush: powershell; title: ; notranslate" title="">$rec = Get-ReceiveConnector "Anon Relay"
$rec.RemoteIPRanges += "ip.address", "ip.address", "ip.address"
Set-ReceiveConnector "Anon Relay" -RemoteIPRanges $rec.RemoteIPRanges
</pre>

**Add multiple IP addresses from a text file (one per line) to the connector:**

<pre class="brush: powershell; title: ; notranslate" title="">$rec = Get-ReceiveConnector "Anon Relay"
Get-Content .\ip.txt | foreach { $rec.RemoteIPRanges += "$_" }
Set-ReceiveConnector "Anon Relay" -RemoteIPRanges $rec.RemoteIPRanges
</pre>

You can add additional IP addresses via the Exchange Management Console. The console accepts CIDR addresses, so a single IP would be /32 eg: 192.168.1.25/32. If you are using this for Printers, you won&#8217;t need to specify SMTP credentials, however you still need to specify a &#8220;send from&#8221; address otherwise Exchange will deny the relay.