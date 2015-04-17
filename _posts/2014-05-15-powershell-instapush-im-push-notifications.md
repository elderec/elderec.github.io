---
title: 'Powershell &#8211; Instapush.im push notifications'
author: Jonathan
layout: post
permalink: /2014/05/powershell-instapush-im-push-notifications/
categories:
  - Powershell
  - Scripting
tags:
  - Android
  - ios
  - Mobile
  - Notification
  - Powershell
  - Scripting
---
Powershell function to send push notifications to iPhone, iPad, or Android devices using the <a href="https://instapush.im/" title="Instapush" target="_blank">Instapush</a> notification service. Utilizes the <a href="http://technet.microsoft.com/en-us/library/hh849971.aspx" title="Invoke-RestMethod" target="_blank">Invoke-RestMethod</a> and <a href="http://technet.microsoft.com/en-us/library/hh849922.aspx" title="ConvertTo-Json" target="_blank">ConvertTo-Json</a> cmdlets.

<pre class="brush: powershell; title: ; notranslate" title="">function Send-InstapushNotification() {
    &lt;#
    .SYNOPSIS
    Instapush makes it easy to get real-time notifications on your Android device, iPhone, and iPad
 
    .DESCRIPTION
    Instapush allows you to issue an http request, and have a notification delivered to your device.
     
    .PARAMETER applicationID
    (required) - your apps application ID
     
    .PARAMETER applicationSecret
    (required) - your application secret
     
    .PARAMETER pushArray
    (required) - An array containing your event and tracker information

    .EXAMPLE
    $trackers = @{email='rabble'}
    $push = @{event='test'; trackers=$trackers}
    Send-InstapushNotification -applicationID xxxxxxxxxxxx -applicationSecret xxxxxxxxxxxx -pushArray $push
     
    .LINK
    InstaPush API Documentation: https://instapush.im/developer/rest
 
    .LINK
    Invoke-RestMethod Technet Article: http://technet.microsoft.com/en-us/library/hh849971.aspx

    .LINK
    ConvertTo-Json Technet Article: http://technet.microsoft.com/en-us/library/hh849922.aspx

    #&gt;
 
    param(
        [Parameter(Mandatory=$True)][string]$applicationID,
        [Parameter(Mandatory=$True)][string]$applicationSecret,
        [Parameter(Mandatory=$True)][array]$pushArray
    )

    # build the notification    
    $httpHeaders = @{}
    $httpHeaders.Add('x-instapush-appid',$applicationID)
    $httpHeaders.Add('x-instapush-appsecret',$applicationSecret)
    $httpHeaders.Add('Content-Type','application/json')
           
    # send the notification
    $result = Invoke-RestMethod -Uri 'https://api.instapush.im/v1/post' -Headers $httpHeaders -Body ($pushArray | ConvertTo-Json -Compress) -Method Post -ErrorAction SilentlyContinue
     
    return $result
}
</pre>