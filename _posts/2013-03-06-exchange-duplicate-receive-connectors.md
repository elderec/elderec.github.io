---
title: 'Exchange &#8211; Duplicate Receive Connectors'
author: Jonathan
layout: post
permalink: /2013/03/exchange-duplicate-receive-connectors/
categories:
  - Exchange
  - Microsoft
  - Powershell
tags:
  - Exchange
  - Exchange 2010
  - Microsoft
  - Powershell
  - Scripting
---
Powershell snippet for duplicating receive connectors on new servers. This goes well with my [Setup Anonymous Relay on Exchange 2010 Receive Connector][1] post.

<pre class="brush: powershell; title: ; notranslate" title="">New-ReceiveConnector "Anonymous Connector" -Server HT02 -Bindings 0.0.0.0:25 -RemoteIPRanges ( Get-ReceiveConnector "HT01\Anonymous Connector" ).RemoteIPRanges
</pre>

 [1]: http://elderec.org/2011/06/setup-anonymous-relay-on-exchange-2010-receive-connector/ "Setup Anonymous Relay on Exchange 2010 Receive Connector"