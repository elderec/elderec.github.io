---
title: 'Exchange 2010 &#8211; Watch PST Import Request Progress'
author: Jonathan
layout: post
permalink: /2012/06/exchange-2010-watch-pst-import-request-progress/
fb_mentioned_pages:
  - 'a:0:{}'
fb_mentioned_pages_message:
  - 
fb_mentioned_friends:
  - 'a:0:{}'
fb_mentioned_friends_message:
  - 
fb_author_message:
  - 
fb_author_post_id:
  - 3322270737734
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3322270737734" target="_blank">your Facebook Timeline</a>";s:5:"error";s:0:"";}}'
fb_social_plugin_settings_box_like:
  - default
dsq_thread_id:
  - 3684415184
categories:
  - Exchange
  - Microsoft
  - Powershell
  - Scripting
tags:
  - Exchange 2010
  - Microsoft
  - Powershell
  - Scripting
---
Quick Powershell script to watch the status of import requests in Exchange 2010.

<pre class="brush: powershell; title: ; notranslate" title="">&lt;#
.SYNOPSIS
    Detailed stats on a single PST import request
.DESCRIPTION
	Watch the status of a single PST import request
.NOTES
    Author: Jonathan
.LINK 
    http://elderec.org
.PARAMETER Identity
	Identity of Mailbox Import Request
.EXAMPLE
	.\Watch-ImportRequest.ps1 -Identity "Jon Q. User"
#&gt;
param (
	[parameter(Mandatory=$true, HelpMessage="Enter the Identity of the active move request.")][string]$Identity
)

do { 
	$mbMove = Get-MailboxImportRequestStatistics -Identity $Identity
	$stat = "Importing PST | $Identity"
	$act = "Duration: " + [string]$mbMove.OverallDuration + " | " + $mbMove.PercentComplete + "% complete"
	Write-Progress -activity $act -Status $stat -percentComplete $mbMove.PercentComplete
}
Until ( $mbMove.Status -eq "Completed" )
</pre>