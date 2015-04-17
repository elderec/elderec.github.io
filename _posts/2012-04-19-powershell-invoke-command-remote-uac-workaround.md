---
title: 'Powershell &#8211; Invoke-Command Remote UAC Workaround'
author: Jonathan
layout: post
permalink: /2012/04/powershell-invoke-command-remote-uac-workaround/
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
I recently had to install a software agent on several remote servers with that had UAC (User Account Control) enabled. The issue was with the way Invoke-Command runs the commands, it uses the credentials of the person/account running the script to authenticate with the server then tries to use another account to run the command the permissions or lowers the privileges of the user so that certain commands cannot be run. I was able to get the software to install using the SYSTEM account by using a work around with the task scheduler. The following snippet creates a scheduled task to run as SYSTEM on the remote server, starts the task, and then deletes the task. In my case all servers follow a common naming convention, however it wouldn&#8217;t be difficult to adapt this snippet to other environments. 

<pre class="brush: powershell; title: ; notranslate" title="">$siteCodes = @('PDX','NYC','LAX')

ForEach ($x in $siteCodes) {
# create PSSession on remote server
$Session = New-PSSession -ComputerName "$x-fs.constoso.com"

# run command on remote server
$Job = Invoke-Command -ComputerName "$x-fs.contoso.com" -ErrorAction SilentlyContinue -ArgumentList $x -ScriptBlock {
param($site)

# get bogus time
$currTime = Get-Date
$currTime.AddMinutes(90)
$currTime = Get-Date $currTime -Format %H:m

# create scheduled task
schtasks /create /tn "SoftInstall" /ru SYSTEM /tr "\\$site-fs.contoso.com\setup.exe /some /switches /go /here" /sc once /st $currTime

# run the task
schtasks /run /tn "SoftInstall"

# sleep for 10 sec
Start-Sleep -s 10

# delete the task
schtasks /delete /f /tn "SoftInstall"
}

# close the PSSession
Remove-PSSession -Session $Session
}
</pre>