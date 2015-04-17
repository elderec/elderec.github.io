---
title: 'Scripting &#8211; Push OpenFire Spark Client spark.properties file to multiple machines'
author: Jonathan
layout: post
permalink: /2012/09/scripting-push-openfire-spark-client-spark-properties-file-to-multiple-machines/
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
  - 3697964009831
fb_status_messages:
  - 'a:1:{i:0;a:2:{s:7:"message";s:100:"Posted to <a href="http://www.facebook.com/3697964009831" target="_blank">your Facebook Timeline</a>";s:5:"error";b:0;}}'
categories:
  - Scripting
  - Windows
tags:
  - Batch
  - Microsoft
  - Scripting
  - Windows
---
The batch script below can be used to copy the spark.properties for the [OpenFire][1] [Spark IM Client][2] file over to new machines on startup/logon. The batch script will copy/create the necessary structure on both Windows 7 and Windows XP machines.

<pre class="brush: plain; title: ; notranslate" title="">@echo off
cls

:: set the location of the spark.properties file
set sparkLocation=\\server.contoso.com\software$\spark.properties

:: determine OS version
ver | findstr /i "5\.1\." &gt; nul
IF %ERRORLEVEL% EQU 0 GOTO ver_XP
ver | findstr /i "6\.1\." &gt; nul
IF %ERRORLEVEL% EQU 0 GOTO ver_Win7

:ver_Win7
:: windows 7, check to see if properties file exists
IF EXIST %HOMEPATH%\AppData\Roaming\Spark\spark.properties GOTO alreadyThere7
IF NOT EXIST %HOMEPATH%\AppData\Roaming\Spark\spark.properties GOTO notThere7
GOTO end

:alreadyThere7
:: already there, copy over
copy /Y %sparkLocation% %HOMEPATH%\AppData\Roaming\Spark\spark.properties
goto end

:notThere7
:: not there, make directory and copy over
md %HOMEPATH%\AppData\Roaming\Spark
copy /Y %sparkLocation% %HOMEPATH%\AppData\Roaming\Spark\spark.properties
goto end

:ver_XP
:windows xp check to see if file exists
IF EXIST %USERPROFILE%\Local Settings\Application Data\Spark\spark.properties GOTO alreadyThereX
IF NOT EXIST %USERPROFILE%\Local Settings\Application Data\Spark\spark.properties GOTO notThereX
GOTO end

:alreadyThereX
:: already there, copy over
copy /Y %sparkLocation% %USERPROFILE%\Local Settings\Application Data\Spark\spark.properties
goto end

:notThereX
:: not there, make directory and copy over
md %USERPROFILE%\Local Settings\Application Data\Spark\spark.properties
copy /Y %sparkLocation% %USERPROFILE%\Local Settings\Application Data\Spark\spark.properties
goto end

:end
</pre>

 [1]: http://www.igniterealtime.org/projects/openfire/
 [2]: http://www.igniterealtime.org/projects/spark/