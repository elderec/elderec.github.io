---
title: 'Powershell &#8211; Send PushBullet Notifications from PRTG'
author: Jonathan
layout: post
permalink: /2014/05/powershell-send-pushbullet-notifications-from-prtg/
dsq_thread_id:
  - 3684415627
categories:
  - Powershell
  - Scripting
tags:
  - Microsoft
  - Notification
  - Powershell
  - PRTG
  - Scripting
  - Windows
---
Powershell v3+ script to send notifications using the <a href="https://www.pushbullet.com/" title="Pushbullet" target="_blank">Pushbullet</a> notification service. The script will determine all available devices based on the provided API keys and send the notification to all of them. Adding multiple API keys will result in the notification being sent to those users as well.

<pre class="brush: powershell; title: ; notranslate" title=""># specify the pushbullet api key(s)
$pushbulletApiKeys = @('xxxxxxxxxxxxxxxxxxxxxxxxx')

# build the message from the arguments passed by PRTG
for ($i=0; $i -lt $args.count; $i++) {
	$message+="$($args[$i]) "
}

# function to pushbullet notifications
function sendPushBulletNotification($apiKey, $message) {

    # convert api key into PSCredential object
    $credentials = New-Object System.Management.Automation.PSCredential ($apiKey, (ConvertTo-SecureString $apiKey -AsPlainText -Force))

    # get list of registered devices
    $pushDevices = Invoke-RestMethod -Uri 'https://api.pushbullet.com/api/devices' -Method Get -Credential $cred

    # loop through devices and send notification
    foreach ($device in $pushDevices.devices) {

        # build the notification
        $notification = @{
            device_iden = $device.iden
            type = 'note'
            title = 'PRTG Alert'
            body = $message
        }

        # push the notification
        Invoke-RestMethod -Uri 'https://api.pushbullet.com/api/pushes' -Body $notification -Method Post -Credential $credentials
    }
}

# send the notification(s)
foreach ($apiKey in $pushbulletApiKeys) {
    sendPushBulletNotification $apiKey $message
}
</pre>