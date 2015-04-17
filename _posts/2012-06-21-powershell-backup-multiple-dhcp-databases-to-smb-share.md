---
title: 'Powershell &#8211; Backup Multiple DHCP Databases to SMB Share'
author: Jonathan
layout: post
permalink: /2012/06/powershell-backup-multiple-dhcp-databases-to-smb-share/
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
  - 3302670927751
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3302670927751" target="_blank">your Facebook Timeline</a>";s:5:"error";s:0:"";}}'
fb_social_plugin_settings_box_like:
  - default
categories:
  - Microsoft
  - Scripting
  - Windows
tags:
  - Microsoft
  - Powershell
  - Scripting
  - Windows
---
Quick script to backup DHCP databases from various Windows 2008R2 site servers to a central file share. 

<pre class="brush: powershell; title: ; notranslate" title=""># server site codes
$siteCodes = @('PDX','NYC','LAX')

# set backup path
$backupRoot = "\\my-fps.contoso.com\some-share\backups\dhcp"

# copy the DHCP backups over to MY-FPS
ForEach ($x in $siteCodes) {
	$path = "\\$x-ad.contoso.com\c$\windows\system32\dhcp\backup"
	Copy-Item $path -Destination $backupRoot\$x -Recurse -Force -ErrorAction SilentlyContinue
}
</pre>