---
title: 'Powershell &#8211; Backing Up Group Policy'
author: Jonathan
layout: post
permalink: /2012/09/powershell-backing-up-group-policy/
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
  - 3720352009517
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3720352009517" target="_blank">your Facebook Timeline</a>";s:5:"error";b:0;}}'
categories:
  - Active Directory
  - Group Policy
  - Powershell
  - Scripting
  - Windows
tags:
  - Active Directory
  - GPO
  - Group Policy
  - Powershell
  - Scripting
---
Powershell script to backup ALL Group Policy Objects to a network share complete with email notification.

<pre class="brush: powershell; title: ; notranslate" title=""># import the Group Policy module
if (-not (Get-Module GroupPolicy)){
	Import-Module GroupPolicy -ErrorAction Stop            
}
# remove backups older than 7 days
$max_days = "-7"
  
# get the current date
$curr_date = Get-Date
 
# determine how far back we go based on current date
$del_date = $curr_date.AddDays($max_days)

# set the backup path
$backupRoot = "\\server.contoso.com\backups\group-policy"

# set the email options
$smtpServer = 'mail.contoso.com'
$smtpPort = '25'
$fromAddy = 'noreply@contoso.com'
$toAddy = 'notify@contoso.com'
$mailMsg = "GPO Backup for $curr_date complete. Backups saved in $backupRoot\$((get-date).toString('MM-dd-yyyy'))"
$mailSubject = "GPO Backup $curr_date"
 
# create the folder for todays date
md "$backupRoot\$((get-date).toString('MM-dd-yyyy'))"

# backup the GPOs
Backup-Gpo -All -Path "$backupRoot\$((get-date).toString('MM-dd-yyyy'))"

# delete the files
Get-ChildItem $backupRoot -Recurse | Where-Object { $_.LastWriteTime -lt $del_date } | Remove-Item

# send an email stating it was backed up
Send-MailMessage -SmtpServer $smtpServer -From $fromAddy -To $toAddy -Body $mailMsg -Subject $mailSubject
</pre>