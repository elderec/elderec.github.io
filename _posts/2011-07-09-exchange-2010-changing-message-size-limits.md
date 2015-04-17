---
title: 'Exchange 2010 &#8211; Changing message size limits'
author: Jonathan
layout: post
permalink: /2011/07/exchange-2010-changing-message-size-limits/
categories:
  - Exchange
  - Microsoft
tags:
  - Exchange
  - Exchange 2010
  - Microsoft
---
There are three places message size limits can be set. At the organizational level, on send connectors, and on receive connectors. 

**Determine the current message size limits:**

  1. Open the Exchange Management Shell
  2. Run the following commands:  
      * Get organizational settings: <pre class="brush: powershell; title: ; notranslate" title="">Get-TransportConfig | Format-List -Property MaxReceiveSize, MaxSendSize
</pre>
    
      * Send connector settings: <pre class="brush: powershell; title: ; notranslate" title="">Get-SendConnector | Format-List -Property Identity, MaxMessageSize
</pre>
    
      * Receive connector settings: <pre class="brush: powershell; title: ; notranslate" title="">Get-ReceiveConnector | Format-List -Property Identity, MaxMessageSize
</pre>

**Change the message size limits:**

  1. Open the Exchange Management Shell
  2. Run the following commands, these examples up the limits to 100mb  
      * Setting at the organizational level: <pre class="brush: powershell; title: ; notranslate" title="">Set-TransportConfig -MaxReceiveSize 100MB -MaxSendSize 100MB
</pre>
        
          * Setting at the send connector: <pre class="brush: powershell; title: ; notranslate" title="">Set-SendConnector -Identity "send connector name" -MaxMessageSize 100MB
</pre>
        
          * Setting at the receive connector: <pre class="brush: powershell; title: ; notranslate" title="">Set-ReceiveConnector -Identity "receive connector name" -MaxMessageSize 100MB
</pre></ul> </ol>