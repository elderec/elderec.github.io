---
title: 'Powershell &#8211; Send-PushoverNotification &#8211; Sending Pushover Notifications via Powershell'
author: Jonathan
layout: post
permalink: /2014/05/powershell-send-pushovernotification-sending-pushover-notifications-via-powershell/
categories:
  - Powershell
  - Scripting
tags:
  - Notification
  - Powershell
  - Scripting
---
<a href="https://pushover.net/" title="Pushover" target="_blank">Pushover </a>makes it easy to get real-time notifications on your Android device, iPhone, iPad, and Desktop. Below is a Powershell function utilizing the <a href="http://technet.microsoft.com/en-us/library/hh849971.aspx" title="Invoke-RestMethod" target="_blank">Invoke-RestMethod</a> Powershell cmdlet to make it easier to send notifications from Powershell scripts.

<pre class="brush: powershell; title: ; notranslate" title="">function Send-PushoverNotification() {
    &lt;#
    .SYNOPSIS
    Pushover makes it easy to get real-time notifications on your Android device, iPhone, iPad, and Desktop. 

    .DESCRIPTION
    Pushover uses a simple REST API to receive messages from your application and send them to devices running our device clients.
    
    .PARAMETER Token
    (required) - your application's API token
    
    .PARAMETER User
    (required) - the user/group key (not e-mail address) of your user (or you), viewable when logged into the pushover dashboard
    
    .PARAMETER message
    (required) - Your message
    
    .PARAMETER priority
    Send as -1 to always send as a quiet notification, 1 to display as high-priority and bypass the user's quiet hours, or 2 to also require confirmation from the user
    
    .PARAMETER device
    Your user's device name to send the message directly to that device, rather than all of the user's devices
    
    .PARAMETER title
    Your message's title, otherwise your app's name is used
    
    .PARAMETER url
    A supplementary URL to show with your message
    
    .PARAMETER url_title
    A title for your supplementary URL, otherwise just the URL is shown
    
    .PARAMETER timestamp
    A Unix timestamp of your message's date and time to display to the user, rather than the time your message is received by our API
    
    .PARAMETER sound
    The name of one of the sounds supported by device clients to override the user's default sound choice
    
    .EXAMPLE
    Send-PushoverNotification -token 'xxxxxxxxxxxxxx' -user 'xxxxxxxxxxxxxxxx' -message 'regular message goes here'

    .EXAMPLE
    Send-PushoverNotification -token 'xxxxxxxxxxxxxx' -user 'xxxxxxxxxxxxxxxx' -message 'important message' -priority 1 

    .EXAMPLE
    Send-PushoverNotification -token 'xxxxxxxxxxxxxx' -user 'xxxxxxxxxxxxxxxx' -message 'emergency message' -priority 2 -url 'http://site.contoso.com'  
    
    .LINK
    Pushover API Documentation: https://pushover.net/api

    .LINK
    Invoke-RestMethod Technet Article: http://technet.microsoft.com/en-us/library/hh849971.aspx
    #&gt;

    param(
        [Parameter(Mandatory=$True)][string]$token,
        [Parameter(Mandatory=$True)][string]$user,
        [Parameter(Mandatory=$True)][string]$message,
        [Parameter(Mandatory=$False)][int]$priority = '0',
        [Parameter(Mandatory=$False)][string]$device,
        [Parameter(Mandatory=$False)][string]$title,
        [Parameter(Mandatory=$False)][string]$url,
        [Parameter(Mandatory=$False)][string]$url_title,
        [Parameter(Mandatory=$False)][string]$timestamp,
        [Parameter(Mandatory=$False)][string]$sound
    )
  
    # build the notification    
    $notification = @{}
    $psboundparameters.GetEnumerator() | % { 
        $notification.Add($($_.key), $($_.value))
    }
    
    # send the notification
    $result = Invoke-RestMethod -Uri 'https://api.pushover.net/1/messages.json' -Body $notification -Method Post -ErrorAction SilentlyContinue
    
    return $result
}
</pre>