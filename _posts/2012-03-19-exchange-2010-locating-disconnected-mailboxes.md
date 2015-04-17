---
title: 'Exchange 2010 &#8211; Locating Disconnected Mailboxes'
author: Jonathan
layout: post
permalink: /2012/03/exchange-2010-locating-disconnected-mailboxes/
categories:
  - Exchange
  - Microsoft
  - Powershell
  - Scripting
tags:
  - Exchange
  - Exchange 2010
  - Exchange 2010 SP1
  - Powershell
  - Scripting
---
Here is a quick Powershell function to make finding disconnected mailboxes easier.

Save this as Get-DisconnectedMailbox.ps1:

<pre class="brush: powershell; title: ; notranslate" title="">function Get-DisconnectedMailbox {
    [CmdletBinding()]
    param(
        [Parameter(Position=0, Mandatory=$false)]
        [System.String]
        $Name = '*'
    )

    $mailboxes = Get-MailboxServer
    $mailboxes | %{
        $disconn = Get-Mailboxstatistics -Server $_.name | ?{ $_.DisconnectDate -ne $null }
        $disconn | ?{$_.displayname -like $Name} |
            Select DisplayName,
            @{n="StoreMailboxIdentity";e={$_.MailboxGuid}},
            Database
    }
}
</pre>

Open the Powershell console, and dot source the function, assuming PS1 is stored in c:\scripts

<pre class="brush: powershell; title: ; notranslate" title="">cd c:\scripts
. .\Get-DisconnectedMailbox.ps1
Get-DisconnectedMailbox mailserver1.contoso.com
</pre>