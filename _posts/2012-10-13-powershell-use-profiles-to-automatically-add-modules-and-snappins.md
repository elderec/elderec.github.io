---
title: 'Powershell &#8211; Use Profiles to Automatically Add Modules and Snappins'
author: Jonathan
layout: post
permalink: /2012/10/powershell-use-profiles-to-automatically-add-modules-and-snappins/
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
  - 3804501033190
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3804501033190" target="_blank">your Facebook Timeline</a>";s:5:"error";b:0;}}'
dsq_thread_id:
  - 3684415403
categories:
  - Powershell
tags:
  - Active Directory
  - Exchange
  - Exchange 2010 SP1
  - Powershell
  - Scripting
  - SCVMM
  - System Center Virtual Machine Manager
  - Windows
---
Automatically run commands when Powershell launches by using Powershell <a href="http://msdn.microsoft.com/en-us/library/windows/desktop/bb613488(v=vs.85).aspx" title="Windows PowerShell Profiles" target="_blank">profiles</a>. Kind of like .bashrc in *nix.

Open Powershell and create a profile:

<pre class="brush: powershell; title: ; notranslate" title="">New-Item -path $profile -type file –force
notepad $profile
</pre>

Add the commands you want to run on startup. I load the Active Directory, Exchange 2010, Systems Center Virtual Machine Manager, and Quest ActiveRoles cmdlets:

<pre class="brush: powershell; title: ; notranslate" title="">Get-Module -Name ActiveDirectory
Add-PSSnapin -Name Microsoft.Exchange.Management.PowerShell.E2010
Add-PSSnapin -Name Microsoft.Exchange.Management.Powershell.Support
Add-PSSnapin -Name Microsoft.SystemCenter.VirtualMachineManager 
Add-PSSnapin -Name Quest.ActiveRoles.ADManagement
</pre>

Save it. Now the next time you open Powershell the commands in this profile script will run.

You can get a list of modules / snappins in your Powershell instance running:

<pre class="brush: powershell; title: ; notranslate" title="">Get-PSSnapin -Registered
Get-Module -ListAvailable 
</pre>