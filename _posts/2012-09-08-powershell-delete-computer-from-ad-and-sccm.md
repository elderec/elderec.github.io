---
title: 'Powershell &#8211; Delete Computer from AD and SCCM'
author: Jonathan
layout: post
permalink: /2012/09/powershell-delete-computer-from-ad-and-sccm/
fb_social_plugin_settings_box_like:
  - default
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
  - 3683114358599
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3683114358599" target="_blank">your Facebook Timeline</a>";s:5:"error";b:0;}}'
dsq_thread_id:
  - 3684415323
categories:
  - Active Directory
  - Microsoft
  - Powershell
  - SCCM
  - Scripting
  - Windows
tags:
  - Active Directory
  - Microsoft
  - Powershell
  - SCCM
  - Scripting
  - Windows
---
Powershell script to delete computer account from active directory and remove the computer object from SCCM. Threw this together after having to delete/remove 10+ computers in a single sitting. Much quicker than using the GUI.

<pre class="brush: powershell; title: ; notranslate" title="">&lt;#
.SYNOPSIS
    Deletes computer from SCCM and AD
.DESCRIPTION
    Queries AD & SCCM, deletes the computer account from AD, and removes the computer object from SCCM 
.NOTES
    Author: Jonathan - jon@elderec.org 
.LINK 
    http://elderec.org
.PARAMETER computerName
	Name of computer to delete from AD/SCCM
.PARAMETER sccmServer
	Name of the SCCM server to use
.PARAMETER sccmSite
	Name of the SCCM site to use
.EXAMPLE
	.\delcomp-adsccm.ps1 -computerName CON-01337
	.\delcomp-adsccm.ps1 -computerName CON-01337 -sccmServer sccm.contoso.com
	.\delcomp-adsccm.ps1 -computerName CON-01337 -sccmServer sccm.contoso.comm -sccmSite YOURSITE
#&gt; 

param (
	[parameter(Mandatory=$true, HelpMessage="Enter a computer name")][string]$computerName,
	[parameter(Mandatory=$false, HelpMessage="Enter SCCM server")][string]$sccmServer='sccm-server.contoso.com',
	[parameter(Mandatory=$false, HelpMessage="Enter SCCM server")][string]$sccmSite='YOURSITE'
)

# find and delete the computer from AD
$dom = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
$root = $dom.GetDirectoryEntry()
$search = [System.DirectoryServices.DirectorySearcher]$root
$search.filter = "(&(objectclass=computer)(name=$computerName))"
$search.findall() | %{$_.GetDirectoryEntry() } | %{$_.DeleteObject(0)}

# find and delete from SCCM
$comp = get-wmiobject -query "select * from SMS_R_SYSTEM WHERE Name='$computerName'" -computername $sccmServer -namespace "ROOT\SMS\site_$sccmSite"
$comp.psbase.delete()

# spit out results
Write-Host "Deleted $computerName from AD. Removed $computerName from SCCM server $sccmServer, site $sccmSite"
</pre>