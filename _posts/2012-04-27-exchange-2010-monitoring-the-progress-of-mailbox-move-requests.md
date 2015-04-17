---
title: 'Exchange 2010 &#8211; Monitoring The Progress of Mailbox Move Requests'
author: Jonathan
layout: post
permalink: /2012/04/exchange-2010-monitoring-the-progress-of-mailbox-move-requests/
categories:
  - Exchange
  - Microsoft
  - Powershell
tags:
  - Exchange
  - Exchange 2010 SP1
  - Microsoft
  - Powershell
  - Scripting
---
In Exchange 2010 mailbox moves are asynchronous and are performed by the Microsoft Exchange Mailbox Replication service (MRS). Unfortunately there is no built in way to watch the mailbox move complete with a progress bar. Below is a Powershell script that will present a status bar for the mailbox move.

<pre class="brush: powershell; title: ; notranslate" title="">&lt;#
.SYNOPSIS
    Detailed stats on a single mailbox move 
.DESCRIPTION
	Watch the status of a single mailbox move
.NOTES
    Author: elderec.org
.LINK 
    http://elderec.org
.PARAMETER Identity
	Identity of Mailbox Move Request
.EXAMPLE
	.\Watch-MailboxMove.ps1 -Identity "John Q. User"
#&gt;
param (
	[parameter(Mandatory=$true, HelpMessage="Enter the Identity of the active move request.")][string]$Identity,
	[parameter(Mandatory=$false, HelpMessage="Which Domain Controller are we using?")][string]$DomainController
)

do { 
	$mbMove = Get-MoveRequestStatistics -Identity $Identity -DomainController $DomainController
	$stat = [string]$mbMove.BytesTransferred + " - " + [string]$mbMove.ItemsTransferred + " of " + [string]$mbMove.TotalMailboxItemCount + " items."
	$act = "Moving " + $mbMove.DisplayName + "'s mailbox | " + [string]$mbMove.OverallDuration + " | " + $mbMove.PercentComplete + "% complete"
	Write-Progress -activity $act -Status $stat -percentComplete $mbMove.PercentComplete
}
Until ( $mbMove.Status -eq "Completed" )
</pre>