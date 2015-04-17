---
title: 'Powershell &#8211; Bulk User Password Resets'
author: Jonathan
layout: post
permalink: /2012/09/powershell-bulk-user-password-resets/
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
  - 3703446266884
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3703446266884" target="_blank">your Facebook Timeline</a>";s:5:"error";b:0;}}'
dsq_thread_id:
  - 3684415336
categories:
  - Active Directory
  - Microsoft
  - Powershell
  - Scripting
  - Windows
tags:
  - Active Directory
  - Microsoft
  - Powershell
  - Scripting
  - Windows
---
Simple Powershell script to bulk reset passwords from a text file containing one user per line. This makes use of the <a href="http://technet.microsoft.com/en-us/library/ee617241.aspx" title="Get-ADUser Syntax" target="_blank">Get-ADUser</a>, <a href="http://technet.microsoft.com/en-us/library/ee617215.aspx" title="Set-ADUser Syntax" target="_blank">Set-ADUser</a>, and <a href="http://technet.microsoft.com/en-us/library/ee617261.aspx" title="Set-ADAccountPassword Syntax" target="_blank">Set-ADAccountPassword</a> Powershell active directory cmdlets.

<pre class="brush: powershell; title: ; notranslate" title=""># import the AD module
if (-not (Get-Module ActiveDirectory)){
	Import-Module ActiveDirectory -ErrorAction Stop            
}

# set new default password
$password = ConvertTo-SecureString -AsPlainText "Password01" -Force  

# get list of account names (1 per line)
$list = Get-Content -Path c:\scripts\users.txt

# loop through the list
ForEach ($u in $list) {

	if ( -not (Get-ADUser -LDAPFilter "(sAMAccountName=$u)")) { 
		Write-Host "Can't find $u" 
	}
	else { 
		$user = Get-ADUser -Identity $u
		$user | Set-ADAccountPassword -NewPassword $password -Reset
		$user | Set-AdUser -ChangePasswordAtLogon $true
		Write-Host "changed password for $u"
	}
}
</pre>